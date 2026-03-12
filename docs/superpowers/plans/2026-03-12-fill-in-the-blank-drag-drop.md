# Fill-in-the-Blank Drag-and-Drop Implementation Plan

> **For agentic workers:** REQUIRED: Use superpowers:subagent-driven-development (if subagents available) or superpowers:executing-plans to implement this plan. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Replace typed `<input>` blanks in the fill-in-the-blank template with a drag-and-drop word bank, updating both the template file and the worked example.

**Architecture:** Both HTML files are self-contained — CSS and JS live inline. Changes are applied to `fill-in-the-blank-template.html` first (template file + instructional comments), then the same structural changes (minus comment updates) are applied to `examples/urbanisation-facts.html`. No build tools. Verification is manual: open the file in a browser and interact.

**Tech Stack:** Vanilla HTML/CSS/JS (no frameworks, no build tools). HTML5 Drag-and-Drop API.

**Spec:** `docs/superpowers/specs/2026-03-12-fill-in-the-blank-drag-drop-design.md`

---

## Chunk 1: Update the template file

### Task 1: Replace `<input class="blank">` elements with `<span class="blank-drop">` drop zones

**Files:**
- Modify: `fill-in-the-blank/fill-in-the-blank-template.html`

- [ ] **Step 1: Replace all five `<input class="blank">` elements**

In `fill-in-the-blank/fill-in-the-blank-template.html`, find each occurrence of:
```html
<input class="blank" type="text" data-answer="..." placeholder="_____">
```
and replace with:
```html
<span class="blank-drop" data-answer="..."></span>
```

The five replacements (lines ~335, ~346, ~356, ~366, ~376):

```html
<!-- Question 1 — was: <input class="blank" type="text" data-answer="100" placeholder="_____"> -->
<span class="blank-drop" data-answer="100"></span>

<!-- Question 2 — was: <input class="blank" type="text" data-answer="gas" placeholder="_____"> -->
<span class="blank-drop" data-answer="gas"></span>

<!-- Question 3 — was: <input class="blank" type="text" data-answer="melting" placeholder="_____"> -->
<span class="blank-drop" data-answer="melting"></span>

<!-- Question 4 — was: <input class="blank" type="text" data-answer="solid" placeholder="_____"> -->
<span class="blank-drop" data-answer="solid"></span>

<!-- Question 5 — was: <input class="blank" type="text" data-answer="evaporation" placeholder="_____"> -->
<span class="blank-drop" data-answer="evaporation"></span>
```

- [ ] **Step 2: Add the word bank div between the `.instructions` banner and `.quiz-container`**

The template has an `.instructions` div (lines ~292–307) sitting between `</header>` and `.quiz-container`. Insert the word bank immediately after the closing `</div>` of `.instructions` (line ~307), before the `<!-- QUIZ CONTAINER -->` comment:

```html
  <!-- ============================================================
       WORD BANK
       All answer words appear here as draggable chips.
       Students drag chips into the blank slots in each sentence.
       ============================================================ -->
  <div id="word-bank">
    <div class="word-bank-label">Word Bank</div>
    <!-- chips are injected here automatically by JavaScript -->
  </div>
```

- [ ] **Step 3: Commit**

```bash
git add fill-in-the-blank/fill-in-the-blank-template.html
git commit -m "feat: replace input blanks with drop zones and add word bank HTML (template)"
```

---

### Task 2: Replace `input.blank` CSS with drag-and-drop styles

**Files:**
- Modify: `fill-in-the-blank/fill-in-the-blank-template.html`

- [ ] **Step 1: Remove the `input.blank` rule block**

Find and delete this entire block (lines ~140–178, including the HOW TO ADD A BLANK instructional comment):

