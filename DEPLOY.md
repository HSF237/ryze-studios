# RYZE STUDIOS — deploy

The repo is already initialised and committed. All that's left is creating the GitHub
repo and pushing — that part needs your account, so it's the one thing I can't do.

## 1. Fix two links first (2 minutes)

In `index.html`, search for `TODO`:

- `mailto:hello@ryzestudios.com` → your real email
- `https://wa.me/910000000000` → `https://wa.me/91` + your WhatsApp number, no spaces
  (e.g. a number `98470 12345` becomes `https://wa.me/919847012345`)

Then commit the change:

```powershell
cd ryze
git add -A
git commit -m "contact details"
```

## 2. Create the repo and push

Go to **github.com/new**, name it `ryze` (or `ryze-studios`), **Public**, and do **not**
add a README. Then:

```powershell
cd ryze
git remote add origin https://github.com/hsf237/ryze.git
git push -u origin main
```

## 3. Turn on Pages

Repo → **Settings → Pages** → Source: **Deploy from a branch** → Branch: **main**,
folder **/ (root)** → Save.

Wait ~60 seconds. Live at:

```
https://hsf237.github.io/ryze/
```

## 4. After it's live

- Open it on your phone, not just the desktop. Tap "Play the live demo" — the voxel game
  needs WebGL2, so use Chrome or Safari, not an in-app browser (Instagram/WhatsApp
  webviews will show a black screen).
- Add the URL to your LinkedIn headline and your GitHub profile README.
- In the old `hasan` repo, add one line to `index.html` pointing at the studio site so
  the two don't compete: `<a href="https://hsf237.github.io/ryze/">RYZE STUDIOS →</a>`

## What's in here

| File | Size | Purpose |
|---|---|---|
| `index.html` | ~11 KB | The studio site, single page, 8 sections |
| `styles.css` | ~14 KB | All styling. No framework, no fonts loaded |
| `voxel-sandbox.html` | ~68 KB | The playable demo linked from the work section |

Total: **92 KB** for the whole studio, including a 3D game.

## Changing the look later

Everything lives in `:root` at the top of `styles.css`:

```css
--accent:#c8ff2e;   /* swap for #ff5c1a (orange) or #3b5bff (cobalt) */
--bg:#0a0a0b;       /* try #f7f6f3 for a light version — you'll also need to flip
                       --fg, --dim, --card and --line */
```

Editing one hex value changes the entire site's accent.
