# Sorting Activity — Check Answers Implementation Plan

> **For agentic workers:** REQUIRED: Use superpowers:subagent-driven-development (if subagents available) or superpowers:executing-plans to implement this plan. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Add a "Check Answers" button to the sorting activity template and example that marks each card correct/wrong and shows a score once all cards are placed.

**Architecture:** Pure HTML/CSS/JS edits to two self-contained single-file templates and one Markdown brief. No build tools. Changes are additive — new CSS rules, new JS functions, one new button, and `data-answer` attributes on cards.

**Tech Stack:** Vanilla HTML, CSS, JavaScript (var/querySelectorAll/addEventListener). No frameworks, no build tools.

**Spec:** `docs/superpowers/specs/2026-03-22-sorting-activity-check-answers-design.md`

---

## Chunk 1: Template file (`sorting-activity-template.html`)

### Task 1: Update `.controls` CSS and add Check Answers button HTML

**Files:**
- Modify: `sorting-activity/sorting-activity-template.html`

The `.controls` div currently uses `text-align: center`. Replace with flex layout and add the Check Answers button.

- [ ] **Step 1: Replace `.controls` CSS**

Find this rule in the `<style>` block:

```css
    .controls {
      text-align: center;
      margin-bottom: 20px;
    }
```

Replace with:

```css
    .controls {
      display: flex;
      justify-content: center;
      gap: 12px;
      margin-bottom: 20px;
    }
```

- [ ] **Step 2: Add Check Answers button to HTML**

Find this in the HTML body:

```html
  <div class="controls">
    <button class="btn-reset" id="resetBtn">Reset All</button>
  </div>
```

Replace with:

```html
  <div class="controls">
    <button class="btn-reset" id="resetBtn">Reset All</button>
    <button class="btn-check" id="checkBtn" disabled>Check Answers</button>
  </div>
```

- [ ] **Step 3: Commit**

```bash
git add sorting-activity/sorting-activity-template.html
git commit -m "feat(template): add Check Answers button HTML and flex controls layout"
```

---

### Task 2: Add CSS for card feedback states and Check Answers button

**Files:**
- Modify: `sorting-activity/sorting-activity-template.html`

Add new CSS rules after the existing sorted-card border rules.

- [ ] **Step 1: Add btn-check CSS**

Find this comment and rule block (near end of the CSS, before the `@media` block):

```css
    /* ---------- Responsive ---------- */
```

Insert the following immediately before that comment:

```css
    /* ---------- Check Answers button ---------- */
    .btn-check {
      background: var(--accent-b);
      border: 2px solid var(--accent-b);
      color: #fff;
      padding: 8px 22px;
      border-radius: var(--btn-radius);
      cursor: pointer;
      font-size: 0.9rem;
      font-family: var(--font-family);
      transition: opacity 0.2s;
    }
    .btn-check:disabled {
      opacity: 0.35;
      cursor: not-allowed;
    }
    .btn-check:not(:disabled):hover {
      opacity: 0.85;
    }

    /* ---------- Card feedback states ---------- */
    /* Selectors use 3-class specificity (.column-x .card.card-state) to beat
       the existing 2-class sorted-card border rules (.column-x .card).       */
    .column-a .card.card-correct,
    .column-b .card.card-correct {
      border-color: #4caf50;
      border-left: 4px solid #4caf50;
    }
    .column-a .card.card-wrong,
    .column-b .card.card-wrong {
      border-color: #e94560;
      border-left: 4px solid #e94560;
    }
    .card-result-icon {
      font-weight: bold;
      margin-left: 6px;
    }
    .column-a .card.card-correct .card-result-icon,
    .column-b .card.card-correct .card-result-icon { color: #4caf50; }
    .column-a .card.card-wrong .card-result-icon,
    .column-b .card.card-wrong .card-result-icon { color: #e94560; }

```

- [ ] **Step 2: Commit**

