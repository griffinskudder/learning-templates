# Urban Greening — Student Brief

## What You Are Making

An **annotated urban scene** that shows how nature can be added to a city, suburb, or schoolyard. You choose an image — a street photo, a map of your school grounds, a drawing, or even a satellite view — and place labelled icon pins on it. Each pin uses one of four realistic icons (bug, tree, beehive, or flower garden) and opens a popup that explains the benefit of that intervention.

**Time:** approximately 30 minutes
**File to edit:** `urban-greening-template.html` — open it in a text editor and a browser side-by-side.

---

## Learning Objectives

**Subject skills**
- Describe at least four ways nature can be added to an urban space (trees, gardens, habitats for pollinators and insects, beehives, green roofs, etc.)
- Explain the social and ecological benefits of each intervention (cooling, pollination, biodiversity, food production, community wellbeing)
- Match a specific intervention to a specific location — not just "somewhere green"

**Digital technology skills**
- Link an image file to an HTML page using a `src` attribute
- Position HTML elements on screen using percentage-based `top` and `left` values
- Use an SVG icon library: pick an icon for each pin by editing `<use href="#icon-..."/>`
- Edit CSS custom properties to change the visual design

---

## Steps

1. **Choose a space you want to green.** A photo of your street, a drawing of your school grounds, a satellite image of a nearby park, or even a made-up city block. Save the image file in the same folder as the HTML file.

2. **Open `urban-greening-template.html`** in a text editor and in a browser.

3. **Link your image.** Find the line `src="placeholder.svg"` inside the `<img class="bg-image">` tag. Change it to your image filename, e.g. `src="my-street.jpg"`. Save and refresh the browser.

4. **Change the title.** Edit the text inside `<header>` near the top of the `<body>`, and the `<title>` tag inside `<head>`.

5. **Plan your pins.** Decide what four interventions you want to show and where each one goes on your image. Examples: street tree on a bare pavement, community garden on an empty lot, insect hotel on a sunny wall, beehive on a flat rooftop.

6. **Position each pin.** Every `<button class="pin-btn">` has `style="top: __%; left: __%;"`. Change the percentages to place the pin over the right spot on your image. Refresh to check.

7. **Pick the right icon for each pin.** Each pin contains `<use href="#icon-..."/>`. Change the value to one of the four available icons:
   - `#icon-bug` — ladybug (use for insect hotels, bug habitats, beneficial insects)
   - `#icon-tree` — broadleaf tree (street trees, park trees, canopy cover)
   - `#icon-beehive` — woven beehive (apiaries, pollinator habitats)
   - `#icon-garden` — flower planter (community gardens, verges, green roofs, flowerbeds)

8. **Update the pin's `aria-label`.** Change it from e.g. `"Bug location"` to something specific like `"Insect hotel on the school fence"`. This helps screen readers.

9. **Write each popup.** Open the matching `<div class="popup">` and change the `<h2>` heading and the paragraph text. Explain what the intervention is *and* why it matters — e.g. cooling the street, food for pollinators, habitat for birds.

10. **Aim for at least 4 pins** — one of each icon — or **6 pins** for a challenge (you can reuse icons).

11. **Customise the look** (optional — see below).

12. **Delete the instructions banner.** Remove the entire `<div class="instructions">` block before submitting.

13. **Test.** Click every pin. Check every popup opens, shows the right content, and closes when you click elsewhere.

---

## Customisation Options

Choose one direction (or combine them):

- **Content-focused:** Keep the default colour scheme. Put your energy into writing detailed, accurate popup descriptions — include real facts, statistics, or examples from a specific city.
- **Design-focused:** Change the CSS variables in `:root` to match your image. A sunny street might want warm yellows; a rainy schoolyard might want cool blues and greys.
- **Coverage-focused:** Add as many pins as possible — aim for 8 or more — reusing icons as needed (multiple street trees along a road, several bug hotels on different walls). Show what a fully-greened version of the space could look like.
- **Extension-focused:** Add a new icon to the library (see `HOW TO ADD A NEW ICON` inside the file) — e.g. a butterfly, a bird, a vegetable bed.

---

## What a Finished Version Looks Like

- A dark-themed page with a clear title at the top.
- Your chosen image displayed in full, with at least 4 icon pins placed accurately on it.
- Each pin uses a sensible icon for what it represents (a tree icon for a tree, not a bug icon).
- Clicking a pin opens a popup with a heading and a description you wrote in your own words.
- Clicking elsewhere (or the ✕ button) closes the popup.
- The instructions banner is gone.
- The colour scheme has been personalised at least slightly.

---

## Teacher Notes

### How to Introduce It

Start with the example (`examples/greening-a-city-block.html`) on the projector. Click each of the four pins and let students read the popup content aloud. Ask: *"What was here before each intervention? What's better now?"* Then open the template HTML and show students the three zones they will edit: the `<img>` tag, the four `<button class="pin-btn">` elements (pointing out the `<use href="#icon-...">` line inside each), and the matching `<div class="popup">` elements. Emphasise that the icons are reusable — they can put three tree pins along a street, or four bug pins on different walls.

### Differentiation

**Basic support**
- Provide an image (street photo, schoolyard map, or satellite view) so students can skip the research step.
- Ask students to edit only the four existing pins (change icons, positions, and popup text) without adding new ones.
- Pair students so one navigates the HTML and the other checks the browser.

**Extended challenge**
- Add a new icon to the library (copy a `<symbol>` block, redraw shapes, give it a new id) — for example a butterfly, bird, vegetable bed, or rain barrel.
- Add a `<ul>` bullet list inside a popup body listing three species or benefits.
- Write popups that reference real local examples — a named street tree species, a specific community garden, a council policy.
- Adjust the popup `top` / `left` values so popups always appear clear of the image edges.

### Extension Activities

- Produce two versions of the same image: "before" (no pins) and "after" (fully greened), and compare them side-by-side.
- Research a real urban greening project (Singapore's green corridors, Melbourne's urban forest strategy, a local council initiative) and build the pins around its actual interventions.
- Calculate roughly how many trees, gardens, or bug hotels would fit on a real street near the school, using the image as a map.

### Assessment Guidance

Look for:
- The student's own image is linked and displays correctly (not the placeholder).
- At least 4 pins, each using an icon that sensibly matches what it represents.
- Pins are placed at positions that make sense for the image (a tree pin on a pavement, not a rooftop; a beehive pin on a rooftop or garden, not the road).
- Popup text is written in the student's own words and names both *what* the intervention is and *why* it matters.
- At least two CSS variables have been changed from their defaults.
- The instructions banner has been removed.

### Curriculum Connections

- **Geography:** urban environments, sustainability, biodiversity, land use, planning
- **Science:** ecosystems, pollinators, photosynthesis, the urban heat island effect, food webs
- **Digital Technologies:** HTML attributes, CSS custom properties, SVG references (`<use href>`), percentage-based layout, event-driven interactivity
- **Literacy:** summarising complex ecological ideas into concise popup descriptions; writing for a public audience
