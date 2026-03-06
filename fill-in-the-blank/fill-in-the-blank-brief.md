# Fill in the Blank — Student Brief

## What You Are Making

A fill-in-the-blank quiz that works in any web browser. You write sentences with a missing keyword, and a classmate types the answer into the blank to test their knowledge. The page checks answers instantly and shows a score.

---

## Learning Objectives

By the end of this activity you will be able to:

- **Subject skill:** Identify and recall key vocabulary or facts from your topic.
- **Digital tech skill:** Edit HTML attributes to store data, and understand how JavaScript reads that data to compare values.

---

## What You Need

- A text editor (VS Code, Notepad++, or similar)
- A web browser
- The file `fill-in-the-blank-template.html`

---

## Steps

1. **Open** `fill-in-the-blank-template.html` in your text editor.

2. **Change the title.** Find the `<h1>` tag near the top and replace "Fill in the Blank" with your quiz name. Change the `<p class="subtitle">` to your subject and topic.

3. **Write your sentences.** Find the five `<div class="sentence-card">` blocks. In each one, replace the example sentence with your own. Keep the `<input class="blank">` element where the missing word should go.

4. **Set the correct answer.** On each `<input class="blank">` element, change `data-answer="word"` so the value is the correct missing word. Spelling must be exact; capitalisation does not matter.

5. **Update the question numbers.** Change "Question 1", "Question 2", etc. to match your content if you like (e.g. "Question 1 — Photosynthesis").

6. **Add or remove questions.** To add a question, copy one `<div class="sentence-card">` block and paste it below the last one. To remove a question, delete a card block entirely. The score counter adjusts automatically.

7. **Delete the instructions banner.** Find the `<div class="instructions">` block and delete it — this removes the purple setup box students should not see.

8. **Test your quiz.** Open the file in a browser. Type some right and wrong answers and click **Check All** to confirm the feedback works correctly.

---

## Customisation Options

| What to change | Where to find it |
|---|---|
| Background colour | `--bg-color` in the `:root` block |
| Accent / button colour | `--accent` in the `:root` block |
| Correct answer highlight colour | `--correct-color` in the `:root` block |
| Incorrect answer highlight colour | `--incorrect-color` in the `:root` block |
| Font | `--font-family` in the `:root` block |
| Sentence text size | `--font-size-body` in the `:root` block |

---

## What a Finished Version Looks Like

- A heading with your quiz title and topic subtitle.
- Five or more sentence cards, each with a text box embedded in the sentence.
- A **Check All** button that colours correct answers green and incorrect answers red, and reveals the correct word below wrong inputs.
- A score banner: "You got X / Y correct!" with encouraging feedback.
- A **Try Again** button that clears everything and resets the quiz.
- No purple instructions box visible.

---

## Tips

- Keep your blank answers to **one word** where possible — multi-word answers require the student to type them exactly.
- Use the **Enter key** to jump between blanks quickly.
- The template works entirely offline — no internet connection is needed.
