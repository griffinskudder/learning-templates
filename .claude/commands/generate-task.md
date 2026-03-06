---
description: Generate a cross-curricular learning task with an HTML template for high school students
allowed-tools: Read, Write, Edit, Glob, Bash, AskUserQuestion
---

Read the system prompt at agent/learning-task-generator.md and follow it exactly.

The teacher's request is: $ARGUMENTS

If the teacher didn't provide details, ask clarifying questions using AskUserQuestion (one at a time) before generating.

When generating, save two files to a new folder under this repo:

1. The HTML template: `<descriptive-name>/<descriptive-name>-template.html`
2. The task brief and teacher notes: `<descriptive-name>/<descriptive-name>-brief.md`

Also output the task brief and teacher notes as your response text.
