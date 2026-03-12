# Fill-in-the-Blank: Drag-and-Drop Word Bank

**Date:** 2026-03-12
**Status:** Approved

## Summary

Replace the typed `<input>` blanks in the fill-in-the-blank template with drag-and-drop drop zones. A shuffled word bank at the top of the page holds all answer words as draggable chips. Students drag chips into inline blank slots in each sentence.

## Decisions

- **Interaction model:** HTML5 native Drag-and-Drop API (`dragstart`, `dragover`, `drop`)
- **Word bank placement:** Single pool above the quiz container, all answers shuffled together
- **Word bank contents:** Correct answers only (no distractors)
- **Typed input:** Removed entirely — drop zones replace `<input class="blank">`
- **Touch / keyboard support:** Out of scope. The template targets desktop/laptop school environments.
- **Drag source tracking:** Module-level `draggedChip` variable is the authoritative reference. `dataTransfer.setData` is called in `dragstart` only because the browser requires it to activate the drop API — its value is never read back.
- **Chip display casing:** Chips display the `data-answer` value as written by the author (original casing preserved). Comparison at check time lowercases both sides, so author casing does not affect correctness.
- **Mid-session return:** When a chip is dropped back to the word bank mid-session it is appended to the end (unshuffled). Words are only re-shuffled on `tryAgain()`.
- **Word bank empty state:** When all chips are placed, the word bank retains its height and shows only the label — no placeholder message needed.
- **Rapid tryAgain during drag:** If `tryAgain()` is called while a chip is being dragged, the dragged chip will be orphaned. This is an accepted edge case — students are unlikely to trigger it in normal use.
- **Responsive behaviour:** The responsive media query for `input.blank` is removed. No responsive rules are added for `.blank-drop` or `.word-chip` — chips wrap naturally in the word bank, and the template is primarily used on desktop. This is out of scope.

## HTML Structure Changes

### Drop zones (replace `<input class="blank">`)

```html
<span class="blank-drop" data-answer="answer"></span>
```

- Inline `<span>` sitting within sentence text
- `data-answer` carries the correct answer (original author casing; comparison is case-insensitive)
- Fixed `min-width: 130px` so the slot does not reflow when a chip is placed or removed
- Underlined slot appearance when empty
- At most one `.word-chip` child at a time

### Word bank (new, above `.quiz-container`)

```html
<div id="word-bank">
  <div class="word-bank-label">Word Bank</div>
  <!-- chips injected by JS on page load -->
</div>
```

- JS reads all `data-answer` values at page load, shuffles them, renders one `.word-chip` per answer
- Duplicate answer words produce two separate chips (each with a unique `data-chip-id`)
- Flex-wrap chip layout
- Same `max-width: 780px` and centred margin as `.quiz-container`

### Word chip (JS-generated)

```html
<div class="word-chip" draggable="true" data-chip-id="chip-0">word</div>
```

- Each chip is assigned a unique `data-chip-id` at creation time: `"chip-" + index`
- Chips retain `draggable="true"` at all times — whether in the word bank or inside a drop zone
- `textContent` is the `data-answer` value as written by the author

## Drag-and-Drop Behaviour

| Action | Result |
|---|---|
| Drag chip from word bank → empty blank | Chip moves into blank |
| Drag chip from word bank → occupied blank | Existing chip appended to end of word bank; `.correct`/`.incorrect` cleared from target blank; new chip placed in blank |
| Drag chip from blank → word bank | Chip appended to end of word bank; `.correct`/`.incorrect` cleared from source blank |
| Drag chip from blank → empty blank | Chip moves to target blank; `.correct`/`.incorrect` cleared from source blank |
| Drag chip from blank → occupied blank | Chips swap: dragged chip goes to target blank, displaced chip goes to source blank; `.correct`/`.incorrect` cleared from both blanks |
| Drag chip → same blank it came from | No change (no-op) |
| Drop outside any valid target | No DOM change; chip remains where it was; `draggedChip` reset to `null` on `dragend` |
| `dragover` a drop zone or word bank | `event.preventDefault()` called; `.drag-over` class applied |
| `dragleave` / `drop` | `.drag-over` class removed |

**`dragleave` bubbling:** Both `.blank-drop` and `#word-bank` `dragleave` handlers return early (no action) when `event.currentTarget.contains(event.relatedTarget)` is `true` (pointer moved to a child element, not a real leave). `element.contains(null)` returns `false` in all current browsers, so no null-check on `relatedTarget` is needed.

