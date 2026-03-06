# Bug Hunt Easter Egg — Student Brief

## What You Are Making

A web page about a topic of your choice. Hidden inside the page are three bug emoji icons. Readers who spot and click a bug unlock a **Bonus Fact** popup. You write the content AND hide the bugs.

---

## Learning Objectives

**Subject skills**
- Research and summarise information about a topic into clear paragraphs
- Identify interesting "bonus" facts worth highlighting

**Digital technology skills**
- Edit HTML text content (headings, paragraphs)
- Modify HTML attributes (`data-fact`, `title`)
- Understand how JavaScript reads `data-*` attributes to drive interactivity
- Adjust CSS custom properties to change the visual design

---

## Steps

1. **Open the template file** `bug-hunt-easter-egg-template.html` in a text editor and in a browser side-by-side.

2. **Choose your topic.** It can be anything from your current unit — a historical event, a scientific process, a geographical feature, a literary theme.

3. **Write your content.** Replace the placeholder heading, subtitle, and three paragraphs with your own topic text. Aim for 3–5 sentences per paragraph.

4. **Write your bonus facts.** Each `<span class="bug">` element has a `data-fact="..."` attribute. Replace the placeholder fact text inside the quotes with a real surprising or interesting fact about your topic. Keep each fact to one or two sentences.

5. **Position your bugs.** The three `<span class="bug">` elements can be moved anywhere inside a paragraph. Place them so they are findable but not obvious — mid-sentence is sneakier than the start or end.

6. **Customise the look** (optional). Open the `:root` block near the top of the `<style>` section and adjust colours and fonts. Lower `--bug-opacity` (e.g. `0.15`) to make bugs harder to spot; raise it (e.g. `0.5`) to make them easier.

7. **Delete the instructions banner.** Remove the entire `<div class="instructions">` block from the HTML before you share or submit your page.

8. **Test your page.** Reload in the browser. Click each bug, check the counter increments, and confirm the celebration message appears when all three are found.

---

## Customisation Options

| What to change | Where to find it |
|---|---|
| Page title (browser tab) | `<title>` tag in `<head>` |
| Main heading | `<h1>` inside `.content-card` |
| Subtitle / unit name | `<p class="subtitle">` |
| Section headings | `<h2>` tags |
| Paragraph content | `<p>` tags (keep bugs inside them) |
| Bonus fact text | `data-fact="..."` on each `<span class="bug">` |
| Bug visibility | `--bug-opacity` in `:root` (0 = invisible, 1 = fully visible) |
| Colours | `--bg-color`, `--accent-color`, `--popup-bg`, etc. in `:root` |
| Fonts | `--font-body`, `--font-ui` in `:root` |
| Celebration message | The text inside `<div class="celebration">` |

---

## What a Finished Version Looks Like

- A polished, dark-themed web page with a clear title, subtitle, and three well-written paragraphs on a real topic.
- Three bug emoji hidden within the paragraph text, subtle enough that a reader has to look for them.
- Clicking a bug opens a small popup card with a "Bonus Fact!" heading and a genuine fact that adds to the topic.
- A counter in the top-right corner that tracks how many bugs have been found.
- A congratulations message that appears once all three bugs are found.
- No instructions banner visible.

---

## Teacher Notes

### Introducing the Activity

Show students the finished template in a browser before they open the code. Let them hunt the bugs themselves first — this gives them a clear target to build towards. Then open the HTML source and walk through the three student-editable zones: the content paragraphs, the `data-fact` attributes, and the `:root` variables.

### Differentiation

**Basic support**
- Provide a topic and three pre-written paragraph outlines; students fill in details.
- Allow `--bug-opacity: 0.5` so bugs are easy to locate while editing.
- Pair students to share the research load.

**Extended challenge**
- Add a fourth or fifth bug by duplicating a `<span class="bug" data-fact="...">` element anywhere in the content.
- Add a **timer** that starts when the page loads and stops when all bugs are found — requires students to declare a variable in the script and use `setInterval`.
- Style each bug differently using an additional CSS class (e.g. colour tint) to hint at difficulty level.
- Add a "Hint" button that temporarily raises `--bug-opacity` for 3 seconds using JavaScript `style.setProperty`.

### Assessment Guidance

| Criterion | Look for |
|---|---|
| Content quality | Paragraphs are accurate, in the student's own words, and clearly organised |
| Fact selection | Bonus facts are genuinely interesting and relevant, not just repeated paragraph content |
| HTML editing | Correct attribute syntax; no broken tags; instructions banner removed |
| Design | CSS variables changed meaningfully; bug opacity adjusted intentionally |
| Functionality | All three bugs work; counter increments correctly; celebration appears |

### Curriculum Connections

- **English / Literacy:** summarising, paraphrasing, selecting salient detail
- **Digital Technologies:** data attributes, event listeners, DOM manipulation, CSS custom properties
- **Cross-curricular:** adaptable to any subject area — history, science, geography, literature
