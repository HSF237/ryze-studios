# Recording real scroll-through videos — brief for Claude

**What this is:** the RYZE STUDIOS portfolio currently fakes the "scroll recording" of each
project by animating a tall screenshot inside a browser frame. To replace it with real
video (loader animation, scroll-triggered reveals, click-through on multi-page sites) the
videos have to be recorded in a real browser. Claude can do that with Playwright.

## Links

| what | where |
|---|---|
| Playwright (records video natively) | https://playwright.dev/docs/intro |
| ffmpeg (re-encode + shrink) | https://ffmpeg.org/download.html |
| Windows ffmpeg build | https://www.gyan.dev/ffmpeg/builds/ |

`npm init playwright@latest` → then `npx playwright install chromium`.
Playwright needs Node 22, 24 or 26 — this machine has 24 already.

---

## The prompt (copy everything in the block)

```
You are recording screen-capture videos of live websites for a design portfolio.

GOAL
For each site in the list below, produce ONE video of ~12-18 seconds that shows the page
from the moment it loads (including any loading/intro animation) all the way to the bottom,
scrolling smoothly — and, if the site has more than one page, also clicking through its
top-level navigation pages and scrolling each one.

SETUP
- Use Playwright with Chromium (npm init playwright@latest; npx playwright install chromium).
- Browser context: viewport 1440x900, deviceScaleFactor 1.
- Enable Playwright's built-in video recording:
    browser.newContext({ viewport:{width:1440,height:900},
                          recordVideo:{ dir:'raw', size:{width:1440,height:900} } })
- Inject this CSS on every page before interacting:
    ::-webkit-scrollbar{display:none!important} html{scrollbar-width:none!important}
    *{cursor:none!important}
- Navigate with waitUntil:'networkidle', then WAIT 2000ms before doing anything — that is
  what captures the loader / intro animation instead of jumping past it.

SCROLLING (the important part)
- Do NOT use window.scrollTo for each frame. Scroll in small steps in a loop so the motion
  is smooth at ~30fps:
    for each step: window.scrollBy(0, stepPx) and await 33ms
- Total scroll time per page: 8-12 seconds regardless of page height. Compute
  stepPx = scrollHeight / (durationMs / 33).
- Use an ease-in-out curve, not constant speed: slow at the start, faster in the middle,
  slow at the end. Roughly easeInOutCubic(t).
- Pause 600ms at the bottom before moving on.
- If the page uses scroll-reveal animations, this pacing lets them fire naturally.

MULTI-PAGE
- After the first page finishes, collect the top-level nav links (header nav only):
    page.$$eval('header a, nav a', as => as.map(a => ({text:a.textContent.trim(), href:a.href})))
- Filter to same-origin links, dedupe by href, drop anything with '#', 'login', 'signin',
  'cart', 'checkout', 'privacy', 'terms' or 'contact' in the text/href.
- Visit at most THREE of them. On each: wait 1500ms, then scroll the whole page the same way.
- If there is no usable nav (single-page site), just do the one scroll.

OUTPUT
- One video per site, named exactly <slug>.mp4, 1280x800, H.264, no audio,
  CRF 30, target under 2.5 MB each. Convert Playwright's .webm with ffmpeg:
    ffmpeg -y -i raw/<file>.webm -vf "scale=1280:-2,fps=30" -c:v libx264 -crf 30 -preset slow -an -movflags +faststart videos/<slug>.mp4
- Also export a poster frame (first frame after load) as videos/<slug>.jpg at 1280x800.
- If a page fails to load or produces a near-blank recording (check the mean pixel value —
  anything above 250 mean for 2 consecutive frames means a white page), skip it, log it,
  and continue. Do not invent a video.

DELIVERABLE
A ZIP with:
  videos/<slug>.mp4    (the recording)
  videos/<slug>.jpg    (the poster frame)
  failures.txt         (one line per site that produced nothing usable)

SITES (slug — url)
hasan            https://hsf237.github.io/hasan/
al-awaans        https://leens.online
coreui           https://core-ui-kit-v2-3.vercel.app
tisk             https://tiskems.edu.in
meridian         https://meridianarchstudio.com
carzo            https://carzocrs.vercel.app
edusphere        https://eduspere-beta.vercel.app
tech-orbit       https://hsf237.github.io/tech-orbit/
muttamsouq       https://hsf237.github.io/muttamsouq.com/
ryzestore        https://ryzestore.vercel.app
rendezvous       https://hsf237.github.io/rendezvous.com/
g-zel            https://hsf237.github.io/g-zel.com/
ruchifood        https://hsf237.github.io/ruchifood/
sol-harbor       https://hsf237.github.io/sol-harbor/
kerala-bus       https://kerala-bus-time-report.vercel.app
pulsegrid        https://pulsegrid-jet.vercel.app
aifinder         https://aifinder-phi.vercel.app
veloop           https://veloop-exchange-center.vercel.app
elitewebstudio   https://hsf237.github.io/elitewebstudio.in/
fahadsportfolio  https://hsf237.github.io/fahadsportfolio/
sinansportfolio  https://hsf237.github.io/sinansportfolio/

KNOWN PROBLEMS — expect these to fail, log them and move on:
- carzo and tech-orbit render a blank white page in a clean browser after 15s.
- sol-harbor is a WebGL canvas; video capture of a canvas works in Chromium but check the
  frames are not black.
- Pages served from vercel.app may take 3-5s to hydrate; keep the 2000ms wait after networkidle.

If you cannot run a browser in your environment, do not guess: output the complete,
runnable Node script (with package.json) so I can run it locally, and say so.
```

---

## What happens when you send me the ZIP

1. I re-encode anything oversized and normalise every clip to 1280×800 / H.264.
2. The project detail view swaps the animated tall screenshot for
   `<video autoplay muted loop playsinline poster="...">`, keeping the tall JPEG as the
   poster and as the no-video fallback.
3. The three stills stay where they are.
4. Anything in `failures.txt` keeps the current scroll-image version, so nothing breaks.

## Size budget

21 videos × ~2 MB = ~42 MB in the repo. GitHub Pages serves that fine; a git repo with
40 MB of video is acceptable but not ideal. If it climbs past ~80 MB I'll move the videos
to a `/media` branch or an external host and keep only the posters in the main repo.
