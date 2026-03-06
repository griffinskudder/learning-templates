# Interactive Map — Student Brief

## Overview

In this activity you will create an **interactive world map** that lets viewers click location markers to read short explanations. You can use this for any topic that has a geographical component — volcano locations, explorer routes, historical events, trading routes, animal habitats, and more.

**Time:** approximately 30 minutes (1 period)
**File to edit:** `interactive-map-template.html` — open it in a text editor and a browser side-by-side.

---

## What the template gives you

- A full-width map area with simplified continent outlines, all built from HTML and CSS — no internet connection needed.
- Four example markers already placed on the map (Sydney, Rio de Janeiro, London, Nairobi).
- A pulsing animation on each marker to draw the viewer's eye.
- A popup that appears when a marker is clicked, showing a title and a short description.

---

## Your task

1. **Choose a topic.** Examples:
   - Locations of major volcanic eruptions
   - Ports visited by a historical explorer
   - Cities affected by a historical event
   - Countries where a particular species lives

2. **Change the page title** (`<h1>`) and subtitle to match your topic.

3. **Replace the four placeholder markers** with markers relevant to your topic. For each location you need:
   - A `top` percentage (distance from the top of the map, 0 % = top edge, 100 % = bottom edge)
   - A `left` percentage (distance from the left of the map, 0 % = left edge, 100 % = right edge)
   - A `data-title` — the name of the place
   - A `data-desc` — one or two sentences explaining why this place is significant to your topic

4. **Add at least 6 markers** in total (you can keep or replace the placeholders).

5. **Customise the colours** using the CSS variables in the `:root` block if you want a different visual theme.

6. **Delete the instructions banner** (the `<div class="instructions">` block) before submitting.

---

## How to position a marker

Each marker is a single HTML line inside the `.map-area` div:

```html
<div class="marker"
     style="top: 48%; left: 51%;"
     data-title="Your Place Name"
     data-desc="One or two sentences about why this place matters.">
</div>
```

Change `top` and `left` to move the dot. The values are percentages of the total map height and width respectively.

**Finding the right percentages:**
1. Open the HTML file in your browser.
2. Right-click on the map and choose **Inspect** (or press F12).
3. Hover your mouse over the map — the browser shows the pixel coordinates.
4. Divide the X pixel coordinate by the total map width to get `left`, and the Y coordinate by the total map height to get `top`. Multiply by 100 to convert to a percentage.

---

## Customisation options (CSS variables)

| Variable | What it changes |
|---|---|
| `--ocean-color` | Background colour of the map (the "sea") |
| `--land-color` | Fill colour of the continent shapes |
| `--marker-color` | Colour of the circular marker dots |
| `--popup-bg` | Background colour of the info popup |
| `--popup-title-color` | Colour of the popup heading text |
| `--font-family` | Font used across the whole page |

---

## Marking checklist

- [ ] Page title and subtitle match your chosen topic
- [ ] At least 6 markers placed at geographically correct positions
- [ ] Every marker has a meaningful title and a clear 1–2 sentence description
- [ ] The instructions banner has been removed
- [ ] The file opens and works fully offline (no internet required)
- [ ] Colours or font have been personalised in the `:root` block