```bash
git add sorting-activity/sorting-activity-template.html
git commit -m "feat(template): add CSS for check answers button and card feedback states"
```

---

### Task 3: Add module-level variables and update `init()`

**Files:**
- Modify: `sorting-activity/sorting-activity-template.html`

- [ ] **Step 1: Add module-level variable declarations**

Find this block near the top of the `<script>`:

```js
    var nameA = document.getElementById('colA').getAttribute('data-name');
    var nameB = document.getElementById('colB').getAttribute('data-name');

    var pool  = document.getElementById('pool');
    var bodyA = document.getElementById('bodyA');
    var bodyB = document.getElementById('bodyB');
    var countAEl = document.getElementById('countA');
    var countBEl = document.getElementById('countB');
```

Replace with:

```js
    var nameA = document.getElementById('colA').getAttribute('data-name');
    var nameB = document.getElementById('colB').getAttribute('data-name');

    var pool  = document.getElementById('pool');
    var bodyA = document.getElementById('bodyA');
    var bodyB = document.getElementById('bodyB');
    var countAEl = document.getElementById('countA');
    var countBEl = document.getElementById('countB');

    var scoreBar;
    var scoreBarOriginal = '';
    var checkBtn;
```

- [ ] **Step 2: Update `init()` to wire up Check Answers**

Find the `init()` function:

```js
    function init() {
      var cards = pool.querySelectorAll('.card');
      cards.forEach(function(card) {
        buildPoolButtons(card);
      });
      updateCounts();
    }
```

Replace with:

```js
    function init() {
      var cards = pool.querySelectorAll('.card');
      cards.forEach(function(card) {
        buildPoolButtons(card);
      });
      updateCounts();
      scoreBar = document.querySelector('.score-bar');
      scoreBarOriginal = scoreBar.innerHTML;
      checkBtn = document.getElementById('checkBtn');
      checkBtn.addEventListener('click', checkAnswers);
    }
```

- [ ] **Step 3: Commit**

```bash
git add sorting-activity/sorting-activity-template.html
git commit -m "feat(template): add scoreBar/checkBtn module vars and wire up init()"
```

---

### Task 4: Add `checkAnswers()` and `updateCheckButton()` functions

**Files:**
- Modify: `sorting-activity/sorting-activity-template.html`

- [ ] **Step 1: Add `updateCheckButton()` after `updateCounts()`**

Find the `updateCounts()` function:

```js
    // Update the item count display
    function updateCounts() {
      countAEl.textContent = bodyA.querySelectorAll('.card').length;
      countBEl.textContent = bodyB.querySelectorAll('.card').length;
    }
```

Insert the following immediately after it:

```js
    // Enable Check Answers only when every card has been sorted (pool is empty)
    function updateCheckButton() {
      checkBtn.disabled = pool.querySelectorAll('.card').length !== 0;
    }

    // Mark each sorted card correct or wrong, show score, lock cards
    function checkAnswers() {
      var cards = document.querySelectorAll('#bodyA .card, #bodyB .card');
      var correctCount = 0;
      var total = cards.length;

      cards.forEach(function(card) {
        var answer = card.getAttribute('data-answer') || '';
        var inA = card.parentNode === bodyA;
        var isCorrect = (inA && answer === 'A') || (!inA && answer === 'B');

        if (isCorrect) { correctCount++; }
        card.classList.add(isCorrect ? 'card-correct' : 'card-wrong');

        var icon = document.createElement('span');
        icon.className = 'card-result-icon';
        icon.textContent = isCorrect ? '\u2713' : '\u2717';
        card.querySelector('.card-text').appendChild(icon);

        var actions = card.querySelector('.card-actions');
        if (actions) { actions.parentNode.removeChild(actions); }
      });

      scoreBar.innerHTML = '<span>' + correctCount + ' / ' + total + ' correct</span>';
      checkBtn.disabled = true;
    }
```

- [ ] **Step 2: Verify function placement**

