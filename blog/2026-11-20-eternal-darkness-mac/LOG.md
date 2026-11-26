# Eternal Darkness recomp diary

Running log for every harness. Append facts with the date. Say what was measured and what was not. The public draft is `post.md` in this folder. Do not publish, push, or open a pull request until Ian says so. Do not fullscreen the game. Do not launch a second copy on the real memory card. A throwaway run uses `ED_SETTINGS` and a `--card` path under `/tmp` whose name contains `-slot-a.dolcard`, plus `ED_NO_BOOT_STATE=1`. Build locally with `BUILDFLEET_LOCAL=1 ninja -C src/build-mac -j 10 EternalDarkness`.

Nintendo owns the game art. Texture packs stay unreleased. Nothing from the disc image goes in this diary. Screenshots are one window, `screencapture -x -o -l<id>`, with an italic caption in `post.md`. Look at the image before it is added.

## 2026-11-20

Variable refresh is in the Mac build. The game clock is still 60. Choice 0 means follow a screen that can vary. On a screen that cannot, an unset preference stays unset, so quitting does not rewrite a 60 setting into Variable.

The display-link file failed to compile because `CADisplayLink` was only a forward declaration from AppKit. Importing `QuartzCore/CADisplayLink.h` and `QuartzCore/CAFrameRateRange.h` fixed it. The preferred-rate setter is still invoked by selector, because the property is not marked available on Mac. Local ninja then linked `EternalDarkness` (exit 0). Pre-existing warnings in the graphics switch and one string warning in the host remained. The known duplicate-library linker warning remained.

Throwaway launches, sound off, windowed, copy of the card under `/tmp/ed-diary/`. Real files checked before and after, unchanged:

- `src/ed-GEDP01-slot-a.dolcard` mtime 1794975527, 122988 bytes
- `src/saves/slot1.state` mtime 1795042522, 35794892 bytes
- `src/settings.ini` mtime 1795220963, 1056 bytes

First launch log (window on a 60 Hz screen):

- `[display] screen 60-60 Hz`
- `[display] window 2908x1554 pixels, render native, fixed rate, windowed`
- `[display] frame rate 60 Hz`
- `[display] frame rates: 60 (game runs at 60)`
- probe settled at 17.074 ms (58.57 Hz) after one unsettled pass
- `[texpack] 7738 textures`
- no `[display] refresh range` line

Counters read off the window, not averaged over a play session:

- 19:43 Nintendo logo, title 58, corner 58
- 19:44 opening movie, one frame, title 48, corner 51 (blurred; not used in the post)
- 19:45 title screen, title 60, corner 60

Second launch opened options with frame rate selected. Probe settled at 16.663 ms (60.01 Hz) after three unsettled passes. The menu showed 60 and the note that the screen is at 60. No Variable row.

Attached screens at 19:49, all fixed (shortest interval equals longest, and the step equals that interval). The laptop panel was not in the list. The window was not moved onto the 165 Hz panel, so a menu offer of 165 was not seen.

| Screen | Points | Interval seconds |
| --- | --- | --- |
| LF32TU87 | 2304 by 1296 | 0.016668 |
| IIIP 44522 | 2496 by 1404 | 0.016668 |
| 100027813 | 1920 by 1080 | 0.006061 (165 Hz) |
| Elgato Prom. | 1024 by 600 | 0.016667 |

Screenshots in `assets/`, each looked at before it went into `post.md`:

- `eternal-darkness-launcher-window.png` at 19:42
- `eternal-darkness-nintendo-logo-58fps.png` at 19:43
- `eternal-darkness-title-screen-60fps.png` captured 19:45
- `eternal-darkness-options-frame-rate.png` captured 19:48

Not measured: a fight or a still camera over minutes, the 165 Hz menu, the laptop panel, and whether Variable presents a blended frame. Do not invent a 120 Hz picture.

Publishing: Eternal Darkness is not in `~/projects/PROJECTS.md` and not in `product-diary/categories.json`. The folder is not on the infra or do-not-promote lists, so the nightly survey can see it. Do not edit `PROJECTS.md` unless Ian asks. `posts/2026-11-20/` already has the weekly roundup, so a later pull request has to share that day. Do not sign, deploy, or push.

