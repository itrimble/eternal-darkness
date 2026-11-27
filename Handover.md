# Handover — Eternal Darkness recomp

Updated 2026-11-23. The running record is `blog/2026-11-20-eternal-darkness-mac/LOG.md`; the blog draft is `post.md` in the same folder. Neither may be published without Ian saying so. This file is the operational summary: what builds, how to build it, what is broken, and what to do next.

## Layout

| Path | What it is |
| --- | --- |
| `src/` | the game repo (host, generated recompiled code, tools) |
| `src/.deps/GXRuntime`, `src/.deps/DolRecomp` | dependency trees, gitignored, patched by `src/patches/*` — edits must be folded into those patches |
| `src/build-mac` | Mac build tree |
| `src/build-tvos` | tvOS build tree |
| `disc/GEDP01.iso` | European disc, 1.4 GB |
| `generated/main.dol` | extracted executable |
| `textures/` | HD texture pack, 7.6 GB |
| `saves/`, `src/ed-GEDP01-slot-a.dolcard` | saves and memory card — never write these from a throwaway run |
| `dist/Eternal Darkness.app` | Mac portable app, 9.1 GB, ad hoc |
| `dist-tvos/Eternal Darkness TV.app` | tvOS app, 1.5 GB lean or 5.2 GiB with the trimmed HD pack |
| `/tmp/tvos-deps` | **ephemeral** tvOS dependency prefix + Dawn source. If it disappears the tvOS build cannot link |

## Mac build

```sh
BUILDFLEET_LOCAL=1 ninja -C src/build-mac -j 10 EternalDarkness
./src/play.sh                       # tome, then the disc
```

Project rule: build locally, do **not** send this project through build fleet.

A throwaway run uses a copy of the card and settings under `/tmp`:

```sh
ED_SETTINGS=/tmp/x/settings.ini ED_NO_BOOT_STATE=1 \
  ./build-mac/EternalDarkness generated/main.dol --iso "$PWD/dump/GEDP01.iso" \
  --no-launcher --card /tmp/x/throwaway-slot-a.dolcard
```

Package: `src/tools/mac/make-portable-app.sh` (refuses to run while the game is running).

## tvOS build

Needs Xcode 27.2 (tvOS 27.2 SDK), the `/tmp/tvos-deps` prefix, and a paired Apple TV: **Shed** (`AppleTV11,1`, A12, 3 GB, tvOS 27.3, 32 GB) or **Living Room** (`AppleTV6,2`, A15, 4 GB, tvOS 26.6.1).

Configure (from `src/`):

```sh
cmake -S . -B build-tvos -G Ninja \
  -DCMAKE_SYSTEM_NAME=tvOS -DCMAKE_OSX_ARCHITECTURES=arm64 \
  -DCMAKE_OSX_SYSROOT=appletvos -DCMAKE_OSX_DEPLOYMENT_TARGET=18.0 \
  -DCMAKE_BUILD_TYPE=Release -DCMAKE_PREFIX_PATH=/tmp/tvos-deps/prefix \
  -DAURORA_SDL3_PROVIDER=system -DSDL3_DIR=/tmp/tvos-deps/prefix/lib/cmake/SDL3 \
  -DAURORA_DAWN_PROVIDER=vendor \
  -DFETCHCONTENT_SOURCE_DIR_DAWN=/tmp/tvos-deps/dawn-266c1cf8de969a364afa4fa49311631fc99a881e \
  -DFETCHCONTENT_SOURCE_DIR_IMGUI=$PWD/build-mac/_deps/imgui-src \
  -DFETCHCONTENT_SOURCE_DIR_RMLUI=$PWD/build-mac/_deps/rmlui-src \
  -DFETCHCONTENT_SOURCE_DIR_XXHASH=$PWD/build-mac/_deps/xxhash-src \
  -Dzstd_DIR=/tmp/tvos-deps/prefix/lib/cmake/zstd \
  -Dfmt_DIR=/tmp/tvos-deps/prefix/lib/cmake/fmt \
  -Dfreetype_DIR=/tmp/tvos-deps/prefix/lib/cmake/freetype \
  -DPNG_DIR=/tmp/tvos-deps/prefix/lib/cmake/PNG \
  -DCMAKE_EXE_LINKER_FLAGS=-L/tmp/tvos-deps/prefix/lib \
  -DDAWN_BUILD_PROTOBUF=OFF
ninja -C build-tvos -j 8 EternalDarkness     # -> build-tvos/EternalDarkness.app/EternalDarkness
```

