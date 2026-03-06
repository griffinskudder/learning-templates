# Multiple Choice Quiz — Task Brief & Teacher Notes

---

## Task Brief (Student-Facing)

### Build a Quiz About Your Topic Using HTML

**Learning Objectives**

- **Humanities:** Identify the key ideas from a topic and express them as clear, precise questions with plausible wrong answers.
- **Digital Technologies:** Edit HTML elements and attributes; understand how an `if` statement controls program behaviour.

---

### Instructions

1. Open `multiple-choice-quiz-template.html` in your code editor.
2. Change the `<title>` and `<h1>` to the name of your quiz (e.g. *Eureka Stockade Quiz*).
3. Inside `class="question-text"`, type your question.
4. Change the four `<button>` labels to your four answer options.
5. Find the correct answer button. Add `data-correct="true"` inside its opening tag — like this:
   ```html
   <button class="option-btn" data-correct="true">Peter Lalor</button>
   ```
6. Open the file in your browser and test it. Click your answers. Check that correct and wrong answers highlight correctly.
7. Delete the yellow instruction box from the HTML.
8. **Done early?** Add a second question by copying the commented-out block and pasting it inside `id="quiz"`.

**Optional — Add an Image**

- Place your image file in the same folder as the HTML file.
- Find the commented-out `<img>` line inside the question card.
- Remove the comment markers and change `src="your-image.jpg"` to your filename.

---

### Customisation Options

Choose one (or more) directions to make the quiz your own:

- **Content focus** — write 3–5 questions that cover the most important ideas from your topic.
- **Visual style** — change the colours in the `:root` block at the top of the `<style>`. Every variable is labelled.
- **Feedback text** — edit the `feedback.textContent` lines in the `<script>` to give more helpful hints when a student gets an answer wrong.

---

### What a Finished Version Looks Like

A dark-themed quiz page with your title at the top. One or more question cards appear on screen. When a student clicks an answer, correct answers turn green and wrong answers turn red — with a short feedback message. After all questions are answered, a score banner appears with a "Try Again" button.

---

## Teacher Notes

### How to Introduce It (30-minute session)

Open the template on the projector. Click a wrong answer so students see the red highlight, then click the correct one. Point out the yellow instruction box — "this tells you what to change." Distribute the files, give students 2 minutes to read the instructions box themselves, then let them work.

### Differentiation

**Students who need more support:** Following the six steps produces a working, styled quiz with one question. That alone covers all the core objectives. They do not need to add more questions.

**Confident students:** Add 3–5 questions; customise the colour scheme via CSS variables; edit the feedback messages to include a hint rather than just "not quite."

### Extension Activities

- Add a `data-hint` attribute to wrong-answer buttons and update the JS to display that hint in the feedback (e.g. *"Think about who signed the Ballarat Reform League charter"*).
- Randomise the order of the answer buttons each time the quiz reloads — introduce `Math.random()` and `sort()`.

### Assessment — What to Look For

1. Student has written a factually accurate question with one clearly correct answer and three plausible distractors.
2. `data-correct="true"` is on the correct button only (test by clicking each option in the browser).
3. The quiz title and heading reflect the student's actual topic (not the placeholder).
4. Student can explain in one sentence what `data-correct="true"` does in the code.