Graphics pages compiled into the same binary at 20:42. Local ninja linked `EternalDarkness` (exit 0). The same string warning in the host and the same linker warnings remained. The game was not launched, so the new pages were not seen in a window and no note was checked for clipping.

Options now opens on Graphics & presentation and Accessibility, then the existing sound, controls, camera, frame counter, disc, and back. Graphics opens Display, Image, Lighting, and Colour. Display has the window, internal resolution, frame rate, and aspect. Image has textures, filtering, and game text. Lighting has baked shadows, ambient occlusion, and glow. Colour has HDR, and calibration only when this display can do HDR. The zero end of glow, occlusion, and filtering reads Original. There is no CRT row, no anti-aliasing row, no 30 or uncapped cap, no V-Sync row, no output-resolution list, no benchmark, and no preset that rewrites the ini.

Modern system-failure pictures default off. A settings file without the new keys stays on the game's own presentation. Modern is opt-in and draws inside the game window. Holding Escape or Start dismisses one; that hold is not also the pause menu or photo mode. Losing the window's focus ends one, and the window is not raised. The fake deleted-save still only hides the slot list. This was read from the host code and the successful compile. It was not played.

Internal resolution is still the height the game is told to draw. The note does not claim a hallucination keeps its size at 4K. The real card, `slot1.state`, and `settings.ini` were the same before and after the build: mtimes 1794975527, 1795042522, and 1795220963.

The “not launched” sentence above is out of date. At 20:54 the same binary was opened windowed and muted, on a throwaway copy of settings and with no memory card. `ED_MENU_OPEN=graphics`. The window was 1454 by 777 points (2908 by 1554 pixels). The log said Fifo, screen 60–60 Hz, native, frame rate 60, and 7738 textures. A startup probe printed 18.578 ms (53.83 Hz) after unsettled gaps. That is not the panel rate. The screen line is.

Graphics, Display, Image, Lighting, Colour, Options, and Accessibility all drew. Notes fit on two lines. Values matched the copied file: HD pack, 16x filtering, sharp game text, sharp baked shadows, soft ambient occlusion, glow Original, HDR on, calibrate at 1000 nits peak, camera Original, frame counter on, Europe disc, music and effects at 100, controls Modern. Frame rate was selectable so the note could be read, and only 60 was offered. HDR being available does not say which monitor.

Accept and the arrow keys applied at once. A stray accept moved internal resolution to 4K (3840 by 2160, then 5040 by 2160 at 21:9). The log then showed area resolve back into the window (3840 by 2160 to 2763 by 1554, and 5040 by 2160 to 2908 by 1246). Left restored native, then 16:9. The last display lines before quit were aspect 16:9, framebuffer 2763 by 1554, render native, fixed rate, windowed.

Accessibility was toggled to Modern and back to Original, and failure style to macOS and back to Auto. The log recorded each change. No low-sanity roll was played. Hold-to-dismiss, focus loss, and the 25 second cap were not exercised.

Options Back was not used. Quitting still saves. SIGTERM of that process exited on the first check. The throwaway file gained `system_illusions = 0` and `illusion_style = 0` and kept aspect 2, render height 0, frame rate 60, and windowed. The real card, `slot1.state`, and `settings.ini` were the same after quit: mtimes 1794975527, 1795042522, and 1795220963. A later quit of the real game would write those two keys at those defaults. It would not change the existing choices. `post.md` was not edited. Nothing was published.

Options is the two-page book again. Display, picture and sound are on the left. Light, play and illusions are on the right. There is no Graphics page to open first. Seen windowed at 21:11, then again after the right-hand page was drawing its rows in a column: a fixed page height had collapsed that page. With the pages sized by their rows, both sides read. A value that left and right can change wears ‹ ›. Frame rate on this 60 Hz screen does not, because 60 is the only choice. Calibrate HDR does not, because it opens the three steps. The card and `slot1.state` were still mtimes 1794975527 and 1795042522. `settings.ini` is 1096 bytes after the earlier quit wrote the two new keys at 0. Nothing was published.

