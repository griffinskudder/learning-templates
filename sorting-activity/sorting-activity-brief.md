# Sorting Activity — Student Brief

## What you are making

An interactive webpage where visitors click buttons to sort items into two categories. You choose the topic, the category names, and the items.

---

## Learning Objectives

**Subject skills**
- Organise and classify information for a chosen topic
- Demonstrate understanding of how items differ across two categories

**Digital technology skills**
- Edit an HTML file to change text content
- Use CSS custom properties (variables) to restyle a page
- Understand how HTML attributes pass information to JavaScript

---

## Steps

1. **Open** `sorting-activity-template.html` in a code editor (e.g. VS Code).

2. **Choose your topic.** Good options include:
   - Advantages / Disadvantages
   - Natural causes / Human causes
   - Living / Non-living
   - Primary sources / Secondary sources
   - Renewable / Non-renewable energy

3. **Set your category names.** Find the two `<div class="column ...">` elements. Change the `data-name="..."` attribute and the `<h2>` heading text inside each one to your two category names.

4. **Add your items.** Find the `<!-- ITEM POOL -->` section. Edit the `data-label` attribute and the text inside `<span class="card-text">` on each card. You need at least 4 items; 8 is the default.

5. **Update the page title and heading.** Change the `<title>` tag near the top and the `<h1>` heading to reflect your topic.

6. **Customise the colours** (optional). In the `:root` block near the top of the `<style>` section, change `--accent-a` and `--accent-b` to colours that suit your categories.

7. **Test it.** Open the file in a browser. Click the sort buttons on each card. Check the Reset button works.

8. **Delete the yellow instructions box.** Remove the entire `<div class="instructions">` block from the HTML before sharing your finished page.

---

## Customisation Options

| What to change | Where to find it |
|---|---|
| Category names | `data-name="..."` on `.column` divs + `<h2>` text |
| Item text | `data-label="..."` + `<span class="card-text">` on each `.card` |
| Number of items | Copy or delete `<div class="card" ...>` blocks in the pool |
| Category A accent colour | `--accent-a` in `:root` |
| Category B accent colour | `--accent-b` in `:root` |
| Page background | `--bg-color` in `:root` |
| Font | `--font-family` in `:root` |
| Page heading and subtitle | `<h1>` and `<p class="subtitle">` in `<header>` |

---

## What a Finished Version Looks Like

- A clear heading naming the topic (e.g. "Causes of Climate Change")
- 8 item cards in the pool area, each labelled with a real piece of content
- Two columns with distinct colours and meaningful headings (e.g. "Natural Causes" in red, "Human Causes" in teal)
- Every card correctly sorted into a column when the activity is complete
- The yellow instructions box is gone
- The page looks good in a browser with no errors in the console

---

## Teacher Notes

### Introducing the activity

Show students a completed example first (with items already sorted) so they understand the end goal before they start editing. Emphasise that they are editing *content*, not building the interaction from scratch.

### Differentiation

**Foundation:** Provide students with the category names and item list already written out. They only need to copy text into the correct attributes.

**Standard:** Students choose their own topic from a class list and write their own 8 items.

**Extension:** Students add a third category column by duplicating the HTML column structure and updating the JavaScript's `nameA`/`nameB` variables (requires reading and modifying the script block).

### Extension activities

- Add a **correct answers** feature: give each card a `data-answer` attribute and highlight cards green/red after a "Check Answers" button is clicked.
- Use the activity as a **peer-teaching tool** — one student builds it, another student uses it to study.
- Embed a short written explanation below the columns: students type a sentence justifying why each item belongs in its category.

### Assessment guidance

- **Process:** Check that students can explain why they chose each category name and item (not just that they copied from a source).
- **Product:** The finished HTML file opens without errors, all items are correctly labelled, and the instructions box has been removed.
- **Reflection prompt:** "What was hardest about choosing which category each item belonged to?"
