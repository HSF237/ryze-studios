# RYZE STUDIOS — asset + design record

Written 19 Oct 2026, because the source screenshots are being evicted from the working
context. Everything below is the durable record; the files themselves live in the repo.

## Palette (from the original design)

| token | value | used for |
|---|---|---|
| bg | `#0e0d0c` | page background |
| fg | `#efeae0` | headline / body text |
| mu | `#8f8a80` | muted copy |
| dimmer | `#6b6660` | least emphasis |
| line | `rgba(239,234,224,.13)` | hairlines |
| accent | `#ff5a2c` | buttons, italic accent word, links |
| cream | `#e9e4d8` | hero disc, inverted panels |

Type: headlines **Bricolage Grotesque 800** (-.045em); italic accent words **Instrument
Serif italic**; body/labels **Inter**. Google Fonts with `preconnect`.

## Featured work-card palettes (background / text)

| project | bg | fg |
|---|---|---|
| Hasan Portfolio | `#1b1a17` | `#efeae0` |
| Al Awaans (Leens) | `#2d4f55` | `#efeae0` |
| CoreUI Kit | `#cfd6dc` | `#12181d` |
| TISK School | `#b5523b` | `#fbf1e6` |
| Meridian Arch | `#c8a97a` | `#1a1410` |
| Voxel Sandbox | `#5c6b3d` | `#f1f0e0` |

Grid cards derive their tint automatically from the mean colour of their screenshot
(light screenshot → cream card, dark → near-black card).

## og.png (1200x630 share card)

bg `#0e0d0c`; 6px accent bar at x=78,y=78; "RYZE" Bricolage 800 ~138px; "STUDIOS" 21px
letterspaced +13px, `#8f8a80`; tagline 30px two lines — "Websites that turn visitors"
in `#efeae0`, "into customers." in `#ff5a2c`; footer row 24px muted: left "10+ sites
shipped · live in days · files yours forever", right "hsf237.github.io/ryze-studios";
1px border `#26251f`.

## Phone mockup in the hero

Frame 9:16.2, 7px bezel `#2b2926`, screen `shots/site-1..3.jpg` (660x1264, aspect .522),
cycles every 5.2s with a 330ms fade. Those three images are the studio's **own** designs
(SOLACE / CARZO / AETHER), used at the author's instruction.

## Asset naming

- `shots/<slug>-0|1|2.jpg` — the three-frame gallery on grid cards (560x350)
- `shots/w-<slug>.jpg` — single thumbnails (Tech Orbit, CARZO, EduSphere, CoreUI)
- `shots/site-1|2|3.jpg` — the three phone mockups
- `shots/og.png` — share card
- `shots/tall/<slug>.jpg` — full-page captures used by the project detail view

## Failure modes already hit (do not repeat)

1. GitHub Pages caches ~10 minutes — always check a private tab before "fixing" a deploy.
2. A bracket-matching regex over the data arrays cut at a `]` inside a tags list and
   broke the JS. Rebuild arrays whole, never patch them with regex.
3. Inline Python/Node through PowerShell mangles quotes. Always write a script file.
4. Screenshots of SPA sites need 5-15 s before capture, or they come out blank.
   `carzo` returns a pure white page even after 15 s — use the supplied screenshot.