The DualShock 4 (vendor 054c, product 09cc) was turned on while Options was already open. The light bar path logged `sdl`, so the pad was open, and the footer stayed “A select · B back”. That book only rebuilt its rows for Controls and Mapping. A later launch with the same pad already on logged `[glyphs] prompts: ps` and the footer showed Cross for select and Circle for back, windowed, at 21:40. Turning a pad on now marks the menu dirty and switches prompts to that pad until a key is pressed. That off-then-on path was not exercised again after the rebuild. Card and `slot1.state` mtimes were still 1794975527 and 1795042522. The quit rewrote `settings.ini` (1096 bytes, mtime 1795232401) with the same aspect 2, native height, 60, windowed, and both illusion keys at 0. Nothing was published.

Brightness is a row on the Picture page. It opens the game's own dark room from the texture pack, full-bleed, with the game's TV warning under a parchment banner. Fifty is the picture as it is. Left and right step by one. The screen was up windowed (window 24903, 1454×777) and the room loaded at 1280×985. Left fifty times reached 0% at 22:23. A stone sample fell from mean RGB 25.1, 17.9, 9.0 to 16.4, 11.3, 5.5. Right one hundred times reached 100% at 22:24. The lamp sample rose from 87.4, 64.1, 30.7 to 121.3, 90.3, 45.6. Fifty left steps put it back. At 22:25 that room matched the 22:18 capture exactly (pixel difference 0). The footer was the keyboard set. Bluetooth listed the DualShock 4 under Not Connected, and this launch never logged `controller on`. A frame of the running game, rather than this room, was not photographed. The slider does not write the ini. `settings.ini` was still 1096 bytes, mtime 1795233871, with no brightness key. The card and `slot1.state` were still mtimes 1794975527 and 1795042522 while that screen stayed open. `post.md` was not edited. Nothing was published.

0% was half the light, so the room stayed visible. Ian said dark at 0% has to be black. The dark half is now a fraction of the light (0 black, 50 unchanged) and the bright half is still up to twice. Quitting that screen wrote `brightness = 50` (ini 1112 bytes, mtime 1795237726). Card and `slot1.state` were still 1794975527 and 1795042522. The relaunch opened the same room, windowed, with the DualShock on (`[glyphs] prompts: ps`, vendor 054c, product 09cc). At 0% the stone, the lamp and Alexandra were gone: the upper room had no pixel above 2, and the banner, the warning and the slider stayed. Back at 50% the room matched the 50% capture of that launch exactly. A gameplay frame under the gain was not photographed. Nothing was published.

The Dock and window icon is the book cover Ian generated, 1024 square, with the white corner outside the frame made transparent. `res/icon/window.png` is that picture. `res/icon/AppIcon.icns` is the same art at the Mac icon sizes, which is what the Mac package script copies into the app. The package was not built. The tome page still uses the previous mark (`emblem.png`). A windowed launch at 00:02 (pid 58053, 1454×777) showed the book in the Dock. The main menu was up. The card and `slot1.state` were still mtimes 1794975527 and 1795042522. `settings.ini` was still the earlier quit (1112 bytes, mtime 1795240243, brightness 50). Nothing was published. The brightness plate is still the soft one.

The portable app is `dist/Eternal Darkness.app`. `du` reports 9.1G. `tools/mac/make-portable-app.sh` exited 0. Inside: the host binary as `Contents/MacOS/game`, a small launcher as `Contents/MacOS/EternalDarkness`, 49 libraries in `Contents/Frameworks`, `res/`, `generated/main.dol`, the European disc (first six bytes GEDP01, 1.4G), the texture pack, `AppIcon.icns`, settings, the memory card, and `saves/`. The disc, pack, card and saves were APFS clones (`cp -c`), so on this volume they share blocks with the project copies until one side is written. The game binary’s non-system libraries are `@rpath` only. `codesign --verify --deep --strict` succeeded. The signature is ad hoc. It was not notarized.

