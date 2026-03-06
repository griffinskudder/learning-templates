# Vocabulary Builder — Student Brief

## What you are making

A set of interactive digital flashcards. Each card shows a vocabulary word on the front. Click it to flip the card over and reveal the definition. Works for any subject.

---

## Learning Objectives

**Subject skills**
- Recall and explain key vocabulary from your topic

**Digital technology skills**
- Edit HTML text content inside existing tags
- Understand how CSS variables control visual design
- Read and follow code comments to customise a template
- See how a CSS class toggled by JavaScript creates an animation

---

## Steps

1. **Open the file** `vocabulary-builder-template.html` in your code editor (e.g. VS Code) and in a browser so you can see changes as you make them.

2. **Change the heading.** Find the `<h1>` tag near the top of the `<body>` and replace "Vocabulary Builder" with your topic (e.g. "Year 10 Biology — Cell Processes").

3. **Update the subtitle.** Change the `<p class="subtitle">` text to describe your word set.

4. **Edit Card 1.** Find `<!-- ---- Card 1 ---- -->`. Change the text inside `<span class="word">` to your first vocabulary word. Change the text inside `<span class="definition">` to the correct definition.

5. **Repeat for all 6 cards.** Edit Cards 2–6 the same way.

6. **Test your cards.** Save the file, refresh the browser, and click each card to check the flip works.

7. **Delete the yellow instructions banner.** Remove the entire `<div class="instructions">` block once you are happy with your cards.

---

## Customisation Options

| What to change | Where to find it |
|---|---|
| Card background colours | `--front-bg` and `--back-bg` in `:root` |
| Word accent colour | `--word-color` in `:root` |
| Button colour | `--btn-bg` and `--btn-hover` in `:root` |
| Card size | `--card-width` and `--card-height` in `:root` |
| Flip speed | `--flip-duration` in `:root` |
| Font | `--font-family` in `:root` |
| Number of cards | Copy the blank card template from the HTML comments and paste it into the card grid |

---

## What a Finished Version Looks Like

- A dark-themed page with a clear topic heading
- 6 flashcards arranged in a 3-column grid (2 columns on a phone)
- Each card front shows a vocabulary word in an accent colour
- Clicking a card smoothly flips it 180 degrees to reveal the definition
- "Flip All" and "Reset" buttons let the user study efficiently
- A progress counter shows how many cards have been flipped
- No yellow instructions banner visible

---

## Teacher Notes

### Differentiation

**Basic (getting started)**
- Students edit only the word and definition text — no other changes required
- Provide a word list with definitions so they can focus on the coding task

**Standard**
- Students also adjust two or three CSS variables to personalise the colour scheme

**Extended**
- Students add extra cards beyond the original 6 using the blank card template
- Students modify the `--flip-duration` and experiment with easing values in the CSS transition
- Students investigate the `backface-visibility` property and explain in writing why it is needed

---

### Extension Activities

- Add a **subject label** (e.g. a small badge) to each card front using an extra `<span>` and CSS
- Change the flip direction from Y-axis to X-axis by replacing `rotateY` with `rotateX` in the CSS
- Add a **shuffle** button using `Math.random()` to reorder the cards in the DOM
- Convert the progress counter into a percentage bar using a `<div>` whose `width` is set by JS

---

### Assessment Guidance

**Observation prompts during the activity**
- Can the student locate the correct `<span>` tags to edit without changing the wrong elements?
- Does the student save and refresh the browser to verify changes?
- Can the student explain what the "flipped" CSS class does and how JS applies it?

**Completion indicators**
- All 6 cards show correct, subject-relevant vocabulary and definitions
- Cards flip correctly on click and with the Flip All / Reset buttons
- The instructions banner has been removed
- The page heading reflects the student's actual topic

**Extension indicators**
- Additional cards added correctly using the template
- CSS variables changed to a coherent colour scheme
- Student can explain the CSS 3D flip technique in their own words
