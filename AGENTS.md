# Eternal Darkness recomp

Any harness working in this folder continues the same diary. Do not start a second one.

- Running log and blog draft: `blog/2026-11-20-eternal-darkness-mac/`
- `LOG.md` is the record. Append facts with the date. Say what was measured and what was not.
- `post.md` is the draft for blog.remnantsecurity.com. First person, Ian Trimble. Real window screenshots only, one window at a time, with an italic caption under each image (what it shows, when, what to notice).
- Do not publish, push, or open a pull request until Ian says so. The draft stays on this machine.
- The game art is Nintendo's. Texture packs stay unreleased. Nothing from the disc image goes into the diary.
- Do not fullscreen the game. Do not launch a second copy on the real memory card. A throwaway run uses `ED_SETTINGS` and `--card` under `/tmp`, and `ED_NO_BOOT_STATE=1`.
- Build locally: `BUILDFLEET_LOCAL=1 ninja -C src/build-mac -j 10 EternalDarkness`. Do not send this project through build fleet.
