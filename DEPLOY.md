# RYZE STUDIOS — repo

```
ryze/
├── index.html          ← the whole site, self-contained (inline CSS + JS)
├── voxel-sandbox.html  ← playable demo, linked from the work section
├── og.png              ← 1200×630 share image for WhatsApp / LinkedIn
└── DEPLOY.md
```

## What I changed in your version

Your file was better than the one I built — real point of view, page transitions,
custom cursor, WhatsApp-first. I kept all of it and fixed six things:

| # | Was | Now | Why it mattered |
|---|---|---|---|
| 1 | `₹X,XXX` / `₹XX,XXX` on every tier | ₹6,999 / ₹17,999 / ₹39,999 + add-ons | A pricing page with X's is worse than no pricing page |
| 2 | Voxel Sandbox card had no link | links to `voxel-sandbox.html`, labelled "Play it now" | It's your best work and it wasn't clickable |
| 3 | "we / us / our" throughout | "I / me / my" | You're one person. "We" invites "who else?", and you'd have to answer |
| 4 | No OG tags | `og:title`, `og:description`, `og:image` | You sell on WhatsApp — a link with no preview looks like spam |
| 5 | No favicon | inline SVG "R" mark | Tab looked unfinished |
| 6 | Fonts requested cold | `preconnect` to both Google hosts | Saves 100–200 ms on the first paint |

**Prices are my proposal, not your decision.** ₹6,999 / ₹17,999 / ₹39,999 are placed where
the Kerala market supports them. Change them in the `PRICING` array at the top of the
script — that's the only place they exist.

## Before you push

1. Open `index.html` and click through all six pages (Home, Services, Work, Pricing,
   About, Contact). The page transitions use a hash router — check no page is blank.
2. Open `voxel-sandbox.html` and play 30 seconds.
3. Send yourself the link on WhatsApp after deploying — confirm the preview image appears.

## Deploy

```powershell
cd ryze
git remote add origin https://github.com/hsf237/ryze.git
git push -u origin main
```

Then **Settings → Pages → Source: Deploy from a branch → main / (root) → Save**.
Live in about a minute at `https://hsf237.github.io/ryze/`.

## Known trade-offs (deliberate, not bugs)

- **Google Fonts.** Three families (Bricolage Grotesque, Instrument Serif, Inter) are
  remote. That's what gives the site its character, but the first paint waits on the
  network. If you ever need it fully offline: download the woff2 files, subset them, and
  swap the `<link>` for local `@font-face` rules. You'll save ~250 KB.
- **All content lives in JS.** Clean to edit, but search engines and link previews see an
  almost empty page. The OG tags cover WhatsApp; for Google, add a `<noscript>` block with
  your name, city and services.
- **Hash routing** (`#/work`) means no separate URLs per page. Fine for a studio, bad if
  you ever want `ryze/work` to rank on its own.

## Editing content

Everything is in the `/* ===== EDIT ME ===== */` block at the top of the `<script>`:

- `PHONE` — WhatsApp number, digits only, with country code (currently `919496829330`)
- `PRICING` — the three tiers
- `ADDONS` — the add-on rows
- `WK` — the four work cards: `[name, description, url, cardBg, textColor, tags]`
- `FQ` — FAQ rows
- `WHY`, `PRO`, `SV`, `NEED` — copy blocks

A URL containing `.html` is treated as local; anything else is prefixed with `https://`.
