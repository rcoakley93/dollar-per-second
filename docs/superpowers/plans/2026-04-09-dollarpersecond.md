# dollarpersecond.com Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Build a single-page vanilla JS site where users enter their yearly salary and watch a live counter show how much they've earned since Jan 1 of the current year.

**Architecture:** Single `index.html` file with embedded CSS and JS, two UI states (input and counter), no build step, no dependencies. The counter recalculates from the live clock every 100ms to avoid drift.

**Tech Stack:** Vanilla HTML5, CSS3, JavaScript (ES6+). Deployed as a static site on Cloudflare Pages via GitHub.

---

## File Structure

```
dollar-per-second/
├── index.html                              # entire app — markup, styles, logic
├── docs/
│   └── superpowers/
│       ├── specs/
│       │   └── 2026-04-09-dollarpersecond-design.md   # already written
│       └── plans/
│           └── 2026-04-09-dollarpersecond.md          # copy of this plan
```

---

### Task 1: Initialize Git Repo

**Files:**
- Create: `index.html` (empty scaffold)
- Create: `.gitignore`

- [ ] **Step 1: Init git and create .gitignore**

```bash
cd /Users/ryancoakley/Documents/Projects/dollar-per-second
git init
echo ".DS_Store" > .gitignore
```

- [ ] **Step 2: Create empty index.html scaffold**

Create `index.html`:
```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Dollar Per Second</title>
</head>
<body>
  <!-- content coming in next tasks -->
</body>
</html>
```

- [ ] **Step 3: Initial commit**

```bash
git add index.html .gitignore
git commit -m "chore: initialize project"
```

Expected: commit succeeds, `git log --oneline` shows one commit.

---

### Task 2: Add CSS — Layout and Both UI States

**Files:**
- Modify: `index.html` — add `<style>` block in `<head>`

- [ ] **Step 1: Add full CSS to index.html `<head>`**

Replace the empty `<head>` style area with:
```html
<style>
  *, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }

  :root {
    --bg: #ffffff;
    --text: #111111;
    --subtle: #888888;
    --accent: #2d7a4f;
    --font-sans: 'Inter', system-ui, -apple-system, sans-serif;
    --font-mono: 'JetBrains Mono', 'Fira Mono', monospace;
  }

  html, body {
    height: 100%;
    background: var(--bg);
    color: var(--text);
    font-family: var(--font-sans);
  }

  /* ── Shared centering wrapper ── */
  .screen {
    min-height: 100vh;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    padding: 2rem;
    transition: opacity 0.4s ease;
  }

  .screen.hidden {
    display: none;
  }

  /* ── Input screen ── */
  #input-screen label {
    font-size: 0.875rem;
    letter-spacing: 0.08em;
    text-transform: uppercase;
    color: var(--subtle);
    margin-bottom: 1rem;
    display: block;
    text-align: center;
  }

  #salary-input {
    font-size: 2rem;
    font-family: var(--font-sans);
    border: none;
    border-bottom: 2px solid var(--text);
    outline: none;
    text-align: center;
    width: 100%;
    max-width: 320px;
    padding: 0.5rem 0;
    background: transparent;
    color: var(--text);
    /* hide browser spinner arrows */
    -moz-appearance: textfield;
  }
  #salary-input::-webkit-outer-spin-button,
  #salary-input::-webkit-inner-spin-button {
    -webkit-appearance: none;
  }

  #salary-input::placeholder {
    color: #cccccc;
  }

  #start-btn {
    margin-top: 1.5rem;
    padding: 0.75rem 2rem;
    font-size: 0.875rem;
    letter-spacing: 0.08em;
    text-transform: uppercase;
    background: var(--text);
    color: var(--bg);
    border: none;
    cursor: pointer;
    font-family: var(--font-sans);
    transition: opacity 0.2s;
  }
  #start-btn:hover { opacity: 0.75; }

  /* ── Counter screen ── */
  #counter-screen {
    gap: 0.5rem;
  }

  #earned {
    font-family: var(--font-mono);
    font-size: clamp(2.5rem, 8vw, 5rem);
    color: var(--accent);
    letter-spacing: -0.02em;
    line-height: 1;
  }

  #since-label {
    font-size: 0.8125rem;
    letter-spacing: 0.1em;
    text-transform: uppercase;
    color: var(--subtle);
    margin-bottom: 2.5rem;
  }

  #stats {
    display: flex;
    gap: 2.5rem;
    border-top: 1px solid #e5e5e5;
    padding-top: 1.5rem;
    flex-wrap: wrap;
    justify-content: center;
  }

  .stat {
    display: flex;
    flex-direction: column;
    align-items: center;
    gap: 0.25rem;
  }

  .stat-value {
    font-size: 1.25rem;
    font-family: var(--font-mono);
    color: var(--text);
  }

  .stat-label {
    font-size: 0.75rem;
    letter-spacing: 0.08em;
    text-transform: uppercase;
    color: var(--subtle);
  }

  /* ── Reset link ── */
  #reset-btn {
    position: fixed;
    bottom: 1.5rem;
    right: 1.5rem;
    font-size: 0.75rem;
    letter-spacing: 0.08em;
    text-transform: uppercase;
    color: var(--subtle);
    background: none;
    border: none;
    cursor: pointer;
    font-family: var(--font-sans);
    text-decoration: underline;
    text-underline-offset: 3px;
  }
  #reset-btn:hover { color: var(--text); }
</style>
```