The script should now read (in order): `buildPoolButtons`, `buildBackButton`, `moveToColumn`, `returnToPool`, `updateCounts`, `updateCheckButton`, `checkAnswers`, `resetAll`, `init`.

- [ ] **Step 3: Commit**

```bash
git add sorting-activity/sorting-activity-template.html
git commit -m "feat(template): add checkAnswers() and updateCheckButton() functions"
```

---

### Task 5: Update `moveToColumn()`, `returnToPool()`, and `resetAll()`

**Files:**
- Modify: `sorting-activity/sorting-activity-template.html`

- [ ] **Step 1: Update `moveToColumn()` to call `updateCheckButton()`**

Find `moveToColumn()`:

```js
    // Move a card from the pool into a category column
    function moveToColumn(card, col) {
      if (col === 'A') {
        bodyA.appendChild(card);
      } else {
        bodyB.appendChild(card);
      }
      buildBackButton(card);
      updateCounts();
    }
```

Replace with:

```js
    // Move a card from the pool into a category column
    function moveToColumn(card, col) {
      if (col === 'A') {
        bodyA.appendChild(card);
      } else {
        bodyB.appendChild(card);
      }
      buildBackButton(card);
      updateCounts();
      updateCheckButton();
    }
```

- [ ] **Step 2: Update `returnToPool()` to call `updateCheckButton()`**

Find `returnToPool()`:

```js
    // Return a sorted card back to the pool
    function returnToPool(card) {
      pool.appendChild(card);
      buildPoolButtons(card);
      updateCounts();
    }
```

Replace with:

```js
    // Return a sorted card back to the pool
    function returnToPool(card) {
      pool.appendChild(card);
      buildPoolButtons(card);
      updateCounts();
      updateCheckButton();
    }
```

- [ ] **Step 3: Update `resetAll()` to clear feedback and restore score bar**

Find `resetAll()`:

```js
    // Reset all cards back to the pool
    function resetAll() {
      // Collect cards from both columns and return them
      var sortedCards = document.querySelectorAll('#bodyA .card, #bodyB .card');
      sortedCards.forEach(function(card) {
        pool.appendChild(card);
        buildPoolButtons(card);
      });
      updateCounts();
    }
```

Replace with:

```js
    // Reset all cards back to the pool
    function resetAll() {
      // Collect cards from both columns and return them
      var sortedCards = document.querySelectorAll('#bodyA .card, #bodyB .card');
      sortedCards.forEach(function(card) {
        pool.appendChild(card);
        buildPoolButtons(card);
      });

      // Remove any feedback classes and tick/cross icons from all cards
      document.querySelectorAll('.card').forEach(function(card) {
        card.classList.remove('card-correct', 'card-wrong');
        var icon = card.querySelector('.card-result-icon');
        if (icon) { icon.parentNode.removeChild(icon); }
      });

      // Restore score bar (must happen before updateCounts so #countA/#countB exist)
      scoreBar.innerHTML = scoreBarOriginal;
      // Re-query because innerHTML replacement made the old references stale.
      // Do NOT add var — these must overwrite the module-level variables.
      countAEl = document.getElementById('countA');
      countBEl = document.getElementById('countB');
      updateCounts();
      checkBtn.disabled = true;
    }
```

- [ ] **Step 4: Commit**

```bash
git add sorting-activity/sorting-activity-template.html
git commit -m "feat(template): update moveToColumn, returnToPool, resetAll for check-answers"
```

---

### Task 6: Update setup guide comment in template

**Files:**
- Modify: `sorting-activity/sorting-activity-template.html`

The setup guide must document the new `data-answer` attribute.

- [ ] **Step 1: Update setup guide**

Find the setup guide `<ol>` in the instructions banner. It currently has steps 1–6. Find step 3:

```html
      <li>Find the <code>&lt;!-- ITEM POOL --&gt;</code> section below and edit the 8 <code>data-label</code> values on each <code>&lt;div class="card" ...&gt;</code> to your own items.</li>
```

