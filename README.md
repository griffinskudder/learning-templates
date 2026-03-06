# Learning Templates

A collection of self-contained HTML learning activities for high school students. Each template is a **single HTML file** — no build tools, no internet connection, no dependencies. Students open it in a code editor, customise it, and submit or share it.

---

## Templates

| Template | What students make | Time |
|---|---|---|
| [Interactive Image](interactive-image/) | An annotated image with clickable pin buttons that open popup descriptions | ~30 min |
| [Interactive Map](interactive-map/) | A world map with clickable location markers and popup facts | ~30 min |
| [Multiple Choice Quiz](multiple-choice-quiz/) | A self-marking quiz with four-option questions and instant feedback | ~30 min |
| [True or False Checker](true-or-false-checker/) | A set of statements students mark as true or false, with correct/incorrect feedback | ~20 min |
| [Fill in the Blank](fill-in-the-blank/) | Sentences with missing words — students type answers and check their score | ~20 min |
| [Vocabulary Builder](vocabulary-builder/) | A set of flip cards with a word on one side and a definition on the other | ~30 min |
| [Match the Pairs](match-the-pairs/) | A matching activity where students link related terms or concepts | ~30 min |
| [Order the Timeline](order-the-timeline/) | Events that students drag into chronological order | ~30 min |
| [Sorting Activity](sorting-activity/) | Cards that students sort into two categories | ~30 min |
| [Caption the Storyboard](caption-the-storyboard/) | A sequence of panels students write captions for | ~30 min |
| [Bug Hunt Easter Egg](bug-hunt-easter-egg/) | A content page with hidden bug emoji that reveal bonus facts when clicked | ~45 min |

Each template folder contains:
- `*-template.html` — the file students edit
- `*-brief.md` — student task brief and teacher notes
- `examples/` — completed example files for reference (see below)

---

## Examples

Each template includes a completed example in its `examples/` folder based on a **Year 8 Geography — Urbanisation** unit. These cover curriculum content descriptors VC2HG8K18–K22.

| Template | Example file | Content |
|---|---|---|
| [Interactive Image](interactive-image/examples/parts-of-a-city.html) | `parts-of-a-city.html` | 5 clickable pins on a city diagram: City Centre, Suburbs, Factories, Parks, Transport |
| [Interactive Map](interactive-map/examples/cities-australia-usa.html) | `cities-australia-usa.html` | Markers on Sydney, Melbourne, Brisbane, New York, and Los Angeles |
| [Multiple Choice Quiz](multiple-choice-quiz/examples/urbanisation-quiz.html) | `urbanisation-quiz.html` | 5 questions covering key urbanisation concepts |
| [True or False Checker](true-or-false-checker/examples/urbanisation-true-false.html) | `urbanisation-true-false.html` | 5 statements about urbanisation with explanations |
| [Fill in the Blank](fill-in-the-blank/examples/urbanisation-facts.html) | `urbanisation-facts.html` | 5 sentences about cities, jobs, pollution, and the Australian coast |
| [Vocabulary Builder](vocabulary-builder/examples/city-words.html) | `city-words.html` | 6 flip cards: Urbanisation, Migration, Rural, Urban, Population, Sustainability |
| [Match the Pairs](match-the-pairs/examples/urbanisation-vocabulary.html) | `urbanisation-vocabulary.html` | 6 pairs matching urbanisation terms to plain-language meanings |
| [Order the Timeline](order-the-timeline/examples/how-a-city-grows.html) | `how-a-city-grows.html` | 6 steps from small settlement to sustainable city |
| [Sorting Activity](sorting-activity/examples/causes-and-effects.html) | `causes-and-effects.html` | 8 cards sorted into Cause vs Effect of urbanisation |
| [Caption the Storyboard](caption-the-storyboard/examples/how-cities-grow.html) | `how-cities-grow.html` | 4 panels showing the stages of a city growing |
| [Bug Hunt Easter Egg](bug-hunt-easter-egg/examples/urbanisation-usa.html) | `urbanisation-usa.html` | 3 paragraphs about urbanisation in the USA with 3 hidden bonus-fact bugs |

