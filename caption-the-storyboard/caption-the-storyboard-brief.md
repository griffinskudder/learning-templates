# Caption the Storyboard — Student Brief

## What You Are Making

A cartoon strip with images and captions. You will arrange images in sequence and write one sentence per panel to explain what is happening. Your finished storyboard could document a science process, a historical event, a geographic change, a story arc — anything that unfolds in steps.

---

## Learning Objectives

**Subject skills**
- Summarise a sequence of events or steps in your own words
- Select images that clearly represent each stage
- Communicate meaning concisely (one sentence per panel)

**Digital technology skills**
- Edit HTML attributes (`src`, text content) in a code editor
- Understand how image paths and URLs work in web pages
- Apply basic CSS variable customisation
- Use a browser to preview and test a local HTML file

---

## What You Need

- A text editor (VS Code, Notepad++, or any plain-text editor)
- A web browser (Chrome, Firefox, Edge)
- 4 images saved to your computer **or** 4 image URLs from the web
- The file `caption-the-storyboard-template.html`

---

## Steps

1. **Open the template.** Double-click `caption-the-storyboard-template.html` to preview it in your browser. Then open the same file in your text editor.

2. **Edit the title.** Find the line that starts with `<h1 class="storyboard-title">` and replace *My Storyboard Title* with your own title.

3. **Add your first image.** Find the comment `<!-- REPLACE src="" -->` above Panel 1. Change `src=""` to `src="your-image.jpg"` (if the image is in the same folder) or paste a full web URL, for example `src="https://example.com/photo.jpg"`. The dotted placeholder box will disappear when the image loads.

4. **Write your first caption.** Find `<p class="caption">` inside Panel 1 and replace the placeholder sentence with your own one-sentence caption.

5. **Repeat for Panels 2, 3, and 4.** Each panel has its own `<img>` tag and `<p class="caption">`.

6. **Save and refresh.** Save the HTML file, then press **F5** in your browser to see your changes.

7. **Customise the look (optional).** Scroll to the `:root { }` block inside `<style>`. Change colour values (e.g. `--accent: #e94560;`) to match your subject or school colours.

8. **Delete the instructions banner.** When you are happy with your storyboard, delete the entire `<div class="instructions">` block from the HTML.

9. **Present to the class (optional).** Click the **Start Presentation Mode** button at the bottom of the page to step through panels one at a time.

---

## Customisation Options

| What to change | Where to find it |
|---|---|
| Storyboard title | `<h1 class="storyboard-title">` |
| Panel images | `src=""` inside each `<img>` tag |
| Caption text | `<p class="caption">` inside each panel |
| Accent / highlight colour | `--accent` in `:root` |
| Panel background colour | `--panel-bg` in `:root` |
| Caption font | `--font-body` in `:root` |
| Page background | `--bg-color` in `:root` |

---

## What a Finished Version Looks Like

- A title at the top naming the sequence or topic
- Four panels, each with a clear image and a single descriptive sentence underneath
- No placeholder boxes visible — all four images loaded successfully
- The instructions banner has been removed
- The colour scheme has been adjusted to suit the topic (optional but encouraged)
- Presentation Mode works smoothly, highlighting each panel in order

---

## Tips

- **Image size:** Images do not need to be the same size — the template crops them automatically to fill the panel.
- **Local images:** If you use files saved to your computer, keep the images in the **same folder** as the HTML file and use just the filename as the `src` (e.g. `src="volcano.jpg"`).
- **One sentence rule:** Captions should be a single, complete sentence. Aim for clarity over length.
- **Sequence matters:** Arrange panels so they tell the story in the correct order — left to right, top to bottom.
