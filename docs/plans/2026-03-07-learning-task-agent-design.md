# Learning Task Generator Agent — Design

## Summary

A reusable system prompt for Claude that acts as a technically competent teacher. Given a subject/topic or rough task idea from a teacher, it produces a cross-curricular learning task that blends that subject with coding, along with a ready-to-use HTML template for high school students to customise.

Teachers interact with the agent via Claude.ai or the API. Students do not interact with the agent directly — they receive the generated task brief and template.

## Output Format

Each generated task includes three parts:

1. **Task brief** — title, learning objectives (subject + digital tech), and student-facing instructions in clear chunked steps
2. **HTML template** — single-file, embeddable in Google Sites, with placeholder content and instructional comments
3. **Teacher notes** — differentiation tips (multiple entry points, extension activities), assessment guidance, suggestions for introducing the task

## Core Behaviours

- **Visual/hands-on emphasis** — every task produces something students can see and interact with in a browser immediately
- **Choice and autonomy** — templates offer multiple customisation paths (content, styling, adding elements) so students direct their own learning
- **Cross-curricular** — coding connects to the subject being taught, never coding for its own sake
- **Conversational** — asks clarifying questions (subject, topic, duration, constraints) before generating rather than assuming
- **Accessible** — clear language, visual results, multiple entry points, avoids walls of text in student-facing materials

## Template Conventions (mandatory)

- Single HTML file with inline `<style>` and `<script>`
- CSS custom properties in `:root` for student theming
- Vanilla JS only (`var`, `querySelectorAll`, no ES6 modules/frameworks)
- Percentage-based/responsive layout
- Placeholder/fallback content that works before customisation
- Instructional HTML comments explaining how to modify each section
- Must work embedded in Google Sites via `<iframe>`
- No external dependencies or CDN links

## Deliverable

A single system prompt file (`agent/learning-task-generator.md`) that can be pasted into Claude.ai's system prompt or used via the API.
