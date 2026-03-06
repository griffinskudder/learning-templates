# Match the Pairs — Task Brief & Teacher Notes

---

## STUDENT TASK BRIEF

### Title: Match the Pairs — Build Your Own Quiz

### Learning Objectives

**Subject:**
- Identify and explain key terms related to your chosen topic.
- Match vocabulary to accurate definitions.

**Digital Technology:**
- Edit an HTML file to add your own content.
- Use CSS variables to change how a page looks.
- Understand how data is stored in a JavaScript array.

---

### What You Are Building

A matching quiz where someone drags terms to their correct definitions (or uses a dropdown menu to select). Your quiz will have your own topic, your own terms, and your own styling.

---

### Instructions

**Step 1 — Open the file**
Open `match-the-pairs-template.html` in your code editor.

**Step 2 — Choose your topic**
Decide on a subject area and pick 5–8 key terms you want to quiz. Write them down before you start coding.

**Step 3 — Change the title**
Find `var TITLE` near the top of the `<script>` block.
Change the text inside the quotes to your topic title. Example:
```
var TITLE = "Year 9 Geography — Landforms";
```

**Step 4 — Add your pairs**
Find the `var pairs` array. Replace the example entries with your own.
Each line follows this pattern:
```
{ term: "Your Term", definition: "Your definition here.", image: "" },
```
Copy and paste existing lines to add more. Do not delete the `];` at the end.

**Step 5 — (Optional) Add an image**
If you want to show an image instead of (or alongside) a definition:
1. Put your image file in the same folder as the HTML file.
2. Put the filename in the `image` field, e.g. `image: "volcano.jpg"`.

**Step 6 — Change the colours**
Find the `CUSTOMIZATION ZONE` near the top of the `<style>` block.
Change `--accent-color`, `--bg-color`, and others to colours that match your topic.
Use a hex colour code (e.g. `#ff6600`) or any CSS colour name.

**Step 7 — Delete the instructions banner**
Delete the `<div class="setup-banner">...</div>` block from the HTML.

**Step 8 — Test it**
Open the file in a browser. Try both modes (Drag & Drop and Dropdown). Click **Check Answers** to make sure it works.

---

### Customisation Options

Choose at least one direction to make this yours:

1. **Content focus** — Create a quiz with 8 pairs on one very specific topic (e.g. just the causes of World War I, or just geological features of coasts).
2. **Image matching** — Replace text definitions with images. Students match a word to a photo or diagram. Useful for visual topics (biology diagrams, artworks, maps).
3. **Visual redesign** — Change the colour scheme, font, and layout to match your subject's "feel" (e.g. earthy greens for geography, blues and greys for history).

---

### What a Finished Version Looks Like

A polished finished version has:
- A clear title and subtitle describing the exact topic.
- 5–8 pairs with accurate, concise definitions.
- No setup instructions visible on the page.
- Colours and heading changed to suit the topic.
- Both Drag & Drop and Dropdown modes working correctly.
- (Bonus) At least one image pair, with the image file included in the folder.

---

---

## TEACHER NOTES

### How to Introduce It

1. Open the template in a browser on the projector. Demonstrate drag & drop, then switch to dropdown mode.
2. Click **Check Answers** to show how feedback works.
3. Open the same file in a code editor. Show where the `var pairs` array is. Change one term live on the projector and refresh — students see the immediate result.
4. Hand out the files (or share via Google Drive / OneDrive) and let students begin.

---

### Differentiation — Multiple Entry Points

**Foundation (following the steps):**
The student changes `TITLE`, `SUBTITLE`, and replaces the example `pairs` entries with their own terms. They run both modes and submit a working quiz with 5 pairs. This demonstrates reading and modifying an existing data structure.

**Extending (going further):**
- Add 8+ pairs and use the image feature for at least 2 pairs.
- Change multiple CSS variables and explain in a short comment what each change does.
- Change the button labels or the "Drop term here..." placeholder text to match their topic.
- Add a second mode idea (e.g. modify the JS so terms and definitions are both shown shuffled in a grid and students click to select).

---

### Extension Activities

1. **Multiple sets** — Create a second HTML file for a different subtopic. Add links between them so students can quiz themselves on a whole unit.
2. **Timer** — Add a JavaScript countdown timer using `setInterval`. Display elapsed time after clicking Check Answers. (Good challenge for students who are confident with JS.)

---

### Assessment Guidance

Look for these four things:

1. **Content accuracy** — Are the definitions correct? Can the student explain what each term means when asked?
2. **Data structure understanding** — Has the student correctly edited the `pairs` array without breaking the JavaScript syntax? (Missing commas and mismatched brackets are the most common errors.)
3. **CSS variable use** — Has the student changed at least two CSS variables, and are those changes reflected visually in the browser?
4. **Image feature** — (If attempted) Is the image file in the correct folder and correctly referenced in the `image` field? Does it display at the right size?

---

### Common Student Errors

| Error | Symptom | Fix |
|---|---|---|
| Missing comma between pairs | Page goes blank / quiz disappears | Check for missing `,` after each `}` in the pairs array |
| Image file in wrong folder | Broken image icon | Make sure the `.jpg` / `.png` is in the same folder as the HTML file |
| Quotes inside a definition not escaped | JS error in console | Use `'single quotes'` inside a pair that uses double quotes, or vice versa |
| Deleted the `];` at the end of pairs | Page breaks | Restore the closing `];` |

---

### Notes on the Two Modes

- **Drag & Drop** — More engaging; better for students who will present the quiz on a touchscreen or projector. Drag events do not work well on some mobile browsers.
- **Dropdown** — More accessible; works on all devices including tablets and phones. Better for lower-stakes practice.

Students can hand in the file with either or both modes working. The mode toggle is built in and requires no student changes to function.
