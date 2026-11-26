# X drafts — Eternal Darkness recomp

Publishing is blocked: the instruction requires a verified pipeline that checks evidence, media rights and
duplicates. None exists here, so these stay as drafts for handoff. Do not post directly.

## 2026-11-26 — the Apple TV and the Deck

Draft 1 (thread opener):

> Eternal Darkness now runs on three machines I own: a Mac at 4K, an Apple TV at 1080p and 60 a second, and a Linux
> build aimed at a Steam Deck. Same disc, same recompiled game. Thread with what it took and what is still broken.

Draft 2 (measurements):

> On the Apple TV: 1920×1080, 59.94 presents a second, about one core of six. A frame is 16.7 ms — 2.4 in the
> graphics front end, 12 waiting for the screen. The texture pack is trimmed to 3.8 GB because the set has 32 GB.

Draft 3 (honesty):

> Two things are not done. The full texture pack does not fit the television, and every menu option buzzes with a
> short burst of static. And the Deck build has never run on a Deck — it starts in a container and stops at the
> graphics card, because there isn't one.

Media for these drafts (paths in this project, not yet cleared for posting):

- `blog/2026-11-20-eternal-darkness-mac/assets/tvos-app-icon.png` — our own layered icon art, safe to use.
- Apple TV in-game and menu captures: to be taken with `devicectl device capture screenshot` while the set is awake.
  These show a commercial game; the same caution as the blog post applies.
- Steam Deck: no hardware run yet, so no photograph claims one.

Evidence to cite if published: `Handover.md` (tvOS and Deck sections) and `blog/2026-11-20-eternal-darkness-mac/LOG.md`.