The launch was the launcher binary, started from `/tmp`, not a Finder double-click. Pid 74975. Its working directory is `Contents/Resources`. The command line points `--iso` at the disc inside the app. The log said windowed 1454×777, Fifo, 7738 textures from that pack, and 155 pipelines built at start. At 00:15 the window (id 26021, capture 2908×1618 including the title bar) was the main menu: Continue, Slot 1 · 18 Nov 16:55, New game, Load game, Options, Quit. One process. The project card and `slot1.state` were still mtimes 1794975527 and 1795042522. Project `settings.ini` is still 1112 bytes, brightness 50, windowed, disc the absolute path. Its mtime is now 1795241383 (00:09:43), later than 1795240243. This step did not edit it. The copy in the app is 1075 bytes, mtime 1795241420, disc `dump/GEDP01.iso`. Sound was not muted. Speaker output was not measured. The Dock icon on this launch was not checked. The brightness plate is still the soft one. The process was left running. Nothing was published.

That process quit on its own at 00:27 (exit 0, about 15 minutes up). The log added `[glyphs] controller on: PS4 Controller, vendor 054c, product 09cc, type 5`, `[controls] light bar: sdl`, then `[gx-core] shutdown: submitted=0 rejected=0 failed=0`. The app's `settings.ini` was rewritten (1075 bytes, mtime 1795242472) and still says brightness 50, windowed, `disc = dump/GEDP01.iso`. The app card and `slot1.state` were still mtimes 1794975527 and 1795042522. The project card, `slot1.state`, and `settings.ini` were unchanged (ini still 1112 bytes, mtime 1795241383, absolute disc path). No second process was started. Nothing was published.

Opening the app landed on the recomp tome (Continue, New game, Options). The disc boot is `--no-launcher`. `portable-main.c` now passes that. The stub was recompiled and copied into `/Applications/Eternal Darkness.app` and `dist/Eternal Darkness.app`, and both were ad-hoc signed again (`codesign --verify --deep --strict` passed). The game binary was not rebuilt. A launch of the Applications app (pid 97717, from `/tmp`) did not show the tome. The log has `[boot] entry=0x80005308`, the disc path inside the app, `[run] starting execution`, and `********* BOOOT PAK LOAD********`. At about 15 seconds the window was the Poe quote from the game's intro, title `Eternal Darkness — 59 fps`, windowed 1454×777. The app card and `slot1.state` were still mtimes 1794975527 and 1795042522. `saves/boot.state` was rewritten (2840670 bytes, mtime 1795243012), which is the fresh machine saved at boot. The project card, slot, and settings were unchanged. The process was left on that intro. `play.sh` still opens the tome. Nothing was published.

`tools/mac/make-portable-app.sh` was run again at 00:41 and exited 0. The host binary was not recompiled: `build-mac/EternalDarkness` was still the 23:27 build, and the only newer source files were the icon and `portable-main.c`, which the script compiles itself. The new bundle is `dist/Eternal Darkness.app` and `/Applications/Eternal Darkness.app` (the old Applications copy was removed and replaced). `du` reports 9.1G. 49 libraries. Disc header GEDP01. Settings in the app: brightness 50, windowed, `disc = dump/GEDP01.iso`. The launcher contains `--no-launcher`. `codesign --verify --deep --strict` passed on both copies. `install_name_tool` warned that its edits drop signatures; the script signed them again afterwards. Project `settings.ini` was still 1112 bytes, mtime 1795241383. The project card was still mtime 1794975527. The rebuilt app was not launched. Nothing was published.

The earlier disc boot (pid 97717, the Applications app from before this rebuild) ended when the window closed. The log says `[run] stopped: window closed`, no exception, and `[gx-core] shutdown: submitted=22408 rejected=0 failed=0`. That copy of the app was removed by the rebuild. Nothing is running now.

## 2026-11-21

