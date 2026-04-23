# Urban Greening Template Implementation Plan

> **For agentic workers:** REQUIRED: Use superpowers:subagent-driven-development (if subagents available) or superpowers:executing-plans to implement this plan. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Add a new `urban-greening` learning template — a variant of `interactive-image` where pins are detailed, realistic SVG icons (ladybug, tree, beehive, flowerbed) defined once as `<symbol>`s and referenced via `<use>`. Ships with a worked example of a greened city block.

**Architecture:** Two new self-contained HTML files (`urban-greening/urban-greening-template.html` and `urban-greening/examples/greening-a-city-block.html`) plus an update to `CLAUDE.md`. No build tools. Reuses the existing popup open/close JS from `interactive-image` unchanged.

**Tech Stack:** Vanilla HTML, CSS, SVG (`<symbol>` + `<use>`), vanilla JavaScript (`var`, `querySelectorAll`, `addEventListener`). No frameworks, no build tools, no internet.

**Spec:** `docs/superpowers/specs/2026-04-23-urban-greening-design.md`

**Verification model:** This repo has no automated test suite — templates are verified by opening the HTML file directly in a browser and checking behaviour. Each task's "verify" step describes what to see and try.

---

## Chunk 1: Blank template (`urban-greening/urban-greening-template.html`)

### Task 1: Create the template scaffold

**Files:**
- Create: `urban-greening/urban-greening-template.html`

Create the folder and a scaffold file with the full page shell, CSS variables, styles for everything except pins, the instructions banner, and an empty image area. Pins, icon library, and JS are added in later tasks.

- [ ] **Step 1: Create the file with the following contents**

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Urban Greening</title>
  <style>
    /* ============================================================
       CUSTOMISATION ZONE — Edit these variables freely!
       ============================================================ */
    :root {
      --accent:       #4caf50;       /* Accent / highlight colour      */
      --accent-light: #66bb6a;       /* Hover / focus colour           */
      --popup-bg:     #1a2e1a;       /* Popup background               */
      --popup-text:   #eaeaea;       /* Popup body text                */
      --btn-text:     #ffffff;       /* Text on buttons                */
      --page-bg:      #0d140d;       /* Page background                */
      --icon-size:    56px;          /* Pin icon width / height        */
      --font-title:   Georgia, serif;
      --font-body:    'Segoe UI', Arial, sans-serif;
    }

    /* ============================================================
       PAGE SHELL — generally leave alone
       ============================================================ */
    *, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }

    body {
      font-family: var(--font-body);
      background: var(--page-bg);
      color: var(--popup-text);
      min-height: 100vh;
      display: flex;
      flex-direction: column;
      align-items: center;
    }

    header {
      width: 100%;
      padding: 1.2rem 2rem;
      text-align: center;
      letter-spacing: .12em;
      text-transform: uppercase;
      font-family: var(--font-title);
      font-size: 1.5rem;
      border-bottom: 2px solid var(--accent);
      background: var(--page-bg);
    }

    /* ============================================================
       IMAGE CONTAINER
       ============================================================ */
    .image-wrap {
      position: relative;
      width: 900px;
      max-width: 98vw;
      margin: 2rem auto;
    }

    .bg-image {
      display: block;
      width: 100%;
      height: auto;
      border: 3px solid var(--accent);
      border-radius: 4px;
    }

    /* ============================================================
       POPUPS
       ============================================================ */
    .popup {
      display: none;
      position: absolute;
      z-index: 10;
      width: 260px;
      background: var(--popup-bg);
      border: 2px solid var(--accent);
      border-radius: 6px;
      box-shadow: 0 8px 30px rgba(0,0,0,.7);
      overflow: hidden;
      animation: popIn .2s ease;
    }
    .popup.active { display: block; }

    @keyframes popIn {
      from { opacity: 0; transform: scale(.92); }
      to   { opacity: 1; transform: scale(1);   }
    }

    .popup-header {
      background: var(--accent);
      padding: .6rem 1rem;
      display: flex;
      justify-content: space-between;
      align-items: center;
    }
    .popup-header h2 {
      font-family: var(--font-title);
      font-size: 1rem;
      letter-spacing: .05em;
    }
    .close-btn {
      background: none;
      border: none;
      color: #fff;
      font-size: 1.2rem;
      line-height: 1;
      cursor: pointer;
      padding: 0 .2rem;
    }
    .popup-body {
      padding: .8rem 1rem 1rem;
      font-size: .85rem;
      line-height: 1.6;
    }
    .popup-body img {
      width: 100%;
      border-radius: 3px;
      margin-bottom: .6rem;
    }

    /* ============================================================
       INSTRUCTION BANNER  (students: delete this when done)
       ============================================================ */
    .instructions {
      width: 900px;
      max-width: 98vw;
      margin: 0 auto 2rem;
      padding: 1rem 1.4rem;
      background: #1e2a1e;
      border-left: 4px solid var(--accent);
      border-radius: 0 4px 4px 0;
      font-size: .82rem;
      line-height: 1.7;
    }
    .instructions h2 {
      font-size: .9rem;
      text-transform: uppercase;
      letter-spacing: .06em;
      color: var(--accent-light);
      margin-bottom: .5rem;
    }
    .instructions ol { padding-left: 1.4rem; }
    .instructions li { margin-bottom: .35rem; }
    .instructions strong { color: var(--accent-light); }
    .instructions code {
      background: #2a3a2a;
      padding: .1em .35em;
      border-radius: 3px;
      font-size: .9em;
    }

    footer {
      margin-top: auto;
      padding: 1rem;
      font-size: .75rem;
      opacity: .4;
      text-align: center;
    }
  </style>
</head>
<body>

<!-- ============================================================
     TITLE — change this text
     ============================================================ -->
<header>My Urban Greening Scene</header>


<!-- ============================================================
     INSTRUCTIONS BANNER
     Delete the entire <div class="instructions"> block when done.
     ============================================================ -->