Replace with:

```html
      <li>Find the <code>&lt;!-- ITEM POOL --&gt;</code> section below and edit each <code>&lt;div class="card"&gt;</code>: set <code>data-label</code> to your item text and <code>data-answer</code> to <code>"A"</code> or <code>"B"</code> (uppercase) for the correct column. If <code>data-answer</code> is omitted, Check Answers will mark that card as wrong.</li>
```

Also find the comment above the card blocks in the ITEM POOL section:

```html
      <!-- STUDENT: Replace "Item 1" with your first item text, and so on for each card below -->
```

Replace with:

```html
      <!-- STUDENT: For each card below:
           - Replace the data-label value and the <span class="card-text"> text with your item text
           - Set data-answer="A" or data-answer="B" (uppercase) to the correct category column
             If data-answer is omitted, Check Answers will mark that card as wrong -->
```

- [ ] **Step 2: Commit**

```bash
git add sorting-activity/sorting-activity-template.html
git commit -m "docs(template): update setup guide to document data-answer attribute"
```

---

### Task 7: Manual verification — template

**No file changes.** Open the template in a browser and verify:

- [ ] **Step 1: Open file in browser**

Open `sorting-activity/sorting-activity-template.html` in a browser.

- [ ] **Step 2: Verify initial state**

- Two buttons visible: "Reset All" (ghost style) and "Check Answers" (teal, dimmed)
- Check Answers button is disabled (cannot click)
- Sort a few cards — Check Answers remains disabled while any cards remain in the pool

- [ ] **Step 3: Verify check triggers only when pool is empty**

Sort all 8 cards into columns A or B. Confirm Check Answers button becomes enabled (full opacity, clickable).

- [ ] **Step 4: Verify feedback**

Click Check Answers. Confirm:
- Cards with `data-answer` absent are marked wrong (red border, ✗ icon) — all 8 template cards have no `data-answer`, so all should be wrong
- Score shows `0 / 8 correct`
- Back buttons are gone from all cards (cards are locked)
- Check Answers button is now disabled again

- [ ] **Step 5: Verify reset**

Click Reset All. Confirm:
- All cards return to the pool
- Red borders and ✗ icons are gone
- Score bar reverts to `0 in Category A / 0 in Category B`
- Check Answers button is disabled again
- Sort buttons appear on all cards again

---

## Chunk 2: Example file (`causes-and-effects.html`) and brief

### Task 8: Add `data-answer` to example file

**Files:**
- Modify: `sorting-activity/examples/causes-and-effects.html`

Correct answers (Cause = `"A"`, Effect = `"B"`):

| Card | `data-answer` |
|---|---|
| More job opportunities | `"A"` |
| Traffic jams | `"B"` |
| Better hospitals and schools | `"A"` |
| Air pollution from cars | `"B"` |
| People want good schools | `"A"` |
| Farmland is cleared for buildings | `"B"` |
| Factories need workers | `"A"` |
| Housing shortages | `"B"` |

- [ ] **Step 1: Add data-answer to all 8 cards**

Make the following exact edits to the card divs in the pool:

```html
<!-- Before -->
      <div class="card" data-label="More job opportunities">

<!-- After -->
      <div class="card" data-label="More job opportunities" data-answer="A">
```

```html
<!-- Before -->
      <div class="card" data-label="Traffic jams">

<!-- After -->
      <div class="card" data-label="Traffic jams" data-answer="B">
```

```html
<!-- Before -->
      <div class="card" data-label="Better hospitals and schools">

<!-- After -->
      <div class="card" data-label="Better hospitals and schools" data-answer="A">
```

```html
<!-- Before -->
      <div class="card" data-label="Air pollution from cars">

<!-- After -->
      <div class="card" data-label="Air pollution from cars" data-answer="B">
```