Also add Google Fonts link in `<head>` before the style block:
```html
<link rel="preconnect" href="https://fonts.googleapis.com" />
<link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500&family=JetBrains+Mono:wght@400&display=swap" rel="stylesheet" />
```

- [ ] **Step 2: Verify visually**

Open `index.html` in browser. Page should be blank white (no content yet). No console errors.

- [ ] **Step 3: Commit**

```bash
git add index.html
git commit -m "style: add full CSS for both UI states"
```

---

### Task 3: Add HTML — Input and Counter Markup

**Files:**
- Modify: `index.html` — add markup inside `<body>`

- [ ] **Step 1: Add both screens to `<body>`**

Replace `<!-- content coming in next tasks -->` with:
```html
<!-- Input screen -->
<div id="input-screen" class="screen">
  <label for="salary-input">Your yearly salary</label>
  <input
    id="salary-input"
    type="number"
    placeholder="100000"
    min="1"
    autocomplete="off"
  />
  <button id="start-btn">Start</button>
</div>

<!-- Counter screen -->
<div id="counter-screen" class="screen hidden">
  <div id="earned">$0.00</div>
  <div id="since-label">earned since January 1</div>

  <div id="stats">
    <div class="stat">
      <span class="stat-value" id="per-day">—</span>
      <span class="stat-label">per day</span>
    </div>
    <div class="stat">
      <span class="stat-value" id="per-hour">—</span>
      <span class="stat-label">per hour</span>
    </div>
    <div class="stat">
      <span class="stat-value" id="per-minute">—</span>
      <span class="stat-label">per minute</span>
    </div>
  </div>
</div>

<button id="reset-btn" class="hidden">start over</button>
```

- [ ] **Step 2: Verify visually**

Open/reload `index.html`. Should see:
- Centered "your yearly salary" label
- Large number input
- "Start" button below it
- Counter screen invisible (hidden class)

- [ ] **Step 3: Commit**

```bash
git add index.html
git commit -m "feat: add HTML markup for input and counter screens"
```

---

### Task 4: Add JS — Calculation Logic and Counter

**Files:**
- Modify: `index.html` — add `<script>` block before closing `</body>`

- [ ] **Step 1: Add the full script to `index.html`**