The launch audio was captured, not guessed at. `ED_PCM_OUT` on a windowed disc boot (throwaway settings and card under `/tmp/ed-snd/`, `ED_NO_BOOT_STATE=1`) wrote 1618352 stereo frames at 32 kHz (50.57 s): peak 18816, no clipped sample, and the only zero runs are the boot's own silences (12.18 s for 3.13 s, 29.69 s for 2.33 s). So no chunk was dropped inside a piece of music. The two streamed tracks the game prepared (`[dtk] prepare disc=0x08508000 len=688128` at boot and `disc=0x03A90000 len=557056` under the Dolby card) were decoded again in Python with Dolphin's StreamADPCM coefficients, a 4-byte block header and 28 samples: the first is music-like (79% of its energy between 100 Hz and 1 kHz), the second is a loud ~58 Hz drone, and the capture's own spectrum matches both. The host sends what the disc holds. Ian listened at 14:2x and said start game, load game and the options toggles sound fine now; the earlier glitch was heard while Dante's Inferno was also running. Nothing in the game's mix was changed.

The DualShock 4 (00:22:68:DD:74:9A, 054c/09cc) is paired and connected. A launch at 14:05 opened with `[glyphs] prompts: kb` and logged `controller on: PS4 Controller` only later in the run; a later launch logged the pad at startup (`prompts: ps`). The pad path works; that first run started while the pad was asleep.

R1 at launch did nothing, and it was not the pad. The fast-forward guard in `controls.cpp` records a shoulder button that goes down outside a cutscene as "held from play", and only clears it when the button is let go, so R1 held from the logo cards never fast-forwarded the opening movie. A held R1 now counts as a skip until the first cutscene ends, and, because the game's own skip is A on a movie, R1 is passed on to the game as a pulsed A (six frames in sixteen) while a `/fmv` movie is open. A scene in the letterbox bars still fast-forwards, at the speed the game's own audio production allows (four was asked for; about 1.5x was seen).

Measured with `ED_PAD_SCRIPT` (a virtual pad through the same path as a real one), windowed and muted, throwaway card:

- `rb` held from frame 880: cutscene 968 to over at 1007 (39 frames) and 1295 to 1338 (43 frames), the two intro movies skipped.
- `rb` held from frame 1850, after the first movie had ended: movie 1970 to 2792 ran whole, so a button held from play still refuses to skip. `rb` from 2000 (inside a cutscene) did skip, as a press made during one should.
- Before this change, `rb` from 880 fast-forwarded 968 to 1927 (RB logged on at 970) without skipping it.

The prompt's sign now grows a bar (a skip sign) while a movie plays. The project card, `slot1.state`, and `settings.ini` were the same after every run: mtimes 1794975527, 1795042522, 1795241383. Not done: a real pad's R1 (only the scripted pad was used), a movie skipped part-way after playing, the letterbox fast-forward's exact speed logged with `ED_SPEED_LOG`, and the skip sign seen on screen. The portable app was not rebuilt for that step. Nothing was published.

The portable app was rebuilt at 14:36 from that binary: `tools/mac/make-portable-app.sh` exited 0, `dist/Eternal Darkness.app` is 9.1G with 49 libraries, ad hoc signed, and `codesign --verify --deep --strict` passed. The old `/Applications/Eternal Darkness.app` was removed and the new bundle copied in with `cp -cR`; `codesign --verify --deep --strict` passed there too. Inside, `settings.ini` is 1075 bytes with brightness 50, frame rate 60, `system_illusions = 0`, and `disc = dump/GEDP01.iso`; the card is the same clone (mtime 1794975527, 122988 bytes). A smoke test of the app's own game binary, windowed and muted, with a throwaway card and settings under `/tmp` (so the bundle's were not written), loaded 7738 textures from the app's pack, logged `prompts: ps`, and skipped the intro movies: 968 to 1002 and 1288 to 1323. Launched by LaunchServices (`open -a`), the process was `Eternal Darkness .../generated/main.dol --iso .../dump/GEDP01.iso --no-launcher --max-blocks ...`, so the packaged launcher boots the disc; the window itself was not looked at in that launch. Quitting it rewrote the app's `settings.ini` (1075 bytes, same values); the app's card stayed at mtime 1794975527. The project card, `slot1.state`, and `settings.ini` were still 1794975527, 1795042522, and 1795241383. Nothing was published.