`DAWN_BUILD_PROTOBUF=OFF` is required when cross-compiling (otherwise it wants a host `protoc`).

Package and install:

```sh
PROFILE=/tmp/tvos-test/build/Build/Products/Debug-appletvos/HelloTV.app/embedded.mobileprovision \
SIGN_IDENTITY="Apple Development: Ian Trimble (X6SA7HFNXJ)" TEAM_ID=CLDQBGZBDU \
  ./src/tools/tvos/make-tvos-app.sh            # INCLUDE_TEXTURES=1 TEXTURES_DIR=… SETTINGS_FILE=… optional
xcrun devicectl device install app --device <apple-tv-udid> "dist-tvos/Eternal Darkness TV.app"
```

Run and observe:

```sh
xcrun devicectl device process launch --console --device 439e1c… \
  -e '{"ED_PERF_LOG":"1","ED_MUTE":"1","ED_QUICKLOAD":"saves/slot1.state"}' com.iantrimble.eternaldarkness
xcrun devicectl device capture screenshot --device 439e1c… --destination /tmp/tv.png
xcrun devicectl device process terminate --device 439e1c… --pid <pid>
```

`ED_PERF_LOG=1` prints `[perf] … fps=… cpu=…% blocks/s=…` every two seconds (CPU is % of one core). `DOL_GX_TIME=1` prints the per-frame breakdown. `ED_PAD_SCRIPT="start,stop,input;…"` drives a scripted pad (`up/down/left/right/south/east/west/north/lb/rb/lt/rt/l3/r3/start/dup/ddown/dleft/dright`), which is how the pad was tested without hands.

Signing: a wildcard "tvOS Team Provisioning Profile" minted by building the throwaway project in `/tmp/tvos-test` (bundle id `com.iantrimble.eternaldarkness`, still present); it expires **2027-10-14**. If it lapses, regenerate that project with `xcodegen generate` and build it with `xcodebuild -allowProvisioningUpdates -destination id=<udid>`.

## What works

- 1080p fullscreen at 59.94 Hz, disc boot, saves, sound.
- The recomp's own tome and Options book (resolution, frame rate, aspect, brightness, audio, controls) — the TV build runs the launcher, unlike earlier builds.
- Layered tvOS icon (`src/res/icon/tvos/Assets.xcassets`, rebuilt from `src/res/icon/tvos-icon-source.png` by `tools/tvos/make-icon-from-image.sh`).
- HD pack: the trimmed variant (no `mip override`) at 7738 textures.
- DualShock 4 as the pad; the Siri Remote is detected but cannot play the game.

## What is broken or unfinished

1. **Static buzz on the game's own options menu**, one burst per menu sound. Repro'd on the TV with no other app running. Not explained. The game's PCM capture was clean at boot, so the next step is to capture *while a menu sound plays*.
2. **Saves land in `Library/Caches`**, not `Documents`: the shim's `Documents` path fails and falls back. Caches can be purged by tvOS.
3. **The `.deps` edits are not in the patches**: tvOS support (MetalBinding, MetalFX stub, aurora link flags, cpu.h inline memory path, window.cpp fullscreen, main.c/SDL_main) exists only in the working tree. A fresh checkout cannot build tvOS.
4. **`/tmp/tvos-deps` is ephemeral** and the build recipe above lives only here.
5. **The full 7.6 GB pack does not fit** a 32 GB Apple TV; the trimmed 5.2 GiB app needs the other apps deleted first.
6. The **lean 1.5 GB build** is what is installed right now; the HD build has to be repackaged with `INCLUDE_TEXTURES=1 TEXTURES_DIR=/tmp/ed-textures-trim`. That trim directory still exists (3.8 GB, `textures/` minus `mip override`, APFS clones) but `/tmp` is volatile — recreate it from `textures/` if it is gone.
7. Living Room has never had the app installed.

## Measurements to trust

