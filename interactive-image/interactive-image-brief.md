# Interactive Image — Student Brief

## What You Are Making

An **annotated image** that viewers can explore. You choose the image — a diagram, a photograph, a historical map, a scene from a book, a cross-section, anything visual. Then you add labelled pins that open popup boxes with your own descriptions when clicked.

**Time:** approximately 30 minutes
**File to edit:** `interactive-image-template.html` — open it in a text editor and a browser side-by-side.

---

## Learning Objectives

**Subject skills**
- Select a meaningful image that represents your topic
- Write clear, accurate descriptions for at least three locations or features

**Digital technology skills**
- Link an image file to an HTML page using a `src` attribute
- Position HTML elements on screen using percentage-based `top` and `left` values
- Edit CSS custom properties to change the visual design

---

## Steps

1. **Choose a topic and find an image.** It can be anything — a labelled body diagram, a battle map, a painting, a food web, a scene from a novel. Save the image file in the same folder as the HTML file.

2. **Open `interactive-image-template.html`** in a text editor and in a browser.

3. **Link your image.** Find the line `src="placeholder.svg"` inside the `<img class="bg-image">` tag. Change it to your image filename, e.g. `src="human-heart.jpg"`. Save and refresh the browser.

4. **Change the title.** Edit the text inside `<header>` near the top of the `<body>`, and the `<title>` tag inside `<head>`.

5. **Reposition the existing buttons.** Each `<button class="pin-btn">` has `style="top: __%; left: __%;"`. Change the percentages to place the pin where you want it on your image. Refresh to check.

6. **Update the button labels.** Change the text between `<button ...>` and `</button>` to match the feature you are labelling.

7. **Update the matching popups.** Each popup `<div class="popup">` has a `<h2>` heading and a `<p>` description. Edit these to match your button.

8. **Add more pins.** Copy a button block and its matching popup block. Give them a new `data-popup` / `id` pair (e.g. `popup4`, `popup5`) and position them.

9. **Aim for at least 5 pins** total.

10. **Customise the look** (optional — see below).

11. **Delete the instructions banner.** Remove the entire `<div class="instructions">` block before submitting.

12. **Test.** Click every pin. Check every popup opens, shows the right content, and closes when you click elsewhere.

---

## Customisation Options

Choose one direction (or combine them):

- **Content-focused:** Keep the default colour scheme. Put your energy into writing detailed, accurate popup descriptions. Add images inside popups using `<img src="photo.jpg" alt="...">`.
- **Design-focused:** Change the CSS variables in `:root` to create a completely different colour palette and font. Make it feel like your topic — e.g. earthy greens for ecology, muted blues for history.
- **Coverage-focused:** Add as many pins as possible — aim for 8 or more, each with a brief label and one-sentence description. Quantity and accuracy over detail.

---

## What a Finished Version Looks Like

- A dark-themed page with a clear title at the top.
- Your chosen image displayed in full, with at least 5 labelled pins placed accurately on it.
- Clicking a pin opens a popup with a heading and a description you wrote.
- Clicking elsewhere (or the ✕ button) closes the popup.
- The instructions banner is gone.
- The colour scheme has been personalised at least slightly.

---

## Teacher Notes

### How to Introduce It

Show the template in a browser on the projector before students open any code. Click a pin to demonstrate the popup. Then open the HTML source and point out the three zones students will edit: the `<img>` tag, the `<button class="pin-btn">` elements, and the `<div class="popup">` elements. Emphasise that every change they make is immediately visible on refresh — this is not abstract coding.

### Differentiation

**Basic support**
- Provide a topic and a pre-selected image so students can skip the research step.
- Ask students to edit only the existing three pins (change labels and popup text) rather than adding new ones.
- Pair students so one navigates the HTML and the other checks the browser.

**Extended challenge**
- Add an image inside at least one popup using `<img src="..." alt="...">`.
- Adjust the popup `top` / `left` values so popups always appear clear of the image edges.
- Add a fourth CSS variable change beyond the defaults (e.g. change `--font-title` to a sans-serif).
- Try adding a `<ul>` bullet list inside a popup body instead of a `<p>` tag.

### Extension Activities

- Add a sixth or seventh pin using the commented-out template block at the bottom of the button and popup sections.
- Change `--page-bg` and the `background` inside `.instructions` to match the image's colour palette, then screenshot the final result for a portfolio piece.

### Assessment Guidance

Look for:
- The student's own image is linked and displays correctly (not the placeholder).
- At least 5 pins are placed at positions that make sense for the image (not just default percentages).
- Popup text is written in the student's own words, not copied verbatim from a source.
- At least two CSS variables have been changed from their defaults.
- The instructions banner has been removed.

### Curriculum Connections

- **Any subject:** works wherever students need to label, annotate, or explain a visual
- **Digital Technologies:** HTML attributes, CSS custom properties, percentage-based layout, event-driven interactivity
- **Literacy:** summarising information into concise popup descriptions