The 4K run earlier on 21 November was not written down here when it happened; its numbers come from that window. Windowed on the laptop, internal resolution set to 4K drew 3840 by 2160 (5040 by 2160 at 21:9) and resolved back into the 1454 by 777 window (2763 by 1554, and 2908 by 1246 at 21:9). Because the window is smaller than the render, MetalFX stayed off and the present pass scaled the picture down. The on-screen counter read 60 in 24 of 26 one-second samples and 59 in the other two; the panel measured 59.97 Hz. Once the load settled, the process used about 55 to 62% CPU and 1.4 GB. HDR, SSAO and the HD pack were on. No 4K display was attached, so this is an internal render, not a 4K output.

For the post, a window capture of the Options spread was taken at 14:41 on 21 November (`assets/eternal-darkness-options-book.png`, window 28560, launched with a throwaway card and `ED_MENU_OPEN=options`), showing Window selected, Resolution "Window (2763×1554)", Brightness at 50%, the value arrows, and the footer's Cross and Circle. Looking at it before adding it: both pages read, the four sections on the left and three on the right are titled, and nothing is clipped. A second launch tried to script the brightness room open (seven downs, then A) for a picture of the room; the presses did not take and the selection stayed on Window, so no room picture was added. The game was quit; the project card, `slot1.state` and `settings.ini` were unchanged (1794975527, 1795042522, 1795241383).

`post.md` was rewritten the same day to cover the 4K render, the brightness slider, the Options spread, the pad's glyphs, the R1 skip, the audio capture and the app, keeping the four earlier screenshots and adding the options-book capture. Everything in it is a number or an event from these entries; where a thing was scripted rather than done by hand, it says so. Nothing was published.

## 2026-11-21, the Apple TV

A tvOS build exists and runs on the Shed Apple TV (`AppleTV11,1`, tvOS 27.3, 32 GB, three cores). Xcode 27.2's tvOS 27.2 SDK. Dawn `266c1cf8` builds for tvOS arm64 with the Metal backend unmodified once `DAWN_BUILD_PROTOBUF=OFF` is set for cross-compiling (19 MB static). SDL 3.4.20 builds for tvOS with Metal, CoreAudio and MFi gamepads; fmt 12.2.1, freetype 2.13.3, libpng 1.6.44 and zstd 1.5.6 come from the same prefix; brotli turned out not to be needed. The project configures and builds: 1,363 targets, a 51 MB executable with `LC_BUILD_VERSION platform 3`.

Code it took, all guarded so the Mac build is unchanged: `MetalBinding.mm` loses AppKit, returns false from the EDR setter (a `CAMetalLayer` on tvOS will not take it) and reports 60 Hz instead of the NSScreen display-link code; `MetalFXUpscale` is stubbed (no IOSurface headers in the tvOS SDK); aurora links UIKit/CoreVideo/QuartzCore instead of AppKit/IOSurface; `restart_process()` is a no-op; `sigaltstack` is skipped; `main.c` uses `SDL_main` and a container shim, and `settings.c` forces HDR off on tvOS because the layer cannot show the range.

The app is signed ad hoc plus a development identity with the wildcard "tvOS Team Provisioning Profile" Xcode minted for Shed (`get-task-allow`), installed with `devicectl device install app`, and it survives: still running after 200 s. A layered tvOS icon came from the art Ian supplied: a `.brandassets` catalog (an appiconset is rejected as "unassigned child"; each layer keeps its images in a `Content.imageset` inside the layer), compiled by `actool` to `Assets.car` with `CFBundleIcons`/`CFBundlePrimaryIcon`. The packager strips extended attributes before signing: ImageMagick's xattrs make codesign refuse the bundle.