```html
<!-- Before -->
      <div class="card" data-label="People want good schools">

<!-- After -->
      <div class="card" data-label="People want good schools" data-answer="A">
```

```html
<!-- Before -->
      <div class="card" data-label="Farmland is cleared for buildings">

<!-- After -->
      <div class="card" data-label="Farmland is cleared for buildings" data-answer="B">
```

```html
<!-- Before -->
      <div class="card" data-label="Factories need workers">

<!-- After -->
      <div class="card" data-label="Factories need workers" data-answer="A">
```

```html
<!-- Before -->
      <div class="card" data-label="Housing shortages">

<!-- After -->
      <div class="card" data-label="Housing shortages" data-answer="B">
```

- [ ] **Step 2: Replace `.controls` CSS (flex layout)**

Find in `<style>`:

```css
    .controls {
      text-align: center;
      margin-bottom: 20px;
    }
```

Replace with:

```css
    .controls {
      display: flex;
      justify-content: center;
      gap: 12px;
      margin-bottom: 20px;
    }
```

- [ ] **Step 3: Add Check Answers button HTML**

Find:

```html
  <div class="controls">
    <button class="btn-reset" id="resetBtn">Reset All</button>
  </div>
```

Replace with:

```html
  <div class="controls">
    <button class="btn-reset" id="resetBtn">Reset All</button>
    <button class="btn-check" id="checkBtn" disabled>Check Answers</button>
  </div>
```

- [ ] **Step 4: Add CSS for button and card feedback states**

Find the comment immediately before the `@media` block:

```css
    @media (max-width: 600px) {
```

Insert immediately before it:

```css
    /* ---------- Check Answers button ---------- */
    .btn-check {
      background: var(--accent-b);
      border: 2px solid var(--accent-b);
      color: #fff;
      padding: 8px 22px;
      border-radius: var(--btn-radius);
      cursor: pointer;
      font-size: 0.9rem;
      font-family: var(--font-family);
      transition: opacity 0.2s;
    }
    .btn-check:disabled {
      opacity: 0.35;
      cursor: not-allowed;
    }
    .btn-check:not(:disabled):hover {
      opacity: 0.85;
    }

    /* ---------- Card feedback states ---------- */
    .column-a .card.card-correct,
    .column-b .card.card-correct {
      border-color: #4caf50;
      border-left: 4px solid #4caf50;
    }
    .column-a .card.card-wrong,
    .column-b .card.card-wrong {
      border-color: #e94560;
      border-left: 4px solid #e94560;
    }
    .card-result-icon {
      font-weight: bold;
      margin-left: 6px;
    }
    .column-a .card.card-correct .card-result-icon,
    .column-b .card.card-correct .card-result-icon { color: #4caf50; }
    .column-a .card.card-wrong .card-result-icon,
    .column-b .card.card-wrong .card-result-icon { color: #e94560; }

```

- [ ] **Step 5: Add module-level variable declarations**

Find this block near the top of the `<script>`:

```js
    var nameA = document.getElementById('colA').getAttribute('data-name');
    var nameB = document.getElementById('colB').getAttribute('data-name');

    var pool  = document.getElementById('pool');
    var bodyA = document.getElementById('bodyA');
    var bodyB = document.getElementById('bodyB');
    var countAEl = document.getElementById('countA');
    var countBEl = document.getElementById('countB');
```

Replace with:

```js
    var nameA = document.getElementById('colA').getAttribute('data-name');
    var nameB = document.getElementById('colB').getAttribute('data-name');

    var pool  = document.getElementById('pool');
    var bodyA = document.getElementById('bodyA');
    var bodyB = document.getElementById('bodyB');
    var countAEl = document.getElementById('countA');
    var countBEl = document.getElementById('countB');

    var scoreBar;
    var scoreBarOriginal = '';
    var checkBtn;
```

- [ ] **Step 6: Add `updateCheckButton()` and `checkAnswers()` functions**