```css
    /* ---- Blank input ---- */
    /* HOW TO ADD A BLANK:
       Place this element inline inside your sentence text:
       <input class="blank" type="text" data-answer="yourAnswer" placeholder="_____">

       - data-answer  : the correct answer (case-insensitive comparison)
       - placeholder  : hint text shown before the student types
    */
    input.blank {
      background: var(--input-bg);
      border: 2px solid var(--input-border);
      border-radius: 6px;
      color: var(--text-color);
      font-family: var(--font-family);
      font-size: 1rem;
      padding: 0.25rem 0.55rem;
      width: 130px;
      text-align: center;
      outline: none;
      transition: border-color 0.2s, background 0.2s;
      vertical-align: middle;
    }

    input.blank:focus {
      border-color: var(--input-focus);
    }

    input.blank.correct {
      border-color: var(--correct-color);
      background: rgba(72, 187, 120, 0.15);
      color: var(--correct-color);
    }

    input.blank.incorrect {
      border-color: var(--incorrect-color);
      background: rgba(252, 129, 129, 0.12);
      color: var(--incorrect-color);
    }
```

- [ ] **Step 2: Add the new drag-and-drop CSS rules in place of the deleted block**

Insert the following in the same location:

```css
    /* ---- Word bank ---- */
    #word-bank {
      max-width: 780px;
      margin: 0 auto 1.5rem;
      background: var(--card-bg);
      border: 1px solid var(--card-border);
      border-radius: var(--card-radius);
      padding: 1rem 1.2rem;
      display: flex;
      flex-wrap: wrap;
      gap: 0.6rem;
      align-items: center;
    }

    #word-bank.drag-over {
      border-color: var(--accent);
    }

    .word-bank-label {
      width: 100%;
      font-size: 0.78rem;
      text-transform: uppercase;
      letter-spacing: 0.1em;
      color: var(--muted-text);
      margin-bottom: 0.2rem;
    }

    /* ---- Draggable word chips ---- */
    .word-chip {
      display: inline-block;
      background: var(--input-bg);
      border: 2px solid var(--accent);
      border-radius: 20px;
      padding: 0.25rem 0.8rem;
      font-size: 0.95rem;
      font-family: var(--font-family);
      color: var(--text-color);
      cursor: grab;
      transition: transform 0.1s;
      user-select: none;
    }

    .word-chip:hover  { transform: translateY(-2px); }
    .word-chip:active { cursor: grabbing; }

    /* ---- Blank drop zones ---- */
    /* HOW TO ADD A BLANK:
       Place this element inline inside your sentence text:
       <span class="blank-drop" data-answer="yourAnswer"></span>

       - data-answer : the correct answer (case-insensitive comparison)
    */
    .blank-drop {
      display: inline-block;
      vertical-align: middle;
      min-width: 130px;
      min-height: 36px;
      border-bottom: 2px solid var(--input-border);
      border-radius: 6px;
      text-align: center;
      line-height: 36px;
      transition: border-color 0.2s, background 0.2s;
    }

    .blank-drop.drag-over {
      border: 2px dashed var(--accent);
    }

    .blank-drop.correct {
      border-color: var(--correct-color);
      background: rgba(72, 187, 120, 0.15);
    }

    .blank-drop.incorrect {
      border-color: var(--incorrect-color);
      background: rgba(252, 129, 129, 0.12);
    }
```

- [ ] **Step 3: Remove the `input.blank` rule from the responsive media query**

Find the `@media (max-width: 520px)` block (near the bottom of the `<style>` section). It currently looks like:

```css
    @media (max-width: 520px) {
      header h1 {
        font-size: 1.6rem;
      }

      input.blank {
        width: 100px;
      }

      .sentence-text {
        font-size: 1rem;
      }
    }
```

Remove only the `input.blank` rule, leaving the other two:

```css
    @media (max-width: 520px) {
      header h1 {
        font-size: 1.6rem;
      }

      .sentence-text {
        font-size: 1rem;
      }
    }
```

- [ ] **Step 4: Commit**

```bash
git add fill-in-the-blank/fill-in-the-blank-template.html
git commit -m "feat: add drag-and-drop CSS, remove input.blank styles (template)"
```

---

### Task 3: Replace the JS block with drag-and-drop logic

**Files:**
- Modify: `fill-in-the-blank/fill-in-the-blank-template.html`

- [ ] **Step 1: Replace the entire `<script>` block**

Find the opening `<script>` tag (line ~396) through the closing `</script>` tag (line ~492) and replace the entire contents with the block below. Note: the existing script contains a `document.addEventListener("keydown", ...)` handler for Enter-key navigation between inputs — this is intentionally removed since there are no text inputs in the new design.