Measured on the TV, 1920x1080 fullscreen: 59.94 Hz between presents, 60 fps on the game's own counter, `[texpack] 7738 textures` with the HD pack. `DOL_GX_TIME` per frame in the loaded save: `gx=2.4 ms wait=12.1 ms wplan=3.5 ms submit=1.5 ms present=14.9 ms` of a 16.7 ms frame — about 4 to 6 ms of work and 12 ms waiting on vsync. CPU about 1.08 cores idle, 2.8 cores at the peak of a scripted walk-and-attack, still 60 fps. The tvOS `cpu_resource` reports (84 to 98% average against a 50% policy) come from launches made over the network, where the app counts as background; the same process ran 200 s regardless.

Two corrections worth keeping. An earlier "4.4 cores in play" figure was this session's own frame-dump keeper thread, which forces continuous whole-frame GPU readback: measuring the instrumentation. And the in-app PPM frame dump never fired because the backend only reads the frame back while the guest peeks the EFB; the working tool is `xcrun devicectl device capture screenshot` (Xcode 26.6+), which returns a clean 1920x1080 PNG.

The guest memory path is now inline for main RAM in `core/cpu.h` (both 0x8 and 0xC aliases; MMIO, MEM2 and unmapped fall back), with reservation clearing kept on stores. In the loaded save it changed nothing measurable: 108 to 109% CPU and the same block rate before and after. That scene is not memory-bound.

The HD texture pack: the full 7.6 GB will not fit a 32 GB Apple TV (the 9.1 GB app wanted 8.2 GB with 4.6 GB short). The trimmed pack without the 3.9 GB "mip override" folder makes a 5.2 GiB app; that installed after the two other apps were deleted. The audio buzz on the game's own options menu is still there and unexplained; it is the one real bug left. Nothing was published.

## 2026-11-23, the Steam Deck

The Linux binary in `src/build-linux` is ARM64 (`file` says `ELF 64-bit LSB shared object, ARM aarch64`), so it could never run on a Steam Deck, which is x86-64. A first x86-64 build in an Arch container (`archlinux:latest` under Docker `--platform linux/amd64`, pacman needs `--disable-sandbox` there) linked against glibc 2.44 and would not start on SteamOS: its 3.7.20 manifest lists glibc 2.41. Rebuilt in `fedora:42` (glibc 2.41) with clang, cmake, ninja, `SDL3-devel`, `vulkan-headers`, `libzstd-devel`, `sqlite-devel`, `bzip2-devel`; the binary needs only **GLIBC_2.38** and is at `src/build-linux-fed/EternalDarkness`, 65 MB. The first attempt in that container also caught two bugs in this session's own host edits (an errno block that had landed in `extract_dol`, and a frame-dump thread that needed pthread); the frame-dump hook was removed, since `devicectl device capture screenshot` is the tool.

The package is `dist-deck/EternalDarknessRecomp`, 9.1 GB: the binary, the libraries a Deck may not ship (`libSDL3.so.0`, `libsqlite3.so.0`, `libzstd.so.1`, `libbz2.so.1`, `libstdc++.so.6`, `libgcc_s.so.1`) in `lib/`, `res/`, `generated/main.dol`, the European disc in `dump/`, the full 7.6 GB texture pack in `textures/` (15,726 files), settings with `hd_textures = 1`, saves, the card, and `play.sh` (sets `LD_LIBRARY_PATH=$PWD/lib`, runs the game with the disc). It has not run on Deck hardware. In the Fedora container it starts, resolves every library and stops at `Failed to create surface descriptor` because there is no GPU; that is the end of what a Mac can verify.

## 2026-11-26, the blog

`post.md` gained "The same game under the television" and "A Steam Deck build that matches its libc", an icon image (the 1280x768 composite of the layered TV icon), a corrected summary, and the SteamOS manifest as a source for the 2.41 claim. `blog-check.sh` prints PASS. An X draft is at `content/x/posts.md` and is explicitly blocked from posting until a verified pipeline exists. Two screenshots of the television (the tome and a room in play) are still to be taken: `devicectl device capture screenshot` answers "Display reports zero-sized native size" while the set is asleep. Nothing was published.