Find the `updateCounts()` function in the script:

```js
    function updateCounts() {
      countAEl.textContent = bodyA.querySelectorAll('.card').length;
      countBEl.textContent = bodyB.querySelectorAll('.card').length;
    }
```

Insert immediately after it:

```js
    function updateCheckButton() {
      checkBtn.disabled = pool.querySelectorAll('.card').length !== 0;
    }

    function checkAnswers() {
      var cards = document.querySelectorAll('#bodyA .card, #bodyB .card');
      var correctCount = 0;
      var total = cards.length;

      cards.forEach(function(card) {
        var answer = card.getAttribute('data-answer') || '';
        var inA = card.parentNode === bodyA;
        var isCorrect = (inA && answer === 'A') || (!inA && answer === 'B');

        if (isCorrect) { correctCount++; }
        card.classList.add(isCorrect ? 'card-correct' : 'card-wrong');

        var icon = document.createElement('span');
        icon.className = 'card-result-icon';
        icon.textContent = isCorrect ? '\u2713' : '\u2717';
        card.querySelector('.card-text').appendChild(icon);

        var actions = card.querySelector('.card-actions');
        if (actions) { actions.parentNode.removeChild(actions); }
      });

      scoreBar.innerHTML = '<span>' + correctCount + ' / ' + total + ' correct</span>';
      checkBtn.disabled = true;
    }
```

- [ ] **Step 7: Update `moveToColumn()` to call `updateCheckButton()`**

Find:

```js
    function moveToColumn(card, col) {
      if (col === 'A') {
        bodyA.appendChild(card);
      } else {
        bodyB.appendChild(card);
      }
      buildBackButton(card);
      updateCounts();
    }
```

Replace with:

```js
    function moveToColumn(card, col) {
      if (col === 'A') {
        bodyA.appendChild(card);
      } else {
        bodyB.appendChild(card);
      }
      buildBackButton(card);
      updateCounts();
      updateCheckButton();
    }
```

- [ ] **Step 8: Update `returnToPool()` to call `updateCheckButton()`**

Find:

```js
    function returnToPool(card) {
      pool.appendChild(card);
      buildPoolButtons(card);
      updateCounts();
    }
```

Replace with:

```js
    function returnToPool(card) {
      pool.appendChild(card);
      buildPoolButtons(card);
      updateCounts();
      updateCheckButton();
    }
```

- [ ] **Step 9: Replace `resetAll()` with the full updated version**

Find:

```js
    function resetAll() {
      var sortedCards = document.querySelectorAll('#bodyA .card, #bodyB .card');
      sortedCards.forEach(function(card) {
        pool.appendChild(card);
        buildPoolButtons(card);
      });
      updateCounts();
    }
```

Replace with (note: the existing `updateCounts()` call is removed and replaced by the full 6-step sequence):

```js
    function resetAll() {
      var sortedCards = document.querySelectorAll('#bodyA .card, #bodyB .card');
      sortedCards.forEach(function(card) {
        pool.appendChild(card);
        buildPoolButtons(card);
      });

      document.querySelectorAll('.card').forEach(function(card) {
        card.classList.remove('card-correct', 'card-wrong');
        var icon = card.querySelector('.card-result-icon');
        if (icon) { icon.parentNode.removeChild(icon); }
      });

      scoreBar.innerHTML = scoreBarOriginal;
      countAEl = document.getElementById('countA');
      countBEl = document.getElementById('countB');
      updateCounts();
      checkBtn.disabled = true;
    }
```

- [ ] **Step 10: Update `init()` to wire up Check Answers**

Find:

```js
    function init() {
      var cards = pool.querySelectorAll('.card');
      cards.forEach(function(card) {
        buildPoolButtons(card);
      });
      updateCounts();
    }
```

Replace with:

