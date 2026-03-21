# Sorting Activity — Check Answers Feature Design

**Date:** 2026-03-22

## Overview

Add a "Check Answers" button to the sorting activity template and example that lets students verify their sorting once all cards have been placed. Each card is marked correct or incorrect with a coloured border and a tick/cross icon. A score summary replaces the count display in the score bar.

## Decisions

- **Trigger:** Check Answers button, only enabled when the pool is empty (all cards placed).
- **Feedback style:** Coloured border (green = correct, red = wrong) + tick/cross icon appended to card text.
- **Locking:** Back buttons are removed when answers are checked, preventing changes after feedback.
- **Reset:** Clears all feedback, restores Back buttons, re-disables Check Answers, reverts score bar to counts.

## HTML Changes

### `data-answer` attribute on cards

Each `<div class="card">` gains a `data-answer` attribute set to `"A"` or `"B"` (uppercase, exact match) corresponding to the correct column. `"A"` = Category A column (`bodyA`), `"B"` = Category B column (`bodyB`). The values are case-sensitive — `data-answer="a"` will not be recognised and the card will be marked wrong.

```html
<div class="card" data-label="Factories need workers" data-answer="A">
  <span class="card-text">Factories need workers</span>
</div>
```

**Template placeholder cards** (Items 1–8) have `data-answer` omitted. The setup guide comment must note that if `data-answer` is absent or not `"A"`/`"B"`, Check Answers will mark that card as wrong — this is expected behaviour before the student fills in the correct answers.

The setup guide comment is updated to document this attribute.

### Controls bar

A second button is added alongside Reset All:

```html
<button class="btn-check" id="checkBtn" disabled>Check Answers</button>
```

The `.controls` div gains flex layout to space the two buttons. Replace the existing `text-align: center; margin-bottom: 20px;` on `.controls` with:

```css
.controls {
  display: flex;
  justify-content: center;
  gap: 12px;
  margin-bottom: 20px;
}
```

## CSS Changes

### Card feedback states

The existing column rules `.column-a .card` and `.column-b .card` have specificity 0-2-0 (two classes) and set `border-left` shorthand. The feedback rules must match that specificity by including the column class as a prefix. Within each feedback rule block, `border-left-color` must come **after** `border-color` so the longhand overrides the shorthand's left-side value:

```css
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

Note the selectors: `.column-a .card.card-correct` means "a `.card` element that also has class `.card-correct`, inside `.column-a`" — no space between `.card` and `.card-correct`. This has specificity 0-3-0, which beats the existing `.column-a .card` rules (0-2-0), ensuring the feedback border overrides the column accent regardless of source order. Place these rules after the existing column rules.

### Check Answers button

Styled with a solid accent colour (distinct from the ghost-style Reset button) with a `:disabled` state that dims it.

```css
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
```

## JavaScript Changes

### Module-level state

In the existing script, `pool`, `bodyA`, `bodyB`, `countAEl`, and `countBEl` are declared and assigned at module level (top of script, outside any function). Add three more in the same block:

```js
var scoreBar;
var scoreBarOriginal = '';
var checkBtn;
```

No boolean guard flags are needed — locking after check is achieved by removing Back button DOM elements, so those click handlers can never fire again.

### `init()` additions

The existing `init()` ends with `updateCounts()`. Append these lines **after** `updateCounts()` in this exact order (scoreBar must be assigned before its innerHTML is captured):

```js
scoreBar = document.querySelector('.score-bar');
scoreBarOriginal = scoreBar.innerHTML;
checkBtn = document.getElementById('checkBtn');
checkBtn.addEventListener('click', checkAnswers);
```

`scoreBarOriginal` captures the fully-rendered markup — including the count text set by `updateCounts()` just above — so appending after `updateCounts()` is required. The template guarantees all cards start in the pool (none are pre-placed in columns), so at init time both counts are 0 and `scoreBarOriginal` captures "0 in Category A / 0 in Category B" (or equivalent).

### New: `checkAnswers()`

```
1. var cards = document.querySelectorAll('#bodyA .card, #bodyB .card');
2. For each card:
   a. var answer = card.getAttribute('data-answer') || '';
   b. var inA = card.parentNode === bodyA;
   c. isCorrect = (inA && answer === 'A') || (!inA && answer === 'B');
   d. card.classList.add(isCorrect ? 'card-correct' : 'card-wrong');
   e. Create a <span> element: var icon = document.createElement('span'); icon.className = 'card-result-icon'; icon.textContent = isCorrect ? '✓' : '✗'; then append it to card.querySelector('.card-text'). Append to .card-text (the inner span), not to card itself, so the icon sits inline after the card label text.
   f. Remove card.querySelector('.card-actions') from the DOM (removes Back button; locks card). Every sorted card has a `.card-actions` div — it is added by the existing `buildBackButton()` function when the card is moved to a column. Use a null guard for safety: var actions = card.querySelector('.card-actions'); if (actions) { actions.parentNode.removeChild(actions); }
