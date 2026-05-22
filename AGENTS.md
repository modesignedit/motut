# MODIAMOND16 HIIT Visualizer — Agent Guide

Single-file (700-line) HTML/CSS/JS PWA. No build tools, no deps, no package.json.

## Run
- **Locally:** open `hiit-modiamond16.html` in any browser (works from `file://`)
- **Prod:** `https://motut.vercel.app/` — auto-deploys on push to `main`
- **Vercel config:** `vercel.json` rewrites all routes to `hiit-modiamond16.html`

## Architecture
- **Data:** `PROGS` (6 programs) + `EX` (31 exercises) in `<script>` — arrays of objects with exercise IDs as the linking key
- **State:** single `S` object — `cur`, `play`, `sesh`, `rem`, `tot`, `rest`, `mute`, etc.
- **Figure poses:** each exercise has `p[]` (frame array, each with `{lA,rA,lF,rF,lL,rL,lC,rC,bY,ln}`) and `dr[]` (ms per frame). Interpolated in `updF()` driven by `requestAnimationFrame` in `main()`
- **Timer:** `setInterval(tick,100)` — counts down 0.1s ticks, updates SVG ring via `setPct()`
- **Theme:** `:root` = dark, `:root[data-theme="light"]` = light. `setAttribute`/`removeAttribute('data-theme','light')` to toggle. **Do not use `toggleAttribute` with a value — it won't remove the attribute.**

## CSS breakpoints
- `≤800px` — mobile layout (viewer fixed at bottom)
- `≤500px` — compact viewer (figure hidden, timer/text scaled)
- `≤400px` — small phones (further compaction)
- `≤360px` — fallback (2-col grid)

## localStorage keys
| Key | Type | Purpose |
|-----|------|---------|
| `md16_mute` | `'true'`/`'false'` | Sound mute toggle |
| `md16_hist` | JSON array | Session history (max 200) |
| `md16_seen` | `'1'` | Splash already shown |
| `md16_light` | `'true'`/`'false'` | Theme preference |

## Conventions
- **No comments** in code — all docs are in the commit history and this file
- **CSS:** minified (one-line per rule), all CSS vars on `:root`, category colors in `C` dict
- **JS:** `const $=id=>document.getElementById(id)`
- **Descriptions:** all exercise desc fields prefixed with `✓ ` when displayed (`'✓ '+e.desc`)
- **Social links:** Instagram `@modiamond16`, Telegram `@modiamondfitness`
- **Icon:** `favicon.svg` = "MOtut" letter mark, `icon.svg` = diamond (PWA 512×512)

## Gotchas
- `pick(id)` calls `renderGrid()` which resets DOM — re-apply search filter afterward
- Confetti cleanup timeout stored in `cT` — clear on program switch
- Service worker registers on HTTPS only (silently fails on `file://`)
- `navigator.vibrate(6)` used for haptic — silently fails if unavailable
- `AudioContext` created lazily on first `beep()` call (avoids autoplay block)
- `updPad()` recalculates grid bottom padding on resize to match viewer height

## Git
- Remote: `https://github.com/modesignedit/motut.git`
- Branch: `main`
- Author: `Moses Orji <mosixberyl@gmail.com>`
- Vercel auto-deploys on push to `main`