```html
  <script>
    /* ============================================================
       DRAG-AND-DROP LOGIC
       You do not need to edit this script.
       It automatically reads however many .blank-drop spans you add.
       ============================================================ */

    var draggedChip = null;      /* chip element currently being dragged */
    var dragSourceBlank = null;  /* .blank-drop the chip came from, or null if from word bank */

    /* ---- Chip drag handlers ---- */
    function onChipDragStart(event) {
      draggedChip = event.target;
      dragSourceBlank = event.target.parentElement.classList.contains('blank-drop')
        ? event.target.parentElement : null;
      event.dataTransfer.setData('text/plain', event.target.dataset.chipId);
    }

    function onChipDragEnd() {
      draggedChip = null;
      dragSourceBlank = null;
    }

    /* ---- Drop zone handlers ---- */
    function onDropZoneDragOver(event) {
      event.preventDefault();
      event.currentTarget.classList.add('drag-over');
    }

    function onDropZoneDragLeave(event) {
      if (event.currentTarget.contains(event.relatedTarget)) { return; }
      event.currentTarget.classList.remove('drag-over');
    }

    function onDropZoneDrop(event) {
      event.preventDefault();
      var targetBlank = event.currentTarget;
      targetBlank.classList.remove('drag-over');
      if (!draggedChip) { return; }
      if (draggedChip.parentElement === targetBlank) { return; }

      /* Clear source blank feedback state */
      if (dragSourceBlank) {
        dragSourceBlank.classList.remove('correct', 'incorrect');
      }

      /* Handle displaced chip */
      var displacedChip = targetBlank.querySelector('.word-chip');
      if (displacedChip) {
        if (dragSourceBlank) {
          dragSourceBlank.appendChild(displacedChip); /* swap */
        } else {
          document.getElementById('word-bank').appendChild(displacedChip);
        }
      }

      targetBlank.classList.remove('correct', 'incorrect');
      targetBlank.appendChild(draggedChip);
    }

    /* ---- Word bank handlers ---- */
    function onWordBankDragOver(event) {
      event.preventDefault();
      event.currentTarget.classList.add('drag-over');
    }

    function onWordBankDragLeave(event) {
      if (event.currentTarget.contains(event.relatedTarget)) { return; }
      event.currentTarget.classList.remove('drag-over');
    }

    function onWordBankDrop(event) {
      event.preventDefault();
      var wordBank = document.getElementById('word-bank');
      wordBank.classList.remove('drag-over');
      if (!draggedChip) { return; }
      if (draggedChip.parentElement === wordBank) { return; }
      if (dragSourceBlank) {
        dragSourceBlank.classList.remove('correct', 'incorrect');
      }
      wordBank.appendChild(draggedChip);
    }

    /* ---- Check all blanks against their answers ---- */
    function checkAll() {
      var blanks = document.querySelectorAll('.blank-drop');
      var correct = 0;
      var total = blanks.length;

      blanks.forEach(function (blank) {
        var card   = blank.closest('.sentence-card');
        var reveal = card.querySelector('.answer-reveal');

        blank.classList.remove('correct', 'incorrect');
        reveal.classList.remove('visible');
        reveal.textContent = '';

        var chip          = blank.querySelector('.word-chip');
        var studentAnswer = chip ? chip.textContent.trim().toLowerCase() : '';
        var correctAnswer = blank.dataset.answer.trim().toLowerCase();

        if (studentAnswer === correctAnswer) {
          blank.classList.add('correct');
          correct = correct + 1;
        } else {
          blank.classList.add('incorrect');
          reveal.textContent = 'Correct answer: ' + blank.dataset.answer;
          reveal.classList.add('visible');
        }
      });

      var scoreDisplay = document.getElementById('score-display');
      var scoreText    = document.getElementById('score-text');
      var scoreNote    = document.getElementById('score-note');

      scoreText.textContent = 'You got ' + correct + ' / ' + total + ' correct!';

      if (correct === total) {
        scoreNote.textContent = 'Excellent work! Full marks!';
      } else if (correct >= Math.ceil(total / 2)) {
        scoreNote.textContent = 'Good effort. Review the highlighted answers and try again.';
      } else {
        scoreNote.textContent = 'Keep practising. Read each sentence carefully and try again.';
      }

      scoreDisplay.classList.add('visible');
      scoreDisplay.scrollIntoView({ behavior: 'smooth', block: 'nearest' });
    }

    /* ---- Reset all blanks and re-shuffle word bank ---- */
    function tryAgain() {
      var blanks   = document.querySelectorAll('.blank-drop');
      var wordBank = document.getElementById('word-bank');

      blanks.forEach(function (blank) {
        var chip = blank.querySelector('.word-chip');
        if (chip) { wordBank.appendChild(chip); }
        blank.classList.remove('correct', 'incorrect');
      });

      var chips = Array.prototype.slice.call(wordBank.querySelectorAll('.word-chip'));
      for (var i = chips.length - 1; i > 0; i--) {
        var j    = Math.floor(Math.random() * (i + 1));
        var temp = chips[i]; chips[i] = chips[j]; chips[j] = temp;
      }
      chips.forEach(function (c) { wordBank.appendChild(c); });

      document.querySelectorAll('.sentence-card').forEach(function (card) {
        var reveal = card.querySelector('.answer-reveal');
        reveal.classList.remove('visible');
        reveal.textContent = '';
      });

      document.getElementById('score-display').classList.remove('visible');
    }

    /* ---- Build word bank from all blank answers ---- */
    function buildWordBank() {
      var wordBank = document.getElementById('word-bank');
      var blanks   = document.querySelectorAll('.blank-drop');
      var answers  = [];

      blanks.forEach(function (blank) {
        answers.push(blank.dataset.answer);
      });

      /* Fisher-Yates shuffle */
      for (var i = answers.length - 1; i > 0; i--) {
        var j    = Math.floor(Math.random() * (i + 1));
        var temp = answers[i]; answers[i] = answers[j]; answers[j] = temp;
      }

      answers.forEach(function (answer, i) {
        var chip = document.createElement('div');
        chip.className = 'word-chip';
        chip.setAttribute('draggable', 'true');
        chip.dataset.chipId = 'chip-' + i;
        chip.textContent = answer;
        chip.addEventListener('dragstart', onChipDragStart);
        chip.addEventListener('dragend',   onChipDragEnd);
        wordBank.appendChild(chip);
      });

      document.querySelectorAll('.blank-drop').forEach(function (blank) {
        blank.addEventListener('dragover',  onDropZoneDragOver);
        blank.addEventListener('dragleave', onDropZoneDragLeave);
        blank.addEventListener('drop',      onDropZoneDrop);
      });

      wordBank.addEventListener('dragover',  onWordBankDragOver);
      wordBank.addEventListener('dragleave', onWordBankDragLeave);
      wordBank.addEventListener('drop',      onWordBankDrop);
    }

    buildWordBank();
  </script>
```

