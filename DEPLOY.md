# RYZE STUDIOS — repo

```
ryze/
├── index.html          ← the whole site (inline CSS + JS, six "pages" via hash router)
├── voxel-sandbox.html  ← playable demo, linked from the work cards
├── shots/              ← homepage screenshots used in the work cards (4 × JPEG, 380 KB)
├── og.png              ← 1200×630 share card for WhatsApp / LinkedIn
└── DEPLOY.md
```

## What I changed from your file

Your updated build is the one to ship — pill nav, phone mockup, bento grid. I kept all
of it and fixed one real bug plus seven things that would have cost you work.

| # | Was | Now | Why it mattered |
|---|---|---|---|
| 1 | **Featured work on the homepage was invisible** | cards re-armed after the copy | `$('hw').innerHTML = $('wk').innerHTML` ran *after* the scroll observer had walked the page, so the four copied cards kept `opacity:0` forever. You'd have shipped a blank row. |
| 1b | **Headings had a blank line inside them** | `.it i{line-height:1}` | CSS `font:` shorthand resets `line-height` to `normal`, so every italic word was 181 px tall against a 121 px line box — "real" and "businesses." were separated by a 60 px gap |
| 1c | Work cards were flat colour blocks | **real homepage screenshots** of Meridian, Leens, TISK and the voxel game, framed at 16:10, 380 KB total, lazy-loaded | A portfolio with no pictures of the work is a claim, not evidence |
| 1d | **Below 900 px the cream disc covered the headline, both buttons and the proof row** | disc hidden on one-column layouts, phone gets an orange glow instead | At 860 px "See pricing" was literally invisible — cream text on a cream circle |
| 1e | Rotating WhatsApp badge collided with the phone and the "Found on Google" chip | removed (the green FAB already does that job) | Two WhatsApp CTAs fighting in the same corner |
| 1f | Hero had no proof above the fold | **10+ sites / 3–5 days / ₹0 monthly**, inset above a hairline | A visitor decided whether to scroll before reaching any evidence |
| 1g | Headline ran 4 lines with no rhythm control | `text-wrap:balance`, tuned clamp, tightened vertical rhythm | Everything below the fold disappeared on a 1000 px-tall screen |
| 1h | Phone screens spilled onto the cream disc | glow + stronger shadow + a scroll cue | Hero read flat, nothing anchored the eye |
| 2 | `₹X,XXX` / `₹XX,XXX` everywhere | ₹6,999 / ₹17,999 / ₹39,999 + add-ons | A pricing page with X's is worse than no pricing page |
| 3 | Voxel Sandbox card had no link | links to `voxel-sandbox.html`, labelled "Play it now" | Your best work wasn't clickable |
| 4 | "we / us / our" throughout | "I / me / my" | You're one person. "We" invites "who else?", and you'd have to answer it live |
| 5 | "10+ sites shipped", four shown | added an **Also built** list: CoreUI-Kit, vote.tiskems, ZeroxOpenWorld, EduSphere | The number was on the page with nothing behind it |
| 6 | No reference | VEloop Rewards quote on the About page | You have a written internship reference and weren't using it |
| 7 | No OG tags | `og:title`, `og:description`, `og:image` → `og.png` | You sell on WhatsApp; a link with no preview looks like spam |
| 8 | No favicon, fonts requested cold | SVG "R" favicon, `preconnect` to both Google hosts | Tab looked unfinished; saves 100–200 ms on first paint |
| 9 | Blank page with JS off | `<noscript>` block with name, services, city, contacts | Google and link previews otherwise see nothing |

**Prices are a proposal, not your decision.** ₹6,999 / ₹17,999 / ₹39,999 sit where the
Kerala market supports them. They live in the `PRICING` array — one place.

## Before you push

1. Open `index.html`, click all six nav items, then scroll the homepage slowly. The
   "Featured work" row should be **visible** — that's the bug I fixed.
2. Open `voxel-sandbox.html` and play 30 seconds.
3. After deploying, send the link to yourself on WhatsApp and check the preview card.

## Deploy

```powershell
cd ryze
git remote add origin https://github.com/HSF237/ryze-studios.git
git push -u origin main
```

**Settings → Pages → Source: Deploy from a branch → main / (root) → Save.**
Live in about a minute at `https://hsf237.github.io/ryze-studios/`.

## Known trade-offs (deliberate, not bugs)

- **Google Fonts from CDN.** Three families give the site its character, but first paint
  waits on the network. Subsetting to local woff2 would save ~250 KB if you ever need it.
- **All content lives in JS.** Clean to edit, invisible to crawlers — hence the `<noscript>`.
- **Hash routing** (`#/work`). Fine for a studio; you can't rank `ryze/work` separately.
- **Heavy motion.** Cursor ring, page transitions, 3D tilt, marquee. All disabled under
  `prefers-reduced-motion`, which is the right call — but test it on a low-end Android
  before you send it to a client.

## Editing content

Everything is in the `/* ===== EDIT ME ===== */` block at the top of the `<script>`:

- `PHONE` — WhatsApp number, digits only with country code (`919496829330`)
- `PRICING` / `ADDONS` — tiers and extras
- `WK` — work cards: `[name, description, url, cardBackground, textColor, tags]`
- `ALSO` — the compact "Also built" rows
- `FQ`, `WHY`, `PRO`, `SV`, `NEED` — copy blocks

A URL containing `.html` is treated as local; anything else is prefixed with `https://`.

---

## Deploy state (recorded 19 Oct 2026)

| | |
|---|---|
| Repo | `https://github.com/HSF237/ryze-studios` |
| Live | `https://hsf237.github.io/ryze-studios/` |
| Pages | branch `main`, folder `/ (root)` |
| Build | `index.html` ~52 KB, self-contained (inline CSS + JS) |

**Assets in `shots/`** — 7 JPEGs, ~600 KB total:

- `site-1.jpg` SOLACE studio page — **the studio's own design, used at the author's instruction**
- `site-2.jpg` CARZO car dealership page — studio's own design
- `site-3.jpg` AETHER clothing store page — studio's own design
- `meridian.jpg` `tisk.jpg` `leens.jpg` `voxel.jpg` — live client sites, used in the work cards

`og.png` is the 1200x630 WhatsApp share card.

**Two things that bite here repeatedly:**

1. **Pages caches for ~10 minutes.** After a push, open in a private tab before assuming a deploy failed.
2. **Content is JS-rendered.** If the script throws, the page used to go blank. It is now gated on
   `html.js` (added only at the very end of the script), so a failure shows the full page plus a red
   error banner instead of an empty screen.

**Known guard rails added during the build:** `.nc` chips hidden, phone screen 9:16.2 to match a real
screenshot aspect, cycle fade wrapped in `try/finally` so the screen can never stay blank.