3. var correctCount = 0; (count cards where isCorrect was true during the loop). var total = cards.length.
4. scoreBar.innerHTML = '<span>' + correctCount + ' / ' + total + ' correct</span>';
5. checkBtn.disabled = true;
```

### New: `updateCheckButton()`

```js
function updateCheckButton() {
  checkBtn.disabled = pool.querySelectorAll('.card').length !== 0;
}
```

Called as the last line of `moveToColumn()` and `returnToPool()`. Use `pool.querySelectorAll('.card').length`, not `pool.children.length` — the pool's HTML contains whitespace text nodes which `pool.children.length` would ignore but which could cause off-by-one errors in future. The `.card` query counts only card elements.

### Updated: `moveToColumn()`

Append `updateCheckButton();` after the existing `updateCounts();` call (do not replace it).

### Updated: `returnToPool()`

Append `updateCheckButton();` after the existing `updateCounts();` call (do not replace it). No guard is needed — after `checkAnswers()` the Back button DOM element is removed, so its click listener can never fire.

### Updated: `resetAll()`

Execute in this exact order:

```
1. Move all sorted cards back to pool and rebuild sort buttons — existing behaviour.
   The existing resetAll() does this via:
     var sortedCards = document.querySelectorAll('#bodyA .card, #bodyB .card');
     sortedCards.forEach(function(card) { pool.appendChild(card); buildPoolButtons(card); });
   buildPoolButtons is an existing function in the template that removes .card-actions and adds sort buttons.
   These are synchronous steps.

2. Remove feedback classes and icons from all cards
   (by this point all cards are in the pool):
   document.querySelectorAll('.card').forEach(function(card) {
     card.classList.remove('card-correct', 'card-wrong');
     var icon = card.querySelector('.card-result-icon');
     if (icon) { icon.parentNode.removeChild(icon); }
   });

3. scoreBar.innerHTML = scoreBarOriginal;
   // MUST come before updateCounts() — restores #countA and #countB to the DOM.

4. countAEl = document.getElementById('countA');
   countBEl = document.getElementById('countB');
   // Re-query because the innerHTML replacement in step 3 made the old references stale.
   // Do NOT use var here — these must overwrite the module-level variables, not shadow them.
   // Using var would create local variables and updateCounts() would use the stale module-level references.
   // Confirmed: the existing score bar markup contains <span id="countA"> and <span id="countB">
   // (template lines 379-380), so getElementById will find them after scoreBarOriginal is restored.

5. updateCounts();

6. checkBtn.disabled = true;
   // Direct assignment — pool is full so button should be disabled;
   // this is simpler than calling updateCheckButton() here.
```

## Score Bar Behaviour

| State | Display |
|---|---|
| Sorting in progress | existing two-span count format (from `scoreBarOriginal`) |
| After Check Answers | `6 / 8 correct` (replaces entire `.score-bar` innerHTML) |
| After Reset | `.score-bar` innerHTML restored from `scoreBarOriginal`, count elements re-queried, `updateCounts()` called |

## Example File: Correct Answers

The `causes-and-effects.html` example has 8 cards. Their correct `data-answer` values are:

Note: "Better hospitals and schools" and "People want good schools" are both classified as **Causes** — the presence of good services is a pull factor that draws people to cities (a cause of urbanisation), not a result of it.

| Card text | Correct column | `data-answer` |
|---|---|---|
| More job opportunities | Cause | `"A"` |
| Traffic jams | Effect | `"B"` |
| Better hospitals and schools | Cause | `"A"` |
| Air pollution from cars | Effect | `"B"` |
| People want good schools | Cause | `"A"` |
| Farmland is cleared for buildings | Effect | `"B"` |
| Factories need workers | Cause | `"A"` |
| Housing shortages | Effect | `"B"` |

## Files to Change

1. `sorting-activity/sorting-activity-template.html` — template file
2. `sorting-activity/examples/causes-and-effects.html` — example file (add `data-answer` to each card per table above)
3. `sorting-activity/sorting-activity-brief.md` — update to reflect that check-answers is now built in, not an extension

## Brief Updates

The brief currently lists check-answers as an extension activity (Teacher Notes → Extension activities). Changes needed:

- **Step 4** — add a note that each card also needs a `data-answer="A"` or `data-answer="B"` attribute (uppercase, exact) set to the correct category. If omitted, Check Answers will mark that card as wrong.
- **Step 7 (Test it)** — add: sort all cards, then click Check Answers and confirm the correct/incorrect feedback appears.
- **Customisation Options table** — add a row: `Correct answer for each card | data-answer="A" or "B" on each .card`.
- **What a Finished Version Looks Like** — add: clicking Check Answers marks each card correct or incorrect and shows a score.
- **Teacher Notes → Extension activities** — remove the check-answers bullet since it is now standard.

## Edge Cases

- **All cards in one column:** Valid — `updateCheckButton()` only checks that the pool is empty, not that cards are distributed across both columns. A student who places all cards in one column can still click Check Answers and receive feedback. Cards in the empty column score zero, which is correctly handled by the iteration over both `bodyA` and `bodyB`.

## Out of Scope

- More than two categories (template already supports only A/B).
- Partial checking (pool must be empty before Check Answers is enabled).
- Animated feedback transitions.