```js
    function init() {
      var cards = pool.querySelectorAll('.card');
      cards.forEach(function(card) {
        buildPoolButtons(card);
      });
      updateCounts();
      scoreBar = document.querySelector('.score-bar');
      scoreBarOriginal = scoreBar.innerHTML;
      checkBtn = document.getElementById('checkBtn');
      checkBtn.addEventListener('click', checkAnswers);
    }
```

- [ ] **Step 3: Commit**

```bash
git add sorting-activity/examples/causes-and-effects.html
git commit -m "feat(example): add data-answer to all cards and implement check-answers feature"
```

---

### Task 9: Manual verification — example file

**No file changes.**

- [ ] **Step 1: Open example in browser**

Open `sorting-activity/examples/causes-and-effects.html`.

- [ ] **Step 2: Verify a perfect score**

Sort all 8 cards correctly (Causes in Cause column, Effects in Effect column) and click Check Answers. Confirm:
- All 8 cards show green border and ✓ icon
- Score shows `8 / 8 correct`

- [ ] **Step 3: Verify a mixed score**

Click Reset All. Sort some cards deliberately wrong. Click Check Answers. Confirm:
- Correct cards: green border, ✓
- Wrong cards: red border, ✗
- Score reflects the actual count

- [ ] **Step 4: Verify locking**

After checking, confirm Back buttons are gone and cards cannot be returned to the pool.

- [ ] **Step 5: Verify reset**

Click Reset All. Confirm all feedback is cleared, score bar reverts to counts, and Check Answers is disabled again.

---

### Task 10: Update the brief

**Files:**
- Modify: `sorting-activity/sorting-activity-brief.md`

- [ ] **Step 1: Update Step 4**

Find:

```markdown
4. **Add your items.** Find the `<!-- ITEM POOL -->` section. Edit the `data-label` attribute and the text inside `<span class="card-text">` on each card. You need at least 4 items; 8 is the default.
```

Replace with:

```markdown
4. **Add your items.** Find the `<!-- ITEM POOL -->` section. For each card, edit:
   - `data-label` and the text inside `<span class="card-text">` — your item text
   - `data-answer="A"` or `data-answer="B"` (uppercase, exact) — the correct category column

   You need at least 4 items; 8 is the default. If `data-answer` is omitted, Check Answers will mark that card as wrong.
```

- [ ] **Step 2: Update Step 7 (Test it)**

Find:

```markdown
7. **Test it.** Open the file in a browser. Click the sort buttons on each card. Check the Reset button works.
```

Replace with:

```markdown
7. **Test it.** Open the file in a browser. Sort all cards into columns, then click **Check Answers** and confirm correct/incorrect feedback appears. Check that Reset All clears the feedback and restores the score bar.
```

- [ ] **Step 3: Update Customisation Options table**

Find the table row for item text:

```markdown
| Item text | `data-label="..."` + `<span class="card-text">` on each `.card` |
```

Insert a new row directly after it:

```markdown
| Correct answer for each card | `data-answer="A"` or `"B"` (uppercase) on each `.card` |
```

- [ ] **Step 4: Update "What a Finished Version Looks Like"**

Find:

```markdown
- Every card correctly sorted into a column when the activity is complete
```

Insert a new bullet directly after it:

```markdown
- Clicking **Check Answers** marks each card correct (green) or incorrect (red) and shows a score
```

- [ ] **Step 5: Remove the check-answers extension bullet**

Find:

```markdown
- Add a **correct answers** feature: give each card a `data-answer` attribute and highlight cards green/red after a "Check Answers" button is clicked.
```

Delete that line entirely (it is now standard, not an extension).

- [ ] **Step 6: Commit**

```bash
git add sorting-activity/sorting-activity-brief.md
git commit -m "docs(brief): update for built-in check-answers feature"
```

---

### Task 11: Final review commit

- [ ] **Step 1: Verify all three files are changed**

```bash
git log --oneline -8
```

Confirm commits for: template HTML, example HTML, brief.

- [ ] **Step 2: Push**

```bash
git push
```