---

## How to Use a Template

1. **Copy the template folder** to a location students can access (shared drive, USB, LMS).
2. **Students open the `.html` file** in a text editor (VS Code, Notepad++, or even Notepad) and in a browser side by side.
3. **Students follow the setup guide** shown as a banner at the top of the page in the browser, then delete it when done.
4. **Students submit** the finished `.html` file (and any image files, if used).

> **Tip:** The instructions banner inside each template tells students exactly what to change. You don't need to hand out a separate guide — though the `*-brief.md` file is there if you want to.

---

## Sharing on Google Sites

You can embed any template directly into a Google Site using an iframe. Because these are self-contained HTML files, they work without any server.

### Steps

1. Upload the finished `.html` file to Google Drive.
2. In Google Drive, right-click the file → **Open with > Google Drive Viewer** to confirm it opens.
3. Get the shareable link: **Share > Anyone with the link > Viewer**.
4. In Google Sites, choose **Insert > Embed** and paste the link.

> **Note:** If the template uses a separate image file (e.g. `src="my-map.jpg"`), the image will **not** load when embedded in Google Sites, because Drive does not serve relative file paths. See **Embedding Images** below to fix this.

---

## Embedding Images (Base64 Encoding)

When a template uses an image file (like the Interactive Image template), the image is linked with a relative path:

```html
<img class="bg-image" src="my-photo.jpg" alt="Background">
```

This works fine when the HTML file and image file are in the **same folder** on a local computer. But when embedded in Google Sites, the image file is unreachable.

**The fix:** encode the image as a Base64 data URI and paste it directly into the `src` attribute. This makes the HTML file completely self-contained — one file, no dependencies.

```html
<img class="bg-image" src="data:image/jpeg;base64,/9j/4AAQ..." alt="Background">
```

### How to Encode an Image

1. Go to **[https://www.base64-image.de/](https://www.base64-image.de/)**.
2. Drag your image file onto the page (or click to browse).
3. Click **"show code"** next to your image.
4. Copy the value inside the `src="..."` attribute — it starts with `data:image/...;base64,`.
5. In the HTML template, replace the existing `src` value with what you copied:

```html
<!-- Before -->
<img class="bg-image" src="my-photo.jpg" alt="Background">

<!-- After -->
<img class="bg-image" src="data:image/jpeg;base64,/9j/4AAQSkZJRgAB..." alt="Background">
```

### File Size Note

Base64 encoding increases file size by roughly 33%. For typical classroom images this is fine. For large photos (over 2 MB), consider resizing the image first — any photo editor will do, or use the free browser-based tool [Squoosh](https://squoosh.app).

---

## Design Conventions

All templates follow the same conventions so students (and teachers) can predict how they work:

- **CSS custom properties in `:root`** — all colours and fonts are variables. Students change one value and see the result immediately.
- **Inline `<style>` and `<script>`** — everything is in one file. No imports, no CDN links.
- **Vanilla JS only** — `var`, `querySelectorAll`, `addEventListener`, `forEach`. No frameworks, no ES6 modules.
- **Instructions banner** — a `<div class="instructions">` block at the top of the page body explains the setup steps. Students delete it when done.
- **Commented template blocks** — every place where students can add items (cards, markers, questions) includes a commented-out copy-paste example.
- **Dark background** — works well on projectors and classroom screens.

---

## Adding a New Template

Use the `agent/learning-task-generator.md` prompt with Claude Code to generate a new template and brief. The generator will ask clarifying questions before producing a complete, consistent template and brief.

Place the output in a new folder following the naming convention:

```
new-template-name/
  new-template-name-template.html
  new-template-name-brief.md
```
