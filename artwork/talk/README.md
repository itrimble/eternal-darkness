# Artwork for the local AI talk

Composited from the art already in this project. Nothing here was generated: the image provider returned
"prepayment credits are depleted" twice, so the set is built from `src/res/icon/tvos-icon-source.png` (the wide
banner) and `src/res/icon/window.png` (the square cover), with the type set in Georgia Bold and Georgia by
ImageMagick, which is also what keeps the titles legible at small sizes.

| File | Size | Use |
| --- | --- | --- |
| `site-hero-1920x1080.png` | 1920x1080 | the banner with a line naming the three machines: site or slide title |
| `mac-hero-2560x1440.png` | 2560x1440 | the book art, the app icon and "a personal recompilation": Mac slide |
| `steam-header-920x430.png` | 920x430 | Steam library header capsule shape |
| `steam-vertical-600x900.png` | 600x900 | Steam library vertical capsule shape |
| `box-front-1200x1600.png` | 1200x1600 | a box front for the "if it were a product" slide |
| `talk-card-1080.png` | 1080x1080 | square card for a talk listing or a slide footer |

Rebuild all of it with `/tmp/artwork.sh` (kept out of the repo on purpose: it is four ImageMagick recipes and a
font path). The recipes crop the cover's own metallic title away before drawing white type over it, because at
600 px wide that metallic lettering is unreadable.

## Rights, stated plainly

The game art is Nintendo's, and the reconstruction is a personal project. These files are for a local talk on this
machine. They are not cleared for publishing, a store listing or a product page, and the box front is not a claim
that anything is for sale. If any of it goes public, that decision and the rights check come first.

## Not done yet

- Valve's actual capsule templates and safe-area guides; the two Steam shapes here are the right proportions but
  not the published templates.
- A box mockup with a spine, back panel and disc, rather than a front panel.
- A page for a site: this is imagery only.
- Photography of the Deck build, since it has not run on Deck hardware.