- [ ] **Step 2: Verify in browser**

Open `fill-in-the-blank/fill-in-the-blank-template.html` in a browser. Confirm:
- Word bank appears above the quiz with 5 shuffled word chips
- Chips can be dragged into blank slots — chip snaps into the slot
- Dragging a chip from one blank to another swaps them
- Dragging a chip back to the word bank empties the blank
- "Check All" marks correct blanks green and incorrect blanks red, shows correct answer text
- "Try Again" returns all chips to the word bank (re-shuffled) and clears all feedback

- [ ] **Step 3: Commit**

```bash
git add fill-in-the-blank/fill-in-the-blank-template.html
git commit -m "feat: add drag-and-drop JS logic (template)"
```

---

### Task 4: Update the template's instructional comments

**Files:**
- Modify: `fill-in-the-blank/fill-in-the-blank-template.html`

- [ ] **Step 1: Update the Setup Guide banner**

Find the `.instructions` div (lines ~292–307). Replace its content with:

```html
  <div class="instructions">
    <strong>Setup Guide (delete this box before sharing)</strong>
    <ol>
      <li>Change the <code>&lt;h1&gt;</code> to your quiz title and the subtitle to your topic.</li>
      <li>Edit the five <code>.sentence-card</code> blocks below with your own sentences.</li>
      <li>
        Inside each sentence, find the <code>&lt;span class="blank-drop"&gt;</code> element and set
        <code>data-answer="correctWord"</code> to the expected answer (case-insensitive).
      </li>
      <li>
        To add more questions, copy and paste a <code>.sentence-card</code> block and update
        the question number, sentence, and <code>data-answer</code>.
      </li>
      <li>Students drag word chips from the <strong>Word Bank</strong> into the blank slots.</li>
      <li>Adjust colours and fonts in the <code>:root</code> block at the top of the <code>&lt;style&gt;</code> section.</li>
    </ol>
  </div>
```

