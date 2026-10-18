# RYZE STUDIOS — repo

```
ryze/
├── index.html          ← the whole site (inline CSS + JS, six "pages" via hash router)
├── voxel-sandbox.html  ← playable demo, linked from the work cards
├── og.png              ← 1200×630 share card for WhatsApp / LinkedIn
└── DEPLOY.md
```

## What I changed from your file

Your updated build is the one to ship — pill nav, phone mockup, bento grid. I kept all
of it and fixed one real bug plus seven things that would have cost you work.

| # | Was | Now | Why it mattered |
|---|---|---|---|
| 1 | **Featured work on the homepage was invisible** | cards re-armed after the copy | `$('hw').innerHTML = $('wk').innerHTML` ran *after* the scroll observer had walked the page, so the four copied cards kept `opacity:0` forever. You'd have shipped a blank row. |
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
git remote add origin https://github.com/hsf237/ryze.git
git push -u origin main
```

**Settings → Pages → Source: Deploy from a branch → main / (root) → Save.**
Live in about a minute at `https://hsf237.github.io/ryze/`.

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