- tvOS: 16.683 ms between presents (59.94 Hz); `[texpack] 7738 textures`.
- Per frame in the loaded save: `gx=2.4 ms wait=12.1 ms wplan=3.5 ms submit=1.5 ms present=14.9 ms` of 16.7 ms — 4–6 ms of work, 12 ms of vsync slack.
- CPU: ~1.08 cores idle, 2.8 at the peak of a scripted walk-and-attack, 60 fps throughout.
- The tvOS `cpu_resource` reports ("84–98% average against a 50% policy") come from **background** launches over the network; the app ran 200 s regardless, and a foreground app is not held to that.

## Traps already hit (do not repeat)

- The "4.4 cores in play" figure was this project's own frame-dump keeper thread forcing continuous whole-frame GPU readback. It was measuring the instrumentation.
- The in-app PPM frame dump cannot fire: the backend only reads the frame back while the guest peeks the EFB. Use `devicectl device capture screenshot`.
- An `appiconset` is rejected on tvOS ("unassigned child"); icons need `.brandassets` with a `Content.imageset` inside each `.imagestacklayer`.
- `codesign` refuses a bundle carrying extended attributes (ImageMagick leaves them). The packager runs `xattr -cr` first.
- `devicectl device install app` fails with `Insufficient storage` and reports exact byte counts; free space by deleting apps, nothing else is prunable.
- Launching the app with `devicectl` puts it in the background: tvOS throttles it and the CPU reports fire. Screenshots still work; performance numbers from those runs are a floor.

## Next steps, in order

1. Capture the game's PCM while a menu sound plays, on the TV, and find whether the buzz is in the mix or after it.
2. Get a save made mid-combat and measure that scene with `DOL_GX_TIME` and `ED_PERF_LOG` — the scripted fight is not a boss room.
3. Fold the tvOS and memory-path edits into `src/patches/gxruntime/` and `src/patches/dolrecomp/`, then verify a clean configure builds.
4. Move `/tmp/tvos-deps` somewhere durable (e.g. `~/projects/eternal-darkness/deps-tvos`) and turn the configure line above into a script in `tools/tvos/`.
5. Move saves to `Documents`; repackage with the trimmed HD pack; install on Living Room.

## Steam Deck / Linux x86-64

`src/build-linux/EternalDarkness` is an **ARM64** Linux build — it cannot run on a Deck. The x86-64 build lives in `src/build-linux-fed` and was made in a Fedora 42 container (glibc 2.41, the same version SteamOS 3.7/3.8 ships; the binary needs only **GLIBC_2.38**). A container on the Mac is the way to build it: `fedora:42` under `--platform linux/amd64`, `dnf -y install clang cmake ninja-build SDL3-devel vulkan-headers libzstd-devel sqlite-devel bzip2-devel`, then the usual configure with `-DSDL3_DIR=/usr/lib64/cmake/SDL3`. Arch `latest` builds a binary that needs GLIBC_2.44 and will not start on a Deck.

The ready package is `dist-deck/EternalDarknessRecomp` (**9.1 GB**): the binary, the shared libraries it needs in `lib/` (SDL3, sqlite3, zstd, bz2, libstdc++, libgcc_s), `res/`, `generated/main.dol`, the disc in `dump/`, **the full 7.6 GB HD texture pack in `textures/`** (all four folders, as `src/textures` has them — the game looks for `textures` next to `res` on Linux), settings, saves, the card, and `play.sh` (sets `LD_LIBRARY_PATH=lib`, runs the game with the disc). On a Deck with 64 GB that is the whole budget after SteamOS; drop `textures/mip override` (3.9 GB) if space is tight.

Install on a Deck: copy the folder to `~/Games`, Desktop Mode → Steam → Add a Non-Steam Game → browse to `play.sh`, then run from Game Mode. Controls go through SDL 3, so the Deck's own pad works. A smoke test in the container starts the binary and resolves every library; it stops at surface creation because there is no GPU there, so **nothing beyond that has been verified without Deck hardware**.

HDR in Game Mode comes from the Dawn package with `patches/dawn/hdr.patch` (fetched automatically by the CMake); `tools/deck/build-dawn-hdr.sh` rebuilds it if needed.

`tools/deck/make-deck-package.sh` is the *release* flow (ships a translator and Zig and builds the game from the player's disc on first start, hardcoded to an `edbuild` container at `/home/deck/src/PS5_EternalDarkness`). It is not what produced `dist-deck`; that folder is a prebuilt game.

