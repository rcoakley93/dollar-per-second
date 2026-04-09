# dollarpersecond.com — Design Spec

**Date:** 2026-04-09

## Context

The user owns the domain dollarpersecond.com. The concept: enter your yearly salary and watch a live counter show how much you've earned so far this calendar year, down to the second. It also displays static derived rates (per day, hour, minute). Simple, fun, shareable. Deploys to Cloudflare Pages from GitHub.

---

## Tech Stack

- **Vanilla HTML/CSS/JS** — single `index.html`, no build step, no dependencies
- **Cloudflare Pages** — static hosting, connect GitHub repo, auto-deploy on push

---

## File Structure

```
dollar-per-second/
├── index.html
└── favicon.ico   (optional)
```

---

## Two UI States

### 1. Input State (on load)

- Centered vertically and horizontally on the page
- Label: "your yearly salary"
- Large number input (no spinner arrows via CSS)
- Submit on Enter key or button click
- Input fades out on submit, counter fades in

### 2. Counter State (after salary entered)

- **Running total** — large monospaced font, updates every 100ms
  - Format: `$12,847.63`
- **Subtitle** — "earned since January 1" beneath the total
- **Stats panel** — three figures displayed side by side below the counter:
  - `$X,XXX / day`
  - `$XXX / hour`
  - `$XX / minute`
  - These are static (calculated once from salary, do not tick)
- **"start over" link** — small, bottom corner, resets to input state

---

## Calculation Logic

```
perSecond  = salary / 365 / 24 / 3600
startOfYear = new Date(currentYear, 0, 1)   // Jan 1, 00:00:00
secondsElapsed = (Date.now() - startOfYear) / 1000
earned = secondsElapsed × perSecond

perDay    = salary / 365
perHour   = salary / 365 / 24
perMinute = salary / 365 / 24 / 60
```

- Counter recalculates from live clock every 100ms — no drift, no accumulated error
- 365-day year, no weekends/holidays adjustment

---

## Visual Design

- **Background:** white
- **Text:** near-black (#111 or similar)
- **Running total accent:** muted green (e.g. #2d7a4f or similar)
- **Typography:** geometric sans-serif (system font stack or a single Google Font like Inter)
- **Monospace** for the running total number only
- Clean, minimal — no gradients, no shadows, no decorative elements

---

## Deployment

1. Init git repo locally
2. Push to GitHub (new public or private repo)
3. Connect repo to Cloudflare Pages
4. Build command: none (static)
5. Output directory: `/` (root)
6. Custom domain: dollarpersecond.com (configured in Cloudflare DNS)

---

## Verification

- Open `index.html` locally in a browser — no server needed
- Enter a salary (e.g. $100,000) and confirm:
  - Counter starts at correct value (days elapsed × $273.97/day)
  - Counter increments visibly every 100ms
  - Per-day = $273.97, per-hour = $11.42, per-minute = $0.19
  - "start over" resets to input form
- Deploy to Cloudflare Pages and confirm live on dollarpersecond.com