## CSS Changes

### New rules

**`.blank-drop`**
```css
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
.blank-drop.correct   { border-color: var(--correct-color);   background: rgba(72,187,120,0.15); }
.blank-drop.incorrect { border-color: var(--incorrect-color); background: rgba(252,129,129,0.12); }
```

**`.blank-drop.drag-over`**
```css
.blank-drop.drag-over {
  border: 2px dashed var(--accent);
}
```

**`#word-bank.drag-over`**
```css
#word-bank.drag-over {
  border-color: var(--accent);
}
```

**`.word-chip`**
```css
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
```

**`#word-bank`**
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
```

**`.word-bank-label`**
```css
.word-bank-label {
  width: 100%;
  font-size: 0.78rem;
  text-transform: uppercase;
  letter-spacing: 0.1em;
  color: var(--muted-text);
  margin-bottom: 0.2rem;
}
```

### Removed rules

- `input.blank` and all its states (replaced by `.blank-drop`)
- `input.blank` block inside `@media (max-width: 520px)` — the entire media query block is removed if it becomes empty, otherwise remaining rules are kept

## JS Changes

### Module-level state

Declared at the top of the `<script>` block, before any function definitions:

```js
var draggedChip = null;       // chip element currently being dragged
var dragSourceBlank = null;   // .blank-drop the chip came from, or null if from word bank
```

### Initialisation (`buildWordBank()`)

Defined as a named function, then called as the last statement in the `<script>` block (consistent with the inline pattern of the existing files):

```js
buildWordBank();
```

Steps:

1. Collect all `data-answer` values from `.blank-drop` elements (preserving duplicates)
2. Fisher-Yates shuffle the array
3. For each value at index `i`, create a chip and attach its listeners:
   ```js
   var chip = document.createElement('div');
   chip.className = 'word-chip';
   chip.setAttribute('draggable', 'true');
   chip.dataset.chipId = 'chip-' + i;
   chip.textContent = answers[i];
   chip.addEventListener('dragstart', onChipDragStart);
   chip.addEventListener('dragend', onChipDragEnd);
   wordBank.appendChild(chip);
   ```
4. Attach drop-target listeners to each `.blank-drop`:
   ```js
   document.querySelectorAll('.blank-drop').forEach(function (blank) {
     blank.addEventListener('dragover',  onDropZoneDragOver);
     blank.addEventListener('dragleave', onDropZoneDragLeave);
     blank.addEventListener('drop',      onDropZoneDrop);
   });
   ```
5. Attach drop-target listeners to `#word-bank`:
   ```js
   wordBank.addEventListener('dragover',  onWordBankDragOver);
   wordBank.addEventListener('dragleave', onWordBankDragLeave);
   wordBank.addEventListener('drop',      onWordBankDrop);
   ```

### Named handler functions

**`onChipDragStart(event)`**
```js
draggedChip = event.target;
dragSourceBlank = event.target.parentElement.classList.contains('blank-drop')
  ? event.target.parentElement : null;
event.dataTransfer.setData('text/plain', event.target.dataset.chipId);
```

**`onChipDragEnd(event)`**
```js
draggedChip = null;
dragSourceBlank = null;
```

**`onDropZoneDragOver(event)` / `onWordBankDragOver(event)`**
```js
event.preventDefault();
event.currentTarget.classList.add('drag-over');
```

**`onDropZoneDragLeave(event)` / `onWordBankDragLeave(event)`**
```js
if (event.currentTarget.contains(event.relatedTarget)) { return; }
event.currentTarget.classList.remove('drag-over');
```

**`onDropZoneDrop(event)`**
```js
event.preventDefault();
var targetBlank = event.currentTarget;
targetBlank.classList.remove('drag-over');
if (!draggedChip) { return; }
if (draggedChip.parentElement === targetBlank) { return; } // same blank — no-op

// Clear source blank state regardless of whether target is occupied
if (dragSourceBlank) {
  dragSourceBlank.classList.remove('correct', 'incorrect');
}

var displacedChip = targetBlank.querySelector('.word-chip');
if (displacedChip) {
  if (dragSourceBlank) {
    dragSourceBlank.appendChild(displacedChip); // swap: displaced goes to source blank
  } else {
    document.getElementById('word-bank').appendChild(displacedChip); // displaced goes to word bank
  }
}

targetBlank.classList.remove('correct', 'incorrect');
targetBlank.appendChild(draggedChip);
```