Add before `</body>`:
```html
<script>
  const inputScreen  = document.getElementById('input-screen');
  const counterScreen = document.getElementById('counter-screen');
  const salaryInput  = document.getElementById('salary-input');
  const startBtn     = document.getElementById('start-btn');
  const resetBtn     = document.getElementById('reset-btn');
  const earnedEl     = document.getElementById('earned');
  const perDayEl     = document.getElementById('per-day');
  const perHourEl    = document.getElementById('per-hour');
  const perMinEl     = document.getElementById('per-minute');

  let intervalId = null;

  function fmt(n) {
    return '$' + n.toLocaleString('en-US', {
      minimumFractionDigits: 2,
      maximumFractionDigits: 2
    });
  }

  function startCounter(salary) {
    const now = new Date();
    const startOfYear = new Date(now.getFullYear(), 0, 1); // Jan 1, 00:00:00
    const perSecond = salary / 365 / 24 / 3600;

    // Static derived rates
    perDayEl.textContent  = fmt(salary / 365);
    perHourEl.textContent = fmt(salary / 365 / 24);
    perMinEl.textContent  = fmt(salary / 365 / 24 / 60);

    // Live counter
    function tick() {
      const secondsElapsed = (Date.now() - startOfYear.getTime()) / 1000;
      earnedEl.textContent = fmt(secondsElapsed * perSecond);
    }

    tick(); // immediate first render
    intervalId = setInterval(tick, 100);
  }

  function showCounter(salary) {
    inputScreen.classList.add('hidden');
    counterScreen.classList.remove('hidden');
    resetBtn.classList.remove('hidden');
    startCounter(salary);
  }

  function reset() {
    clearInterval(intervalId);
    intervalId = null;
    counterScreen.classList.add('hidden');
    resetBtn.classList.add('hidden');
    inputScreen.classList.remove('hidden');
    salaryInput.value = '';
    salaryInput.focus();
  }

  function handleStart() {
    const salary = parseFloat(salaryInput.value);
    if (!salary || salary <= 0) {
      salaryInput.focus();
      return;
    }
    showCounter(salary);
  }

  startBtn.addEventListener('click', handleStart);

  salaryInput.addEventListener('keydown', function(e) {
    if (e.key === 'Enter') handleStart();
  });

  resetBtn.addEventListener('click', reset);
</script>
```

- [ ] **Step 2: Verify behavior manually**

Open/reload `index.html` in browser:
1. Enter `100000`, press Start
   - Counter screen appears
   - Counter shows a dollar amount (for April 9 ~99 days in: ~$27,123)
   - Amount increments visibly every 100ms
   - Per day shows `$273.97`, per hour `$11.42`, per minute `$0.19`
2. Click "start over"
   - Returns to input screen, input is empty and focused
3. Press Enter instead of clicking Start — same result as step 1
4. Leave input empty and press Start — nothing happens, input gets focus

- [ ] **Step 3: Commit**

```bash
git add index.html
git commit -m "feat: add counter logic and state transitions"
```

---

### Task 5: Save Plan and Push to GitHub

**Files:**
- Create: `docs/superpowers/plans/2026-04-09-dollarpersecond.md` (copy of this plan)

- [ ] **Step 1: Create plan directory and save plan**

```bash
mkdir -p docs/superpowers/plans
```

Copy this plan file into `docs/superpowers/plans/2026-04-09-dollarpersecond.md`.

- [ ] **Step 2: Commit docs**

```bash
git add docs/
git commit -m "docs: add design spec and implementation plan"
```

- [ ] **Step 3: Create GitHub repo and push**

```bash
gh repo create dollar-per-second --public --source=. --remote=origin --push
```

If `gh` is not authenticated, run `gh auth login` first.

Expected: repo created at `github.com/<username>/dollar-per-second`, all commits pushed.

---

### Task 6: Deploy to Cloudflare Pages

No code changes — this is a manual setup step.

- [ ] **Step 1: Log in to Cloudflare dashboard**

Go to https://dash.cloudflare.com → Pages → Create a project → Connect to Git

- [ ] **Step 2: Connect GitHub repo**

Select `dollar-per-second` repo. Configure:
- **Framework preset:** None
- **Build command:** (leave empty)
- **Build output directory:** `/` (root)

- [ ] **Step 3: Deploy**

Click "Save and Deploy". Wait for deployment to complete. Cloudflare will provide a `*.pages.dev` preview URL — verify the site works there.

- [ ] **Step 4: Add custom domain**

In Cloudflare Pages → Custom domains → Add `dollarpersecond.com`.

Since the domain is already on Cloudflare DNS, it will auto-configure. Wait for DNS propagation (usually instant within Cloudflare).

- [ ] **Step 5: Verify production**

Open https://dollarpersecond.com, enter a salary, confirm counter works live.

---

## Verification Checklist

- [ ] Enter $100,000 — per-day shows `$273.97`, per-hour `$11.42`, per-minute `$0.19`
- [ ] Counter value at ~99 days elapsed (April 9) ≈ `$27,123` ± a few dollars
- [ ] Counter increments visibly (smooth, no jumps)
- [ ] Enter key submits salary
- [ ] Empty input submission does nothing
- [ ] "start over" resets and focuses input
- [ ] Site works on mobile (responsive layout)
- [ ] No console errors
- [ ] Live on dollarpersecond.com