<div class="instructions">
  <h2>How to set up your Urban Greening scene</h2>
  <ol>
    <li><strong>Background image:</strong> Place your image file (a street, schoolyard, park, etc.) in the same folder as this HTML file, then change <code>src="placeholder.svg"</code> on the <code>&lt;img class="bg-image"&gt;</code> tag to your filename, e.g. <code>src="my-street.jpg"</code>.</li>
    <li><strong>Add a pin:</strong> Inside <code>&lt;div class="image-wrap"&gt;</code>, copy any <code>&lt;button class="pin-btn"&gt;</code> block. Give it a unique <code>data-popup</code> value, set <code>top</code> / <code>left</code> percentages to place it on the image, pick an icon by editing <code>&lt;use href="#icon-..."/&gt;</code> (choose from <code>#icon-bug</code>, <code>#icon-tree</code>, <code>#icon-beehive</code>, <code>#icon-garden</code>), and update the <code>aria-label</code>.</li>
    <li><strong>Add a popup:</strong> Copy any <code>&lt;div class="popup"&gt;</code> block, match its <code>id</code> to the button's <code>data-popup</code>, and fill in your title &amp; text.</li>
    <li><strong>Add a new icon (optional):</strong> In the icon library near the top of <code>&lt;body&gt;</code>, copy a <code>&lt;symbol&gt;</code> block and give it a unique <code>id</code>. Then reference it from any pin.</li>
    <li><strong>Change colours &amp; fonts:</strong> Edit the CSS variables in the <code>:root { }</code> block. Icon colours live inside each <code>&lt;symbol&gt;</code>, not as variables.</li>
    <li><strong>Delete this instructions box</strong> when you are done.</li>
  </ol>
</div>


<!-- ============================================================
     ICON LIBRARY — added in a later step
     ============================================================ -->


<!-- ============================================================
     IMAGE AREA
     ============================================================ -->
<div class="image-wrap">

  <!-- BACKGROUND IMAGE — replace src with your file -->
  <img class="bg-image"
       src="placeholder.svg"
       alt="Background image"
       onerror="this.src=this.getAttribute('data-fallback')"
       data-fallback="data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' width='900' height='500'%3E%3Crect width='900' height='500' fill='%23162816'/%3E%3Ctext x='50%25' y='48%25' font-family='Georgia' font-size='22' fill='%234caf50' text-anchor='middle' dominant-baseline='middle'%3EReplace src with your image%3C/text%3E%3Ctext x='50%25' y='57%25' font-family='Segoe UI' font-size='13' fill='%23779977' text-anchor='middle' dominant-baseline='middle'%3Ee.g. src=%22my-street.jpg%22%3C/text%3E%3C/svg%3E">

  <!-- Pins and popups are added in later tasks -->

</div><!-- end .image-wrap -->

<footer>Urban Greening Template | no internet required</footer>

<!-- JavaScript is added in a later task -->

</body>
</html>
```

- [ ] **Step 2: Verify in a browser**

Open `urban-greening/urban-greening-template.html` in a browser.
Expected: green-themed page with title "My Urban Greening Scene," an instructions banner, a green-bordered image area showing the "Replace src with your image" placeholder, and a footer. No pins yet — that's correct for this task.

- [ ] **Step 3: Commit**

```bash
git add urban-greening/urban-greening-template.html
git commit -m "feat(urban-greening): add template scaffold with page shell and instructions"
```

---

### Task 2: Add the SVG icon library

**Files:**
- Modify: `urban-greening/urban-greening-template.html`

Insert a hidden `<svg>` containing four `<symbol>` elements (bug, tree, beehive, garden) just before the `<div class="image-wrap">` block.

- [ ] **Step 1: Replace the `<!-- ICON LIBRARY --> ...` placeholder comment**

Find this block in the file:

```html
<!-- ============================================================
     ICON LIBRARY — added in a later step
     ============================================================ -->
```

Replace it with:

```html
<!-- ============================================================
     ICON LIBRARY

     Four reusable SVG icons drawn as <symbol>s. Each pin button
     below references one via <use href="#icon-..."/>.

     Available icons:
       #icon-bug       — ladybug
       #icon-tree      — broadleaf tree
       #icon-beehive   — traditional woven skep hive
       #icon-garden    — flowerbed / planter

     HOW TO ADD A NEW ICON
     ─────────────────────
     1. Copy one of the <symbol> blocks below
     2. Give it a new unique id, e.g. id="icon-butterfly"
     3. Redraw the shapes inside
     4. Reference it from a pin with <use href="#icon-butterfly"/>

     HOW TO CHANGE COLOURS OF AN ICON
     ────────────────────────────────
     Colours are baked into each <symbol> via the fill="#..."
     attributes. Edit the fills directly. These icons are designed
     to look realistic, so they do NOT follow the --accent CSS
     variable.
     ============================================================ -->
<svg xmlns="http://www.w3.org/2000/svg" style="position:absolute; width:0; height:0; overflow:hidden;" aria-hidden="true">
  <defs>

    <!-- LADYBUG -->
    <symbol id="icon-bug" viewBox="0 0 64 64">
      <!-- Legs -->
      <path d="M 18 42 L 8 48 M 46 42 L 56 48 M 20 32 L 6 32 M 44 32 L 58 32 M 20 22 L 10 14 M 44 22 L 54 14"
            stroke="#111" stroke-width="2" stroke-linecap="round" fill="none"/>
      <!-- Red shell -->
      <ellipse cx="32" cy="34" rx="20" ry="17" fill="#c62828"/>
      <!-- Shell highlight -->
      <ellipse cx="24" cy="26" rx="6" ry="3" fill="#ef5350" opacity="0.6"/>
      <!-- Wing division -->
      <path d="M 32 18 L 32 50" stroke="#111" stroke-width="1.5"/>
      <!-- Spots -->
      <circle cx="22" cy="28" r="2.6" fill="#111"/>
      <circle cx="42" cy="28" r="2.6" fill="#111"/>
      <circle cx="20" cy="40" r="2.2" fill="#111"/>
      <circle cx="44" cy="40" r="2.2" fill="#111"/>
      <circle cx="32" cy="44" r="2.2" fill="#111"/>
      <!-- Head -->
      <ellipse cx="32" cy="18" rx="9" ry="7" fill="#111"/>
      <!-- Eyes -->
      <circle cx="28" cy="17" r="1.3" fill="#fff"/>
      <circle cx="36" cy="17" r="1.3" fill="#fff"/>
      <!-- Antennae -->
      <path d="M 28 12 Q 24 6 22 5 M 36 12 Q 40 6 42 5" stroke="#111" stroke-width="1.5" fill="none" stroke-linecap="round"/>
      <circle cx="22" cy="5" r="1.5" fill="#111"/>
      <circle cx="42" cy="5" r="1.5" fill="#111"/>
    </symbol>

    <!-- TREE -->
    <symbol id="icon-tree" viewBox="0 0 64 64">
      <!-- Ground shadow -->
      <ellipse cx="32" cy="60" rx="18" ry="2.5" fill="#000" opacity="0.25"/>
      <!-- Trunk -->
      <rect x="28" y="40" width="8" height="20" fill="#6d4c2e"/>
      <rect x="34" y="40" width="2" height="20" fill="#4a3219"/>
      <!-- Bark detail -->
      <path d="M 30 44 Q 31 48 30 52 M 33 42 Q 34 50 33 58" stroke="#4a3219" stroke-width="0.8" fill="none"/>
      <!-- Canopy layers (dark to light) -->
      <ellipse cx="32" cy="30" rx="24" ry="20" fill="#1b5e20"/>
      <ellipse cx="24" cy="22" rx="13" ry="12" fill="#2e7d32"/>
      <ellipse cx="40" cy="24" rx="12" ry="11" fill="#2e7d32"/>
      <ellipse cx="32" cy="16" rx="11" ry="10" fill="#43a047"/>
      <!-- Leaf highlights -->
      <circle cx="22" cy="16" r="2" fill="#81c784" opacity="0.8"/>
      <circle cx="38" cy="12" r="2" fill="#81c784" opacity="0.8"/>
      <circle cx="44" cy="22" r="2" fill="#81c784" opacity="0.8"/>
      <circle cx="18" cy="28" r="2" fill="#81c784" opacity="0.8"/>
      <circle cx="30" cy="10" r="1.5" fill="#c8e6c9" opacity="0.9"/>
    </symbol>

    <!-- BEEHIVE (woven skep) -->
    <symbol id="icon-beehive" viewBox="0 0 64 64">
      <!-- Ground shadow -->
      <ellipse cx="32" cy="58" rx="22" ry="2.5" fill="#000" opacity="0.25"/>
      <!-- Base plank -->
      <rect x="8" y="52" width="48" height="5" fill="#5d4037" rx="1"/>
      <!-- Skep rows from bottom to top -->
      <ellipse cx="32" cy="50" rx="22" ry="5" fill="#8d6e3a"/>
      <ellipse cx="32" cy="44" rx="21" ry="5" fill="#a9843d"/>
      <ellipse cx="32" cy="38" rx="19" ry="5" fill="#8d6e3a"/>
      <ellipse cx="32" cy="32" rx="16" ry="5" fill="#a9843d"/>
      <ellipse cx="32" cy="26" rx="13" ry="5" fill="#8d6e3a"/>
      <ellipse cx="32" cy="20" rx="9" ry="4" fill="#a9843d"/>
      <ellipse cx="32" cy="15" rx="5" ry="3" fill="#8d6e3a"/>
      <!-- Top knob -->
      <circle cx="32" cy="12" r="2" fill="#5d4037"/>
      <!-- Entrance -->
      <ellipse cx="32" cy="50" rx="3" ry="3.5" fill="#2a1a0a"/>
      <!-- Bee 1 (right) -->
      <g transform="translate(50 26)">
        <ellipse cx="0" cy="0" rx="3" ry="2" fill="#f9a825"/>
        <path d="M -1.5 -1.5 L -1.5 1.5 M 1 -1.5 L 1 1.5" stroke="#111" stroke-width="0.9"/>
        <ellipse cx="1" cy="-2" rx="2" ry="1.2" fill="#eceff1" opacity="0.85"/>
      </g>
      <!-- Bee 2 (left) -->
      <g transform="translate(12 34)">
        <ellipse cx="0" cy="0" rx="2.5" ry="1.7" fill="#f9a825"/>
        <path d="M -1.2 -1.3 L -1.2 1.3 M 0.8 -1.3 L 0.8 1.3" stroke="#111" stroke-width="0.8"/>
        <ellipse cx="-0.5" cy="-1.6" rx="1.6" ry="1" fill="#eceff1" opacity="0.85"/>
      </g>
    </symbol>

    <!-- GARDEN (planter with flowers) -->
    <symbol id="icon-garden" viewBox="0 0 64 64">
      <!-- Ground shadow -->
      <ellipse cx="32" cy="60" rx="26" ry="2.5" fill="#000" opacity="0.25"/>
      <!-- Planter body -->
      <rect x="6" y="44" width="52" height="14" fill="#6d4c2e" rx="1"/>
      <!-- Planter rim -->
      <rect x="4" y="42" width="56" height="4" fill="#8d6a44" rx="1"/>
      <!-- Planter wood grain -->
      <line x1="16" y1="46" x2="16" y2="57" stroke="#4a3219" stroke-width="0.6"/>
      <line x1="28" y1="46" x2="28" y2="57" stroke="#4a3219" stroke-width="0.6"/>
      <line x1="40" y1="46" x2="40" y2="57" stroke="#4a3219" stroke-width="0.6"/>
      <line x1="52" y1="46" x2="52" y2="57" stroke="#4a3219" stroke-width="0.6"/>
      <!-- Stems -->
      <line x1="14" y1="42" x2="14" y2="28" stroke="#2e7d32" stroke-width="1.8"/>
      <line x1="24" y1="42" x2="24" y2="20" stroke="#2e7d32" stroke-width="1.8"/>
      <line x1="34" y1="42" x2="34" y2="24" stroke="#2e7d32" stroke-width="1.8"/>
      <line x1="44" y1="42" x2="44" y2="18" stroke="#2e7d32" stroke-width="1.8"/>
      <line x1="52" y1="42" x2="52" y2="26" stroke="#2e7d32" stroke-width="1.8"/>
      <!-- Leaves -->
      <ellipse cx="17" cy="34" rx="3.5" ry="1.8" fill="#43a047" transform="rotate(30 17 34)"/>
      <ellipse cx="27" cy="30" rx="3.5" ry="1.8" fill="#43a047" transform="rotate(-30 27 30)"/>
      <ellipse cx="38" cy="30" rx="3.5" ry="1.8" fill="#43a047" transform="rotate(30 38 30)"/>
      <ellipse cx="47" cy="28" rx="3.5" ry="1.8" fill="#43a047" transform="rotate(-30 47 28)"/>
      <ellipse cx="49" cy="32" rx="3.5" ry="1.8" fill="#43a047" transform="rotate(30 49 32)"/>
      <!-- Flowers -->
      <!-- Pink -->
      <circle cx="14" cy="26" r="4" fill="#ec407a"/>
      <circle cx="14" cy="26" r="1.6" fill="#fff59d"/>
      <!-- Yellow -->
      <circle cx="24" cy="18" r="4" fill="#fdd835"/>
      <circle cx="24" cy="18" r="1.6" fill="#e65100"/>
      <!-- Purple -->
      <circle cx="34" cy="22" r="4" fill="#8e24aa"/>
      <circle cx="34" cy="22" r="1.6" fill="#fff59d"/>
      <!-- Orange -->
      <circle cx="44" cy="16" r="4" fill="#fb8c00"/>
      <circle cx="44" cy="16" r="1.6" fill="#fff59d"/>
      <!-- White -->
      <circle cx="52" cy="24" r="4" fill="#fafafa"/>
      <circle cx="52" cy="24" r="1.6" fill="#fdd835"/>
    </symbol>

  </defs>
</svg>
```

- [ ] **Step 2: Verify in a browser**

Reload the file. The page should look identical to Task 1 — the icon library is invisible (`width:0; height:0; overflow:hidden`). That's the correct behaviour.

To sanity-check the library is parsed, temporarily add this markup inside `<div class="image-wrap">`:

```html
<svg width="300" height="80" style="position:absolute; top:0; left:0;"><use href="#icon-bug" x="0"/><use href="#icon-tree" x="70"/><use href="#icon-beehive" x="140"/><use href="#icon-garden" x="210"/></svg>
```

You should see the four icons rendered in the top-left corner of the image area. Remove this temporary `<svg>` before committing.

- [ ] **Step 3: Commit**

```bash
git add urban-greening/urban-greening-template.html
git commit -m "feat(urban-greening): add SVG icon library (bug, tree, beehive, garden)"
```

---

### Task 3: Add pin CSS and sample pins/popups

**Files:**
- Modify: `urban-greening/urban-greening-template.html`

Add CSS for icon-only pin buttons (no background, drop-shadow, hover scale, focus ring) and replace the `<!-- Pins and popups ... -->` placeholder with four sample pins (one per icon) and matching popups.

- [ ] **Step 1: Add pin CSS**

Find this block inside `<style>`:

```css
    .bg-image {
      display: block;
      width: 100%;
      height: auto;
      border: 3px solid var(--accent);
      border-radius: 4px;
    }
```

Insert the following immediately after it:

```css
    /* ============================================================
       PINS

       Each pin is a <button> containing an <svg><use href="#icon-..."/></svg>.
       There is NO background box — the icon is the pin.

       HOW TO ADD A PIN
       ────────────────
       1. Copy a <button class="pin-btn"> block inside .image-wrap
       2. Give it a unique data-popup value
       3. Adjust top / left percentages to place it on the image
       4. Change <use href="#icon-..."/> to pick the icon
       5. Update aria-label to describe what the pin represents
       6. Add a matching <div class="popup" id="...">
       ============================================================ */
    .pin-btn {
      position: absolute;
      transform: translate(-50%, -50%);
      background: none;
      border: none;
      padding: 0;
      cursor: pointer;
      line-height: 0;                  /* remove inline-svg whitespace */
      transition: transform .15s ease, filter .15s ease;
      filter: drop-shadow(0 2px 4px rgba(0,0,0,.6));
    }
    .pin-icon {
      width: var(--icon-size);
      height: var(--icon-size);
      display: block;
      pointer-events: none;            /* clicks go to the button */
    }
    .pin-btn:hover {
      transform: translate(-50%, -50%) scale(1.15);
      filter: drop-shadow(0 4px 8px rgba(0,0,0,.75));
    }
    .pin-btn:focus-visible {
      outline: 3px solid var(--accent-light);
      outline-offset: 4px;
      border-radius: 4px;
    }
```

- [ ] **Step 2: Add sample pins and popups**

Find this line inside `<div class="image-wrap">`:

```html
  <!-- Pins and popups are added in later tasks -->
```

Replace it with:

```html
  <!-- ============================================================
       PINS — one sample per icon
       ============================================================ -->

  <!-- Pin 1 — Bug -->
  <button class="pin-btn"
          data-popup="popup1"
          aria-label="Bug location"
          style="top: 40%; left: 22%;">
    <svg class="pin-icon" viewBox="0 0 64 64" aria-hidden="true"><use href="#icon-bug"/></svg>
  </button>

  <!-- Pin 2 — Tree -->
  <button class="pin-btn"
          data-popup="popup2"
          aria-label="Tree location"
          style="top: 55%; left: 48%;">
    <svg class="pin-icon" viewBox="0 0 64 64" aria-hidden="true"><use href="#icon-tree"/></svg>
  </button>

  <!-- Pin 3 — Beehive -->
  <button class="pin-btn"
          data-popup="popup3"
          aria-label="Beehive location"
          style="top: 30%; left: 72%;">
    <svg class="pin-icon" viewBox="0 0 64 64" aria-hidden="true"><use href="#icon-beehive"/></svg>
  </button>

  <!-- Pin 4 — Garden -->
  <button class="pin-btn"
          data-popup="popup4"
          aria-label="Garden location"
          style="top: 70%; left: 80%;">
    <svg class="pin-icon" viewBox="0 0 64 64" aria-hidden="true"><use href="#icon-garden"/></svg>
  </button>

  <!-- ============================================================
       POPUPS
       ============================================================ -->

  <!-- Popup 1 — Bug -->
  <div class="popup" id="popup1" style="top: 10%; left: 5%;">
    <div class="popup-header">
      <h2>Bug</h2>
      <button class="close-btn" aria-label="Close">&#10005;</button>
    </div>
    <div class="popup-body">
      <p>Write your description of the <strong>bug</strong> here.
         You could describe the species, what it eats, why it is helpful
         in an urban space, or where it likes to live.</p>
    </div>
  </div>

  <!-- Popup 2 — Tree -->
  <div class="popup" id="popup2" style="top: 25%; left: 30%;">
    <div class="popup-header">
      <h2>Tree</h2>
      <button class="close-btn" aria-label="Close">&#10005;</button>
    </div>
    <div class="popup-body">
      <p>Write your description of the <strong>tree</strong> here.
         Consider its species, how much shade it provides, how it helps
         cool the street, and what birds or insects live in it.</p>
    </div>
  </div>

  <!-- Popup 3 — Beehive -->
  <div class="popup" id="popup3" style="top: 5%; left: 50%;">
    <div class="popup-header">
      <h2>Beehive</h2>
      <button class="close-btn" aria-label="Close">&#10005;</button>
    </div>
    <div class="popup-body">
      <p>Write your description of the <strong>beehive</strong> here.
         You might explain why bees matter for pollination and what plants
         in the surrounding area they visit.</p>
    </div>
  </div>

  <!-- Popup 4 — Garden -->
  <div class="popup" id="popup4" style="top: 45%; left: 50%;">
    <div class="popup-header">
      <h2>Garden</h2>
      <button class="close-btn" aria-label="Close">&#10005;</button>
    </div>
    <div class="popup-body">
      <p>Write your description of the <strong>garden</strong> here.
         Consider what is grown, who looks after it, and how it changes
         the feel of the street.</p>
    </div>
  </div>
```

- [ ] **Step 3: Verify in a browser**

Reload the page. Expected: four icon pins visible on the placeholder image — a ladybug, a tree, a beehive, and a flowerbed. Hovering each pin scales it up slightly with a stronger shadow. Tabbing through with the keyboard shows a green focus ring on each pin. **Clicking does nothing yet** — the JS is added in Task 4.

- [ ] **Step 4: Commit**

```bash
git add urban-greening/urban-greening-template.html
git commit -m "feat(urban-greening): add pin CSS and four sample pins/popups"
```

---

### Task 4: Add the popup open/close JavaScript

**Files:**
- Modify: `urban-greening/urban-greening-template.html`

Add the same vanilla JS used by `interactive-image` to open/close popups when pins are clicked.

- [ ] **Step 1: Replace the JS placeholder comment**

Find this line near the end of the file:

```html
<!-- JavaScript is added in a later task -->
```

Replace it with:

```html
<!-- ============================================================
     JAVASCRIPT (minimal — do not edit unless confident)

     Logic:
       • Clicking a .pin-btn adds "active" to its matching popup
       • Clicking a .close-btn or clicking outside a popup removes "active"
       • Only one popup is open at a time
     ============================================================ -->
<script>
  var buttons = document.querySelectorAll('.pin-btn');
  var popups  = document.querySelectorAll('.popup');

  function closeAll() {
    popups.forEach(function(p) { p.classList.remove('active'); });
  }

  buttons.forEach(function(btn) {
    btn.addEventListener('click', function(e) {
      e.stopPropagation();
      var target = document.getElementById(btn.dataset.popup);
      var isOpen = target.classList.contains('active');
      closeAll();
      if (!isOpen) target.classList.add('active');
    });
  });

  document.querySelectorAll('.close-btn').forEach(function(cb) {
    cb.addEventListener('click', function(e) {
      e.stopPropagation();
      closeAll();
    });
  });

  document.addEventListener('click', closeAll);
</script>
```

- [ ] **Step 2: Verify in a browser**

Reload the page. Click each of the four pins in turn — the matching popup should appear. Clicking a second pin closes the previous popup and opens the new one. Clicking the ✕ button or anywhere outside a popup closes it. Only one popup is ever open at a time.

- [ ] **Step 3: Commit**

```bash
git add urban-greening/urban-greening-template.html
git commit -m "feat(urban-greening): add popup open/close JS"
```

---

## Chunk 2: Worked example (`urban-greening/examples/greening-a-city-block.html`)

### Task 5: Create the example file with urban street scene background

**Files:**
- Create: `urban-greening/examples/greening-a-city-block.html`

Create the example file. The content is a copy of `urban-greening-template.html` with these changes:
- Title and header say "Greening a City Block"
- Instructions banner removed (this is a finished example, not a student starting point)
- Inline `data-fallback` contains an urban street scene SVG instead of the "Replace src" placeholder
- The four pin positions and popup contents are replaced with real content (this step)

- [ ] **Step 1: Create the file with the following contents**

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Greening a City Block</title>
  <style>
    :root {
      --accent:       #2e7d32;
      --accent-light: #66bb6a;
      --popup-bg:     #14241a;
      --popup-text:   #eaeaea;
      --btn-text:     #ffffff;
      --page-bg:      #0b140c;
      --icon-size:    60px;
      --font-title:   Georgia, serif;
      --font-body:    'Segoe UI', Arial, sans-serif;
    }

    *, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }

    body {
      font-family: var(--font-body);
      background: var(--page-bg);
      color: var(--popup-text);
      min-height: 100vh;
      display: flex;
      flex-direction: column;
      align-items: center;
    }

    header {
      width: 100%;
      padding: 1.2rem 2rem;
      text-align: center;
      letter-spacing: .12em;
      text-transform: uppercase;
      font-family: var(--font-title);
      font-size: 1.5rem;
      border-bottom: 2px solid var(--accent);
      background: var(--page-bg);
    }

    .image-wrap {
      position: relative;
      width: 900px;
      max-width: 98vw;
      margin: 2rem auto;
    }

    .bg-image {
      display: block;
      width: 100%;
      height: auto;
      border: 3px solid var(--accent);
      border-radius: 4px;
    }

    .pin-btn {
      position: absolute;
      transform: translate(-50%, -50%);
      background: none;
      border: none;
      padding: 0;
      cursor: pointer;
      line-height: 0;
      transition: transform .15s ease, filter .15s ease;
      filter: drop-shadow(0 2px 4px rgba(0,0,0,.6));
    }
    .pin-icon {
      width: var(--icon-size);
      height: var(--icon-size);
      display: block;
      pointer-events: none;
    }
    .pin-btn:hover {
      transform: translate(-50%, -50%) scale(1.15);
      filter: drop-shadow(0 4px 8px rgba(0,0,0,.75));
    }
    .pin-btn:focus-visible {
      outline: 3px solid var(--accent-light);
      outline-offset: 4px;
      border-radius: 4px;
    }

    .popup {
      display: none;
      position: absolute;
      z-index: 10;
      width: 280px;
      background: var(--popup-bg);
      border: 2px solid var(--accent);
      border-radius: 6px;
      box-shadow: 0 8px 30px rgba(0,0,0,.7);
      overflow: hidden;
      animation: popIn .2s ease;
    }
    .popup.active { display: block; }

    @keyframes popIn {
      from { opacity: 0; transform: scale(.92); }
      to   { opacity: 1; transform: scale(1);   }
    }

    .popup-header {
      background: var(--accent);
      padding: .6rem 1rem;
      display: flex;
      justify-content: space-between;
      align-items: center;
    }
    .popup-header h2 {
      font-family: var(--font-title);
      font-size: 1rem;
      letter-spacing: .05em;
    }
    .close-btn {
      background: none;
      border: none;
      color: #fff;
      font-size: 1.2rem;
      line-height: 1;
      cursor: pointer;
      padding: 0 .2rem;
    }
    .popup-body {
      padding: .8rem 1rem 1rem;
      font-size: .85rem;
      line-height: 1.6;
    }

    footer {
      margin-top: auto;
      padding: 1rem;
      font-size: .75rem;
      opacity: .4;
      text-align: center;
    }
  </style>
</head>
<body>

<header>Greening a City Block</header>

<!-- ICON LIBRARY (identical to the template) -->
<svg xmlns="http://www.w3.org/2000/svg" style="position:absolute; width:0; height:0; overflow:hidden;" aria-hidden="true">
  <defs>

    <!-- LADYBUG -->
    <symbol id="icon-bug" viewBox="0 0 64 64">
      <path d="M 18 42 L 8 48 M 46 42 L 56 48 M 20 32 L 6 32 M 44 32 L 58 32 M 20 22 L 10 14 M 44 22 L 54 14"
            stroke="#111" stroke-width="2" stroke-linecap="round" fill="none"/>
      <ellipse cx="32" cy="34" rx="20" ry="17" fill="#c62828"/>
      <ellipse cx="24" cy="26" rx="6" ry="3" fill="#ef5350" opacity="0.6"/>
      <path d="M 32 18 L 32 50" stroke="#111" stroke-width="1.5"/>
      <circle cx="22" cy="28" r="2.6" fill="#111"/>
      <circle cx="42" cy="28" r="2.6" fill="#111"/>
      <circle cx="20" cy="40" r="2.2" fill="#111"/>
      <circle cx="44" cy="40" r="2.2" fill="#111"/>
      <circle cx="32" cy="44" r="2.2" fill="#111"/>
      <ellipse cx="32" cy="18" rx="9" ry="7" fill="#111"/>
      <circle cx="28" cy="17" r="1.3" fill="#fff"/>
      <circle cx="36" cy="17" r="1.3" fill="#fff"/>
      <path d="M 28 12 Q 24 6 22 5 M 36 12 Q 40 6 42 5" stroke="#111" stroke-width="1.5" fill="none" stroke-linecap="round"/>
      <circle cx="22" cy="5" r="1.5" fill="#111"/>
      <circle cx="42" cy="5" r="1.5" fill="#111"/>
    </symbol>

    <!-- TREE -->
    <symbol id="icon-tree" viewBox="0 0 64 64">
      <ellipse cx="32" cy="60" rx="18" ry="2.5" fill="#000" opacity="0.25"/>
      <rect x="28" y="40" width="8" height="20" fill="#6d4c2e"/>
      <rect x="34" y="40" width="2" height="20" fill="#4a3219"/>
      <path d="M 30 44 Q 31 48 30 52 M 33 42 Q 34 50 33 58" stroke="#4a3219" stroke-width="0.8" fill="none"/>
      <ellipse cx="32" cy="30" rx="24" ry="20" fill="#1b5e20"/>
      <ellipse cx="24" cy="22" rx="13" ry="12" fill="#2e7d32"/>
      <ellipse cx="40" cy="24" rx="12" ry="11" fill="#2e7d32"/>
      <ellipse cx="32" cy="16" rx="11" ry="10" fill="#43a047"/>
      <circle cx="22" cy="16" r="2" fill="#81c784" opacity="0.8"/>
      <circle cx="38" cy="12" r="2" fill="#81c784" opacity="0.8"/>
      <circle cx="44" cy="22" r="2" fill="#81c784" opacity="0.8"/>
      <circle cx="18" cy="28" r="2" fill="#81c784" opacity="0.8"/>
      <circle cx="30" cy="10" r="1.5" fill="#c8e6c9" opacity="0.9"/>
    </symbol>

    <!-- BEEHIVE -->
    <symbol id="icon-beehive" viewBox="0 0 64 64">
      <ellipse cx="32" cy="58" rx="22" ry="2.5" fill="#000" opacity="0.25"/>
      <rect x="8" y="52" width="48" height="5" fill="#5d4037" rx="1"/>
      <ellipse cx="32" cy="50" rx="22" ry="5" fill="#8d6e3a"/>
      <ellipse cx="32" cy="44" rx="21" ry="5" fill="#a9843d"/>
      <ellipse cx="32" cy="38" rx="19" ry="5" fill="#8d6e3a"/>
      <ellipse cx="32" cy="32" rx="16" ry="5" fill="#a9843d"/>
      <ellipse cx="32" cy="26" rx="13" ry="5" fill="#8d6e3a"/>
      <ellipse cx="32" cy="20" rx="9" ry="4" fill="#a9843d"/>
      <ellipse cx="32" cy="15" rx="5" ry="3" fill="#8d6e3a"/>
      <circle cx="32" cy="12" r="2" fill="#5d4037"/>
      <ellipse cx="32" cy="50" rx="3" ry="3.5" fill="#2a1a0a"/>
      <g transform="translate(50 26)">
        <ellipse cx="0" cy="0" rx="3" ry="2" fill="#f9a825"/>
        <path d="M -1.5 -1.5 L -1.5 1.5 M 1 -1.5 L 1 1.5" stroke="#111" stroke-width="0.9"/>
        <ellipse cx="1" cy="-2" rx="2" ry="1.2" fill="#eceff1" opacity="0.85"/>
      </g>
      <g transform="translate(12 34)">
        <ellipse cx="0" cy="0" rx="2.5" ry="1.7" fill="#f9a825"/>
        <path d="M -1.2 -1.3 L -1.2 1.3 M 0.8 -1.3 L 0.8 1.3" stroke="#111" stroke-width="0.8"/>
        <ellipse cx="-0.5" cy="-1.6" rx="1.6" ry="1" fill="#eceff1" opacity="0.85"/>
      </g>
    </symbol>

    <!-- GARDEN -->
    <symbol id="icon-garden" viewBox="0 0 64 64">
      <ellipse cx="32" cy="60" rx="26" ry="2.5" fill="#000" opacity="0.25"/>
      <rect x="6" y="44" width="52" height="14" fill="#6d4c2e" rx="1"/>
      <rect x="4" y="42" width="56" height="4" fill="#8d6a44" rx="1"/>
      <line x1="16" y1="46" x2="16" y2="57" stroke="#4a3219" stroke-width="0.6"/>
      <line x1="28" y1="46" x2="28" y2="57" stroke="#4a3219" stroke-width="0.6"/>
      <line x1="40" y1="46" x2="40" y2="57" stroke="#4a3219" stroke-width="0.6"/>
      <line x1="52" y1="46" x2="52" y2="57" stroke="#4a3219" stroke-width="0.6"/>
      <line x1="14" y1="42" x2="14" y2="28" stroke="#2e7d32" stroke-width="1.8"/>
      <line x1="24" y1="42" x2="24" y2="20" stroke="#2e7d32" stroke-width="1.8"/>
      <line x1="34" y1="42" x2="34" y2="24" stroke="#2e7d32" stroke-width="1.8"/>
      <line x1="44" y1="42" x2="44" y2="18" stroke="#2e7d32" stroke-width="1.8"/>
      <line x1="52" y1="42" x2="52" y2="26" stroke="#2e7d32" stroke-width="1.8"/>
      <ellipse cx="17" cy="34" rx="3.5" ry="1.8" fill="#43a047" transform="rotate(30 17 34)"/>
      <ellipse cx="27" cy="30" rx="3.5" ry="1.8" fill="#43a047" transform="rotate(-30 27 30)"/>
      <ellipse cx="38" cy="30" rx="3.5" ry="1.8" fill="#43a047" transform="rotate(30 38 30)"/>
      <ellipse cx="47" cy="28" rx="3.5" ry="1.8" fill="#43a047" transform="rotate(-30 47 28)"/>
      <ellipse cx="49" cy="32" rx="3.5" ry="1.8" fill="#43a047" transform="rotate(30 49 32)"/>
      <circle cx="14" cy="26" r="4" fill="#ec407a"/>
      <circle cx="14" cy="26" r="1.6" fill="#fff59d"/>
      <circle cx="24" cy="18" r="4" fill="#fdd835"/>
      <circle cx="24" cy="18" r="1.6" fill="#e65100"/>
      <circle cx="34" cy="22" r="4" fill="#8e24aa"/>
      <circle cx="34" cy="22" r="1.6" fill="#fff59d"/>
      <circle cx="44" cy="16" r="4" fill="#fb8c00"/>
      <circle cx="44" cy="16" r="1.6" fill="#fff59d"/>
      <circle cx="52" cy="24" r="4" fill="#fafafa"/>
      <circle cx="52" cy="24" r="1.6" fill="#fdd835"/>
    </symbol>

  </defs>
</svg>

<div class="image-wrap">

  <!-- Background: inline SVG of a simplified urban street scene.
       Sky, three buildings of varying heights (one with a flat rooftop),
       a vacant lot between two of them, a pavement, and a road. -->
  <img class="bg-image"
       src="placeholder.svg"
       alt="Simplified urban street scene"
       onerror="this.src=this.getAttribute('data-fallback')"
       data-fallback="data:image/svg+xml;utf8,%3Csvg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 900 500'%3E%3C!-- sky --%3E%3Crect width='900' height='340' fill='%23a7d5ea'/%3E%3C!-- distant clouds --%3E%3Cellipse cx='180' cy='90' rx='60' ry='14' fill='%23ffffff' opacity='0.8'/%3E%3Cellipse cx='680' cy='70' rx='70' ry='15' fill='%23ffffff' opacity='0.8'/%3E%3C!-- left building (tall, red brick) --%3E%3Crect x='50' y='140' width='180' height='200' fill='%238b4a3a'/%3E%3Crect x='50' y='140' width='180' height='12' fill='%236b3828'/%3E%3Cg fill='%23c5d8e6'%3E%3Crect x='70' y='170' width='24' height='30'/%3E%3Crect x='110' y='170' width='24' height='30'/%3E%3Crect x='150' y='170' width='24' height='30'/%3E%3Crect x='190' y='170' width='24' height='30'/%3E%3Crect x='70' y='220' width='24' height='30'/%3E%3Crect x='110' y='220' width='24' height='30'/%3E%3Crect x='150' y='220' width='24' height='30'/%3E%3Crect x='190' y='220' width='24' height='30'/%3E%3Crect x='70' y='270' width='24' height='30'/%3E%3Crect x='110' y='270' width='24' height='30'/%3E%3Crect x='150' y='270' width='24' height='30'/%3E%3Crect x='190' y='270' width='24' height='30'/%3E%3C/g%3E%3C!-- vacant lot (between left and middle buildings) --%3E%3Crect x='230' y='260' width='140' height='80' fill='%23b8a888'/%3E%3Cpath d='M 240 320 L 250 310 L 260 320 M 280 325 L 290 315 L 300 325 M 330 322 L 340 312 L 350 322' stroke='%238a7a5a' stroke-width='2' fill='none'/%3E%3C!-- middle building (short, with flat rooftop for beehive) --%3E%3Crect x='370' y='200' width='200' height='140' fill='%23d4c8a8'/%3E%3Crect x='370' y='200' width='200' height='10' fill='%23a89b7a'/%3E%3Cg fill='%235b7a94'%3E%3Crect x='390' y='230' width='30' height='36'/%3E%3Crect x='440' y='230' width='30' height='36'/%3E%3Crect x='490' y='230' width='30' height='36'/%3E%3Crect x='540' y='230' width='18' height='36'/%3E%3Crect x='390' y='285' width='30' height='36'/%3E%3Crect x='440' y='285' width='30' height='36'/%3E%3Crect x='490' y='285' width='30' height='36'/%3E%3Crect x='540' y='285' width='18' height='36'/%3E%3C/g%3E%3C!-- right building (mid, grey) --%3E%3Crect x='570' y='160' width='230' height='180' fill='%236f7a85'/%3E%3Crect x='570' y='160' width='230' height='10' fill='%234f5a65'/%3E%3Cg fill='%23cfe0ef'%3E%3Crect x='590' y='185' width='26' height='34'/%3E%3Crect x='635' y='185' width='26' height='34'/%3E%3Crect x='680' y='185' width='26' height='34'/%3E%3Crect x='725' y='185' width='26' height='34'/%3E%3Crect x='770' y='185' width='20' height='34'/%3E%3Crect x='590' y='235' width='26' height='34'/%3E%3Crect x='635' y='235' width='26' height='34'/%3E%3Crect x='680' y='235' width='26' height='34'/%3E%3Crect x='725' y='235' width='26' height='34'/%3E%3Crect x='770' y='235' width='20' height='34'/%3E%3Crect x='590' y='285' width='26' height='34'/%3E%3Crect x='635' y='285' width='26' height='34'/%3E%3Crect x='680' y='285' width='26' height='34'/%3E%3Crect x='725' y='285' width='26' height='34'/%3E%3Crect x='770' y='285' width='20' height='34'/%3E%3C/g%3E%3C!-- pavement --%3E%3Crect y='340' width='900' height='40' fill='%23b7b7b7'/%3E%3Cline x1='0' y1='340' x2='900' y2='340' stroke='%23707070' stroke-width='2'/%3E%3C!-- pavement joins --%3E%3Cg stroke='%23909090' stroke-width='1'%3E%3Cline x1='150' y1='340' x2='150' y2='380'/%3E%3Cline x1='300' y1='340' x2='300' y2='380'/%3E%3Cline x1='450' y1='340' x2='450' y2='380'/%3E%3Cline x1='600' y1='340' x2='600' y2='380'/%3E%3Cline x1='750' y1='340' x2='750' y2='380'/%3E%3C/g%3E%3C!-- road --%3E%3Crect y='380' width='900' height='120' fill='%233c3c3c'/%3E%3C!-- lane markings --%3E%3Cg stroke='%23f0c040' stroke-width='4' stroke-dasharray='30 20'%3E%3Cline x1='0' y1='440' x2='900' y2='440'/%3E%3C/g%3E%3C/svg%3E">

  <!-- Pins and popups are added in Task 6 -->

</div>

<footer>Urban Greening Example | Greening a City Block</footer>

<script>
  var buttons = document.querySelectorAll('.pin-btn');
  var popups  = document.querySelectorAll('.popup');

  function closeAll() {
    popups.forEach(function(p) { p.classList.remove('active'); });
  }

  buttons.forEach(function(btn) {
    btn.addEventListener('click', function(e) {
      e.stopPropagation();
      var target = document.getElementById(btn.dataset.popup);
      var isOpen = target.classList.contains('active');
      closeAll();
      if (!isOpen) target.classList.add('active');
    });
  });

  document.querySelectorAll('.close-btn').forEach(function(cb) {
    cb.addEventListener('click', function(e) {
      e.stopPropagation();
      closeAll();
    });
  });

  document.addEventListener('click', closeAll);
</script>

</body>
</html>
```

- [ ] **Step 2: Verify in a browser**

Open `urban-greening/examples/greening-a-city-block.html`. Expected: a green-themed page titled "Greening a City Block." The image area shows a simple urban street scene with a sky, three buildings (one with a flat rooftop, one with a vacant brown lot beside it), a grey pavement, and a dark grey road with dashed yellow lane markings. No pins yet.

- [ ] **Step 3: Commit**

```bash
git add urban-greening/examples/greening-a-city-block.html
git commit -m "feat(urban-greening): add city-block example with urban street background"
```

---

### Task 6: Add the four themed pins and popups to the example

**Files:**
- Modify: `urban-greening/examples/greening-a-city-block.html`

Replace the `<!-- Pins and popups are added in Task 6 -->` placeholder with four pins — one per icon — positioned on the urban scene, each with realistic popup content.

- [ ] **Step 1: Replace the placeholder comment**

Find this line:

```html
  <!-- Pins and popups are added in Task 6 -->
```

Replace it with:

```html
  <!-- PINS -->

  <!-- Community garden on the vacant lot (icon-garden) -->
  <button class="pin-btn"
          data-popup="popup-garden"
          aria-label="Community garden"
          style="top: 60%; left: 33%;">
    <svg class="pin-icon" viewBox="0 0 64 64" aria-hidden="true"><use href="#icon-garden"/></svg>
  </button>

  <!-- Street tree on the pavement (icon-tree) -->
  <button class="pin-btn"
          data-popup="popup-tree"
          aria-label="Street tree"
          style="top: 68%; left: 58%;">
    <svg class="pin-icon" viewBox="0 0 64 64" aria-hidden="true"><use href="#icon-tree"/></svg>
  </button>

  <!-- Rooftop beehive on the middle building (icon-beehive) -->
  <button class="pin-btn"
          data-popup="popup-beehive"
          aria-label="Rooftop beehive"
          style="top: 37%; left: 52%;">
    <svg class="pin-icon" viewBox="0 0 64 64" aria-hidden="true"><use href="#icon-beehive"/></svg>
  </button>

  <!-- Insect hotel on the side of the right building (icon-bug) -->
  <button class="pin-btn"
          data-popup="popup-bug"
          aria-label="Insect hotel"
          style="top: 58%; left: 82%;">
    <svg class="pin-icon" viewBox="0 0 64 64" aria-hidden="true"><use href="#icon-bug"/></svg>
  </button>

  <!-- POPUPS -->

  <div class="popup" id="popup-garden" style="top: 8%; left: 4%;">
    <div class="popup-header">
      <h2>Community Garden</h2>
      <button class="close-btn" aria-label="Close">&#10005;</button>
    </div>
    <div class="popup-body">
      <p>The empty lot between the buildings has been turned into a
         <strong>community garden</strong>. Neighbours share plots to grow
         vegetables, herbs and flowers.</p>
      <p>Community gardens turn unused land into productive green space,
         reduce food miles, and give people a place to meet.</p>
    </div>
  </div>

  <div class="popup" id="popup-tree" style="top: 8%; left: 38%;">
    <div class="popup-header">
      <h2>Street Tree</h2>
      <button class="close-btn" aria-label="Close">&#10005;</button>
    </div>
    <div class="popup-body">
      <p>A <strong>broadleaf street tree</strong> shades the pavement and
         cools the surrounding buildings in summer.</p>
      <p>Street trees also filter air pollution, slow stormwater runoff,
         and provide habitat for birds and insects in the middle of the
         city.</p>
    </div>
  </div>

  <div class="popup" id="popup-beehive" style="top: 8%; left: 60%;">
    <div class="popup-header">
      <h2>Rooftop Beehive</h2>
      <button class="close-btn" aria-label="Close">&#10005;</button>
    </div>
    <div class="popup-body">
      <p>A <strong>rooftop apiary</strong> on the flat roof lets the building
         host bees without taking up any street space.</p>
      <p>Urban bees pollinate nearby gardens, parks and balcony plants, and
         the hive can produce local honey for the people who care for it.</p>
    </div>
  </div>

  <div class="popup" id="popup-bug" style="top: 12%; right: 4%;">
    <div class="popup-header">
      <h2>Insect Hotel</h2>
      <button class="close-btn" aria-label="Close">&#10005;</button>
    </div>
    <div class="popup-body">
      <p>Attached to the side of the building is an <strong>insect hotel</strong>
         — a stack of wood, bamboo and bricks with hollows inside.</p>
      <p>It provides shelter for solitary bees, ladybugs, and lacewings.
         These beneficial insects pollinate plants and keep aphid
         populations down in the community garden.</p>
    </div>
  </div>
```

- [ ] **Step 2: Verify in a browser**

Reload the example. Expected: four icon pins placed on the urban scene:
- A flowerbed pin on the vacant brown lot
- A tree pin on the pavement
- A beehive pin on the flat rooftop of the middle building
- A ladybug pin on the side of the right-hand building

Click each pin and confirm its popup opens with the correct content. Confirm that opening a second pin closes the first. Confirm that clicking outside or the ✕ button closes any open popup.

- [ ] **Step 3: Commit**

```bash
git add urban-greening/examples/greening-a-city-block.html
git commit -m "feat(urban-greening): add four themed pins to city-block example"
```

---

## Chunk 3: Integration

### Task 7: Add urban-greening to CLAUDE.md

**Files:**
- Modify: `CLAUDE.md`

Add an entry for the new template to the "Current Templates" list so future contributors see it.

- [ ] **Step 1: Update the Current Templates list**

Find this block in `CLAUDE.md`:

```markdown
- **true-or-false-checker**: Self-marking true/false statement activity
- **vocabulary-builder**: Definition-matching vocabulary learning activity
```

Replace it with:

```markdown
- **true-or-false-checker**: Self-marking true/false statement activity
- **urban-greening**: Interactive-image variant with realistic SVG icon pins (bug, tree, beehive, garden) themed around adding nature to urban spaces
- **vocabulary-builder**: Definition-matching vocabulary learning activity
```

(The list is alphabetical — `urban-greening` slots in between `true-or-false-checker` and `vocabulary-builder`.)

- [ ] **Step 2: Verify**

Open `CLAUDE.md` and confirm the new entry appears in alphabetical order with a one-line description consistent with the other entries.

- [ ] **Step 3: Commit**

```bash
git add CLAUDE.md
git commit -m "docs: list urban-greening in Current Templates"
```

---

## Done

After Task 7, the new template is complete:
- `urban-greening/urban-greening-template.html` — a blank template with the icon library, four sample pins, and instructions.
- `urban-greening/examples/greening-a-city-block.html` — a finished worked example.
- `CLAUDE.md` — lists the new template.

Open both HTML files in a browser as a final sanity check. Confirm all four icons render, all pins open their popups, and the example displays a recognizable urban street scene.