**`onWordBankDrop(event)`**
```js
event.preventDefault();
var wordBank = document.getElementById('word-bank');
wordBank.classList.remove('drag-over');
if (!draggedChip) { return; }
if (draggedChip.parentElement === wordBank) { return; } // already here — no-op
if (dragSourceBlank) {
  dragSourceBlank.classList.remove('correct', 'incorrect');
}
wordBank.appendChild(draggedChip);
```

### `checkAll()` replacement

The complete `checkAll()` function body, replacing the existing one:

```js
function checkAll() {
  var blanks = document.querySelectorAll('.blank-drop');
  var correct = 0;
  var total = blanks.length;

  blanks.forEach(function (blank) {
    var card   = blank.closest('.sentence-card');
    var reveal = card.querySelector('.answer-reveal');

    // Clear previous state
    blank.classList.remove('correct', 'incorrect');
    reveal.classList.remove('visible');
    reveal.textContent = '';

    var chip          = blank.querySelector('.word-chip');
    var studentAnswer = chip ? chip.textContent.trim().toLowerCase() : '';
    var correctAnswer = blank.dataset.answer.trim().toLowerCase();

    if (studentAnswer === correctAnswer) {
      blank.classList.add('correct');
      correct++;
    } else {
      blank.classList.add('incorrect');
      reveal.textContent = 'Correct answer: ' + blank.dataset.answer;
      reveal.classList.add('visible');
    }
  });

  // Score display — unchanged from existing logic
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
```

### `tryAgain()` replacement

The complete `tryAgain()` function body, replacing the existing one:

```js
function tryAgain() {
  var blanks   = document.querySelectorAll('.blank-drop');
  var wordBank = document.getElementById('word-bank');

  // 1. Return all placed chips to word bank and clear blank states
  blanks.forEach(function (blank) {
    var chip = blank.querySelector('.word-chip');
    if (chip) { wordBank.appendChild(chip); }
    blank.classList.remove('correct', 'incorrect');
  });

  // 2. Collect all chips now in word bank, shuffle, re-append
  var chips = Array.prototype.slice.call(wordBank.querySelectorAll('.word-chip'));
  for (var i = chips.length - 1; i > 0; i--) {
    var j    = Math.floor(Math.random() * (i + 1));
    var temp = chips[i]; chips[i] = chips[j]; chips[j] = temp;
  }
  chips.forEach(function (c) { wordBank.appendChild(c); });

  // 3. Clear answer-reveal divs
  document.querySelectorAll('.sentence-card').forEach(function (card) {
    var reveal = card.querySelector('.answer-reveal');
    reveal.classList.remove('visible');
    reveal.textContent = '';
  });

  // 4. Hide score display
  document.getElementById('score-display').classList.remove('visible');
}
```

### Removed

- Enter-key navigation (no inputs to navigate between)
- `input.blank` focus call in old `tryAgain()`

## Files to Change

Both files require identical structural changes:

1. **Drop zones:** Replace every `<input class="blank" type="text" data-answer="..." placeholder="_____">` with `<span class="blank-drop" data-answer="..."></span>`
2. **Word bank:** Add `<div id="word-bank"><div class="word-bank-label">Word Bank</div></div>` between `<header>` and `.quiz-container`
3. **CSS:** Replace `input.blank` rules (including the responsive media query block entry) with the new `.blank-drop`, `.blank-drop.drag-over`, `#word-bank.drag-over`, `.word-chip`, `#word-bank`, and `.word-bank-label` rules
4. **JS:** Replace the entire `<script>` block with the new drag-and-drop logic (module-level state, all named handler functions, `buildWordBank()`, `checkAll()`, `tryAgain()`, and the `buildWordBank()` call at the end)
5. **Template instructions:** In `fill-in-the-blank-template.html` only — update the `.instructions` div and the `<!-- SENTENCE PATTERN -->` HTML comment to reference `<span class="blank-drop" data-answer="..."></span>` instead of `<input class="blank" ...>`. Update the setup steps to describe the drag-and-drop interaction.

**Files:**
- `fill-in-the-blank/fill-in-the-blank-template.html`
- `fill-in-the-blank/examples/urbanisation-facts.html`
