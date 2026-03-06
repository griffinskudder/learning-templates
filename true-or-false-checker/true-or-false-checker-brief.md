# True or False Checker — Task Brief

## For students

---

### What you are building

A True or False quiz page. You write the statements. The page checks whether the person answering is right or wrong.

---

### Learning objectives

**Subject:** You will practise finding and writing accurate facts and common misconceptions about your chosen topic.

**Digital technology:** You will edit an HTML file, use CSS custom properties to change colours and fonts, and understand how HTML attributes store data.

---

### Instructions

1. Open `true-or-false-checker-template.html` in your code editor (e.g. VS Code).
2. Open the same file in your browser. You should see a working quiz with placeholder questions.
3. Read the yellow **Setup Guide** box at the top of the page — it explains every step.
4. Change the `<h1>` title and subtitle to match your topic.
5. Find the first `<div class="statement-card">` block.
6. Edit the `<p class="statement-text">` to write your first statement.
7. Set `data-answer="true"` or `data-answer="false"` on the card to mark the correct answer.
8. Optionally edit the `<p class="explanation">` to add extra info shown after answering.
9. Repeat for each statement. Delete the ones you do not need.
10. To add more statements, copy the example block in the comments and paste it below the last card.
11. When you are happy with the content, delete the yellow **Setup Guide** box from the HTML.
12. Customise the colours by editing the CSS variables in the `:root` block at the top of `<style>`.

---

### Customisation options

Choose one direction to focus on, or mix them:

- **Content-heavy:** Write 10 or more statements covering detailed facts and common misconceptions. Keep the default styling.
- **Design-focused:** Keep 5 statements but change all CSS variables to create your own colour scheme. Try changing `--btn-true-bg`, `--btn-false-bg`, and `--heading-color`.
- **Explanation-focused:** Write fewer statements (4-5) but write detailed explanations for each one that appear after the answer — turn the page into a mini teaching tool, not just a quiz.

---

### What a finished version looks like

A page with a clear title, 5 or more true/false statement cards on your chosen topic. Each card has a True and False button. Clicking a button locks the card, shows whether you were right, and reveals a brief explanation. A score counter at the bottom updates as you go.

---

---

## Teacher notes

---

### How to introduce it

Open the template on the projector and click through a few statements as a class. Point out the `data-answer` attribute in the HTML — ask students "what do you think this does?" before explaining. Then hand out the files.

---

### Differentiation

**Students who find this challenging:**
Follow steps 1–10 only. Replace the placeholder statements with their own topic, set the correct `data-answer` values, and submit. A working quiz with accurate facts is a complete outcome.

**Confident students:**
Write 10+ statements, craft detailed explanations, and redesign the colour scheme. Extend further by adding an image with `<img>` inside a card, or by writing a JavaScript modification (e.g. shuffle the card order on page load).

---

### Extension activities

1. **Shuffle mode:** Add a JavaScript function that randomises the order of statement cards on page load using `Math.random()`. Discuss why shuffling might matter for a fair quiz.
2. **Second page:** Create a separate `answers.html` page that lists all the statements and explanations — link to it from the quiz page using an `<a>` tag.

---

### Assessment guidance

Look for:

- Student has replaced all placeholder statements with content relevant to their chosen topic.
- `data-answer` attributes are set correctly — clicking True or False produces the right feedback.
- Student has modified at least two CSS custom properties and the changes are visible in the browser.
- Explanations (where included) add genuine information, not just "that's correct."