- [ ] **Step 2: Update the QUIZ CONTAINER comment**

Find the `<!-- QUIZ CONTAINER -->` comment block (lines ~309–313). It currently says:

```html
  <!-- ============================================================
       QUIZ CONTAINER
       Add or remove .sentence-card blocks as needed.
       The JS automatically counts however many .blank inputs exist.
       ============================================================ -->
```

Replace with:

```html
  <!-- ============================================================
       QUIZ CONTAINER
       Add or remove .sentence-card blocks as needed.
       The JS automatically reads however many .blank-drop spans you add.
       ============================================================ -->
```

- [ ] **Step 3: Update the SENTENCE PATTERN comment**

Find the `<!-- ---- SENTENCE PATTERN ... ---- -->` comment block (lines ~316–328). Replace it with:

```html
    <!-- ---- SENTENCE PATTERN (copy this block to add a question) ----

    <div class="sentence-card">
      <div class="q-number">Question N</div>
      <div class="sentence-text">
        Your sentence goes here, and the blank is
        <span class="blank-drop" data-answer="answer"></span>
        right here in the sentence.
      </div>
      <div class="answer-reveal"></div>
    </div>

    ---- END PATTERN ---- -->
```

- [ ] **Step 4: Commit**

```bash
git add fill-in-the-blank/fill-in-the-blank-template.html
git commit -m "docs: update template instructional comments for drag-and-drop"
```

---

## Chunk 2: Update the example file

### Task 5: Apply identical structural changes to the example file

**Files:**
- Modify: `fill-in-the-blank/examples/urbanisation-facts.html`

The example file has no instructional comments or `.instructions` div — only the HTML structure, CSS, and JS need updating.

- [ ] **Step 1: Replace all five `<input class="blank">` elements**

In `fill-in-the-blank/examples/urbanisation-facts.html`, replace each input with a span. Do not add any HTML comments — the example file has no instructional comments.

| Find | Replace with |
|---|---|
| `<input class="blank" type="text" data-answer="cities" placeholder="_____">` | `<span class="blank-drop" data-answer="cities"></span>` |
| `<input class="blank" type="text" data-answer="jobs" placeholder="_____">` | `<span class="blank-drop" data-answer="jobs"></span>` |
| `<input class="blank" type="text" data-answer="urbanised" placeholder="_____">` | `<span class="blank-drop" data-answer="urbanised"></span>` |
| `<input class="blank" type="text" data-answer="pollution" placeholder="_____">` | `<span class="blank-drop" data-answer="pollution"></span>` |
| `<input class="blank" type="text" data-answer="coast" placeholder="_____">` | `<span class="blank-drop" data-answer="coast"></span>` |

- [ ] **Step 2: Add the word bank div between `<header>` and `.quiz-container`**

Find the closing `</header>` tag and insert immediately after it:

```html
  <div id="word-bank">
    <div class="word-bank-label">Word Bank</div>
  </div>
```

- [ ] **Step 3: Replace the `input.blank` CSS rule block**

Find and delete the `input.blank` block (the rule and its three state variants: `:focus`, `.correct`, `.incorrect`). Insert the new rules in the same location:

```css
    #word-bank {
      max-width: 780px;
      margin: 0 auto 1.5rem;
      background: var(--card-bg);
      border: 1px solid var(--card-border);
      border-radius: var(--card-radius);
      padding: 1rem 1.2rem;
      display: flex;
      flex-wrap: wrap;
      gap: 0.6rem;
      align-items: center;
    }

    #word-bank.drag-over {
      border-color: var(--accent);
    }

    .word-bank-label {
      width: 100%;
      font-size: 0.78rem;
      text-transform: uppercase;
      letter-spacing: 0.1em;
      color: var(--muted-text);
      margin-bottom: 0.2rem;
    }

    .word-chip {
      display: inline-block;
      background: var(--input-bg);
      border: 2px solid var(--accent);
      border-radius: 20px;
      padding: 0.25rem 0.8rem;
      font-size: 0.95rem;
      font-family: var(--font-family);
      color: var(--text-color);
      cursor: grab;
      transition: transform 0.1s;
      user-select: none;
    }

    .word-chip:hover  { transform: translateY(-2px); }
    .word-chip:active { cursor: grabbing; }

    .blank-drop {
      display: inline-block;
      vertical-align: middle;
      min-width: 130px;
      min-height: 36px;
      border-bottom: 2px solid var(--input-border);
      border-radius: 6px;
      text-align: center;
      line-height: 36px;
      transition: border-color 0.2s, background 0.2s;
    }

    .blank-drop.drag-over {
      border: 2px dashed var(--accent);
    }

    .blank-drop.correct {
      border-color: var(--correct-color);
      background: rgba(72, 187, 120, 0.15);
    }

    .blank-drop.incorrect {
      border-color: var(--incorrect-color);
      background: rgba(252, 129, 129, 0.12);
    }
```

- [ ] **Step 4: Remove the `input.blank` rule from the responsive media query**

The example file's `@media (max-width: 520px)` block has three rules. Remove only the `input.blank` entry:

```css
    @media (max-width: 520px) {
      header h1 {
        font-size: 1.6rem;
      }

      .sentence-text {
        font-size: 1rem;
      }
    }
```

- [ ] **Step 5: Replace the `<script>` block**

Replace the entire `<script>` block (from `<script>` through `</script>`) with:

```html
  <script>
    var draggedChip = null;
    var dragSourceBlank = null;

    function onChipDragStart(event) {
      draggedChip = event.target;
      dragSourceBlank = event.target.parentElement.classList.contains('blank-drop')
        ? event.target.parentElement : null;
      event.dataTransfer.setData('text/plain', event.target.dataset.chipId);
    }

    function onChipDragEnd() {
      draggedChip = null;
      dragSourceBlank = null;
    }

    function onDropZoneDragOver(event) {
      event.preventDefault();
      event.currentTarget.classList.add('drag-over');
    }

    function onDropZoneDragLeave(event) {
      if (event.currentTarget.contains(event.relatedTarget)) { return; }
      event.currentTarget.classList.remove('drag-over');
    }

    function onDropZoneDrop(event) {
      event.preventDefault();
      var targetBlank = event.currentTarget;
      targetBlank.classList.remove('drag-over');
      if (!draggedChip) { return; }
      if (draggedChip.parentElement === targetBlank) { return; }

      if (dragSourceBlank) {
        dragSourceBlank.classList.remove('correct', 'incorrect');
      }

      var displacedChip = targetBlank.querySelector('.word-chip');
      if (displacedChip) {
        if (dragSourceBlank) {
          dragSourceBlank.appendChild(displacedChip);
        } else {
          document.getElementById('word-bank').appendChild(displacedChip);
        }
      }

      targetBlank.classList.remove('correct', 'incorrect');
      targetBlank.appendChild(draggedChip);
    }

    function onWordBankDragOver(event) {
      event.preventDefault();
      event.currentTarget.classList.add('drag-over');
    }

    function onWordBankDragLeave(event) {
      if (event.currentTarget.contains(event.relatedTarget)) { return; }
      event.currentTarget.classList.remove('drag-over');
    }

    function onWordBankDrop(event) {
      event.preventDefault();
      var wordBank = document.getElementById('word-bank');
      wordBank.classList.remove('drag-over');
      if (!draggedChip) { return; }
      if (draggedChip.parentElement === wordBank) { return; }
      if (dragSourceBlank) {
        dragSourceBlank.classList.remove('correct', 'incorrect');
      }
      wordBank.appendChild(draggedChip);
    }

    function checkAll() {
      var blanks = document.querySelectorAll('.blank-drop');
      var correct = 0;
      var total = blanks.length;

      blanks.forEach(function (blank) {
        var card   = blank.closest('.sentence-card');
        var reveal = card.querySelector('.answer-reveal');

        blank.classList.remove('correct', 'incorrect');
        reveal.classList.remove('visible');
        reveal.textContent = '';

        var chip          = blank.querySelector('.word-chip');
        var studentAnswer = chip ? chip.textContent.trim().toLowerCase() : '';
        var correctAnswer = blank.dataset.answer.trim().toLowerCase();

        if (studentAnswer === correctAnswer) {
          blank.classList.add('correct');
          correct = correct + 1;
        } else {
          blank.classList.add('incorrect');
          reveal.textContent = 'Correct answer: ' + blank.dataset.answer;
          reveal.classList.add('visible');
        }
      });

      var scoreDisplay = document.getElementById('score-display');
      var scoreText    = document.getElementById('score-text');
      var scoreNote    = document.getElementById('score-note');

      scoreText.textContent = 'You got ' + correct + ' / ' + total + ' correct!';

      if (correct === total) {
        scoreNote.textContent = 'Excellent work — full marks!';
      } else if (correct >= Math.ceil(total / 2)) {
        scoreNote.textContent = 'Good effort — review the highlighted answers and try again.';
      } else {
        scoreNote.textContent = 'Keep practising — read each sentence carefully and try again.';
      }

      scoreDisplay.classList.add('visible');
      scoreDisplay.scrollIntoView({ behavior: 'smooth', block: 'nearest' });
    }

    function tryAgain() {
      var blanks   = document.querySelectorAll('.blank-drop');
      var wordBank = document.getElementById('word-bank');

      blanks.forEach(function (blank) {
        var chip = blank.querySelector('.word-chip');
        if (chip) { wordBank.appendChild(chip); }
        blank.classList.remove('correct', 'incorrect');
      });

      var chips = Array.prototype.slice.call(wordBank.querySelectorAll('.word-chip'));
      for (var i = chips.length - 1; i > 0; i--) {
        var j    = Math.floor(Math.random() * (i + 1));
        var temp = chips[i]; chips[i] = chips[j]; chips[j] = temp;
      }
      chips.forEach(function (c) { wordBank.appendChild(c); });

      document.querySelectorAll('.sentence-card').forEach(function (card) {
        var reveal = card.querySelector('.answer-reveal');
        reveal.classList.remove('visible');
        reveal.textContent = '';
      });

      document.getElementById('score-display').classList.remove('visible');
    }

    function buildWordBank() {
      var wordBank = document.getElementById('word-bank');
      var blanks   = document.querySelectorAll('.blank-drop');
      var answers  = [];

      blanks.forEach(function (blank) {
        answers.push(blank.dataset.answer);
      });

      for (var i = answers.length - 1; i > 0; i--) {
        var j    = Math.floor(Math.random() * (i + 1));
        var temp = answers[i]; answers[i] = answers[j]; answers[j] = temp;
      }

      answers.forEach(function (answer, i) {
        var chip = document.createElement('div');
        chip.className = 'word-chip';
        chip.setAttribute('draggable', 'true');
        chip.dataset.chipId = 'chip-' + i;
        chip.textContent = answer;
        chip.addEventListener('dragstart', onChipDragStart);
        chip.addEventListener('dragend',   onChipDragEnd);
        wordBank.appendChild(chip);
      });

      document.querySelectorAll('.blank-drop').forEach(function (blank) {
        blank.addEventListener('dragover',  onDropZoneDragOver);
        blank.addEventListener('dragleave', onDropZoneDragLeave);
        blank.addEventListener('drop',      onDropZoneDrop);
      });

      wordBank.addEventListener('dragover',  onWordBankDragOver);
      wordBank.addEventListener('dragleave', onWordBankDragLeave);
      wordBank.addEventListener('drop',      onWordBankDrop);
    }

    buildWordBank();
  </script>
```

Note: the existing example script uses slightly different score feedback strings ("Excellent work — full marks!" with an em-dash). The replacement above preserves those strings to match the example file's existing style.

- [ ] **Step 6: Verify in browser**

Open `fill-in-the-blank/examples/urbanisation-facts.html` in a browser. Confirm:
- Word bank shows 5 shuffled chips: cities, jobs, urbanised, pollution, coast
- All drag-and-drop interactions work as expected
- Check All and Try Again both work correctly

- [ ] **Step 7: Commit**

```bash
git add fill-in-the-blank/examples/urbanisation-facts.html
git commit -m "feat: add drag-and-drop word bank to urbanisation example"
```
