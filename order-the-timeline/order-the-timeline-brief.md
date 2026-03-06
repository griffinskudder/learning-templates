# Order the Timeline — Student Brief

## What You Are Making

A web activity that challenges someone to sort a set of events into the correct order. You will replace the placeholder steps with real events from your chosen topic, then share it with a classmate to test.

---

## Learning Objectives

**Subject skills**
- Identify the correct sequence of events or steps in a process from your topic area.
- Explain why order matters (cause and effect, logical dependency, chronology).

**Digital technology skills**
- Edit HTML attributes (`data-order`) to define correct answers.
- Customise the visual style using CSS custom properties (variables).
- Understand how a simple JavaScript quiz mechanic works.

---

## What You Need

- A text editor (VS Code, Notepad++, or similar).
- A web browser to preview your work.
- The file `order-the-timeline-template.html`.

---

## Steps

1. **Choose a topic.** Pick 6 events or steps that have a clear correct order. Examples: stages of the water cycle, events in a historical period, steps of the scientific method, phases of a business launch.

2. **Open the template** in your text editor. Read the yellow instructions banner at the top of the page in the browser, then find the matching comments in the HTML.

3. **Edit the 6 event cards** inside `<ul id="timeline-list">`. For each `<li class="event-card">`:
   - Set **`data-order="N"`** to the correct position of that event (1 = earliest, 6 = latest).
   - Change **`card-title`** to the name of your event.
   - Change **`card-description`** to a short supporting detail (date, location, cause, etc.).

4. **Update the heading** — change the `<h1>` text and the subtitle `<p>` to reflect your topic.

5. **Delete the instructions banner** — remove the entire `<div class="instructions">` block.

6. **Preview in the browser.** The cards will shuffle automatically. Use the Up/Down buttons to reorder them, then press **Check Order** to see your score.

7. **Customise the look (optional).** In the `<style>` block, find the `:root` section marked `CUSTOMIZATION ZONE`. Change the colour variables to suit your theme.

8. **Test with a partner.** Swap files with a classmate and try each other's timelines.

---

## Customisation Options

| What | Where |
|---|---|
| Card titles and descriptions | `card-title` and `card-description` inside each `<li>` |
| Correct position of each card | `data-order` attribute on each `<li>` |
| Page heading and subtitle | `<h1>` and `<p class="subtitle">` in `<header>` |
| Background colour | `--bg-color` in `:root` |
| Card colour | `--card-color` in `:root` |
| Highlight colour (buttons) | `--accent-color` in `:root` |
| Correct / incorrect colours | `--correct-color` and `--incorrect-color` in `:root` |
| Font | `--font-main` in `:root` |

---

## What a Finished Version Looks Like

- A dark-themed web page with a clear heading naming the topic.
- 6 event cards displayed in a shuffled order, each with a title and short description.
- Up and Down buttons that smoothly reorder the cards.
- A **Check Order** button that turns correct cards green and incorrect cards red, and shows a score ("4 of 6 in the correct position").
- A congratulations message that appears when all 6 cards are in the right order.
- A **Reset / Shuffle** button to start again.
- No yellow instructions banner visible (you deleted it).
