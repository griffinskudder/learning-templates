# Urban Greening Template — Design

## Overview

A new standalone HTML learning template for the `learning-templates` repo. It is a variant of `interactive-image` in which the pin buttons are replaced with detailed, realistic SVG icons themed around urban greening: a bug (ladybug), a tree, a beehive, and a garden. Students place these icon pins on a background image of their choice; clicking a pin opens a popup with further information.

The template ships with four starter pins — one of each icon — and a worked example showing a "Greening a City Block" scene.

## Goals

- Give students a visually richer alternative to `interactive-image` for topics suited to illustrated icon pins.
- Keep the single-file, no-build, no-dependencies, no-internet constraints of the rest of the repo.
- Reuse the existing `interactive-image` popup mechanism unchanged so the template feels familiar.
- Make the icon set reusable across pins and (optionally) extensible by adding new `<symbol>` blocks.

## Non-goals

- Supporting arbitrary user-uploaded icons at runtime — the palette is defined in HTML at author time.
- Full visual parity with `interactive-image` (e.g., the decorative grid overlay is removed because it clashes with photographic backgrounds and detailed illustrations).
- Runtime icon color theming via CSS variables — the illustrations carry their own realistic palette.

## File structure

```
urban-greening/
├── urban-greening-template.html              # blank template
└── examples/
    └── greening-a-city-block.html            # worked example
```

Follows the project convention `<template-name>/<template-name>-template.html` plus an `examples/` subfolder.

## Icon library

Four SVG icons defined once inside a hidden `<svg>` near the top of `<body>`, each as a `<symbol>` with `viewBox="0 0 64 64"`:

| `id` | Depicts |
|---|---|
| `icon-bug` | Ladybug — red shell with black spots, visible legs and antennae |
| `icon-tree` | Broadleaf tree — brown trunk, layered green canopy with shading |
| `icon-beehive` | Woven skep hive — amber/brown ridged dome, small entrance, one or two bees nearby |
| `icon-garden` | Flowerbed / planter — soil base with several colored flowers on green stems |

**Style:** detailed multi-color illustrations (multiple `<path>`, `<circle>`, `<ellipse>` elements with explicit fill colors). Colors are baked into the `<symbol>` and are *not* driven by CSS variables — this preserves the "realistic" aesthetic regardless of theme.

**Extension:** students may add new icons by copying a `<symbol>` block and giving it a new `id`. This is documented as an optional advanced step in the instructions banner.

A verbose HTML comment above the library explains:

> These are the 4 available icons. To use one on a pin, reference it with `<use href="#icon-...">`. To add a new icon, copy an existing `<symbol>` and give it a new `id`.

## Pin rendering and interaction

Each pin is a `<button>` containing only an SVG `<use>` reference — no background box, no text label, no pin tail.

**Markup:**
```html
<button class="pin-btn"
        data-popup="popup1"
        aria-label="Ladybug habitat"
        style="top: 35%; left: 28%;">
  <svg class="pin-icon" viewBox="0 0 64 64" aria-hidden="true">
    <use href="#icon-bug"/>
  </svg>
</button>
```

**Styling:**
- `.pin-btn` — transparent background, no border, no padding. `position: absolute` with `transform: translate(-50%, -50%)` so the coordinate targets the icon's center.
- `.pin-icon` — sized via a `--icon-size` CSS variable (default 56px) so all pins scale together. `filter: drop-shadow(0 2px 4px rgba(0,0,0,.6))` keeps icons visible on any background.
- `:hover` — scale up to ~1.15 with a stronger drop-shadow, via `transition`.
- `:focus-visible` — outline ring for keyboard navigation (the existing `interactive-image` lacks this; added here because there is no visible text on the pin).

**Accessibility:**
- Every pin requires an `aria-label` — the sample pins ship with sensible defaults.
- The inner `<svg>` carries `aria-hidden="true"` so screen readers announce the button label rather than SVG internals.

**Click behavior:** unchanged from `interactive-image`. Clicking a pin toggles its matching popup; clicking the close button or anywhere outside a popup closes it. Only one popup is open at a time. The JS is the same vanilla script.

**Blank-template starter content:** the four sample pins use placeholder labels and popup bodies that name their icon (e.g., aria-label `"Bug location"`, popup title `"Bug"`, popup body `"Write your description of the bug location here..."`). This mirrors `interactive-image`'s "Location A/B/C" pattern but makes the icon–pin relationship visible to the student reading the file.

## Popups

No changes from `interactive-image`. Same structure (`.popup` → `.popup-header` + `.popup-body`), same positioning via inline `top`/`left` percentages, same `.active` toggle, same pop-in animation, same dark-background / accent-bordered visual style. This preserves continuity between templates.

## Background image and page shell

- Same `<img class="bg-image" src="placeholder.svg" onerror="this.src=this.getAttribute('data-fallback')" data-fallback="...">` pattern as `interactive-image`, with an inline-SVG fallback shown when no image is provided.
- The decorative `.image-wrap::before` grid overlay from `interactive-image` is **removed** — it distracts from detailed illustrated icons and clashes with photographic urban scenes.
- Header, footer, page background, and CSS-variable customization zone follow the `interactive-image` conventions.

## Instructions banner

Student-facing steps inside the template:

1. **Background image** — replace `src="placeholder.svg"` on the `<img class="bg-image">` tag with your own image filename.
2. **Add a pin** — copy a `<button class="pin-btn">` block, change `data-popup` to a unique value, set `top`/`left` percentages to place it on the image, pick an icon via `<use href="#icon-...">` (choose from `#icon-bug`, `#icon-tree`, `#icon-beehive`, `#icon-garden`), and update the `aria-label`.
3. **Add a popup** — copy a `<div class="popup">` block, match its `id` to the button's `data-popup`, and fill in the title and body.
4. **Add a new icon (optional, advanced)** — copy a `<symbol>` block in the icon library and give it a unique `id`.
5. **Change colours & fonts** — edit the CSS variables in `:root`. Note that icon colors are defined inside each `<symbol>`, not as CSS variables.
6. **Delete this instructions box** when done.

## Worked example: "Greening a City Block"

`urban-greening/examples/greening-a-city-block.html`

A fictional urban block with four interventions, one using each icon:

| Icon | Location on background | Popup topic |
|---|---|---|
| `#icon-garden` | Vacant lot between buildings | Community garden — converting unused lots into food-producing community spaces |
| `#icon-tree` | Sidewalk strip | Street trees — canopy cover for cooling, air filtration, and stormwater management |
| `#icon-beehive` | Flat rooftop | Rooftop apiary — urban beekeeping, pollination, and local honey |
| `#icon-bug` | Park edge / building wall | Insect hotel — stacked-wood habitat for solitary bees, ladybugs, and other pollinators |

**Background:** inline-SVG fallback of a simplified street scene (buildings, pavement, vacant lot, flat rooftop) drawn in flat colors — so the example works without shipping a binary image.

**Popup content:** each popup carries 2–3 sentences of real ecological/social content so the example reads as a finished student project rather than a lorem-ipsum mockup.

**Theming:** a custom green accent color in `:root` to demonstrate how the CSS variables work.

## Out of scope

- Drag-to-reposition pins in the browser (authoring uses inline `top`/`left` percentages).
- Per-pin icon switching at runtime (icon is set in HTML).
- Additional icon sets for other topics (this template is themed for urban greening; other themes would be separate templates).

## CLAUDE.md update

After the template is built, add `urban-greening` to the "Current Templates" list in `CLAUDE.md` with a one-line description.
