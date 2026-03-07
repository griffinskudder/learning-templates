# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a collection of standalone HTML learning templates designed for students. Each template is a single self-contained HTML file (no build tools, no dependencies, no internet required). Templates are meant to be copied and customised by students.

## Repository Structure

- Each template lives in its own folder: `<template-name>/`
- Each folder contains `<template-name>-template.html` (the blank template) and an `examples/` subfolder with worked examples
- No build system, package manager, or test framework — open HTML files directly in a browser

## Current Templates

- **bug-hunt-easter-egg**: Students find and fix deliberate code errors hidden in HTML/CSS/JS
- **caption-the-storyboard**: Students write captions for a sequence of storyboard images
- **fill-in-the-blank**: Cloze-style activity with blanks students type answers into
- **interactive-image**: Clickable image with labelled hotspot pins that open popups
- **interactive-map**: Clickable geographic map with location pins and popup descriptions
- **match-the-pairs**: Drag-and-drop or click-to-match vocabulary/concept pairs
- **multiple-choice-quiz**: Self-marking quiz with multiple-choice questions
- **order-the-timeline**: Drag-and-drop chronological ordering activity
- **sorting-activity**: Drag-and-drop sorting into categories (e.g. causes vs effects)
- **true-or-false-checker**: Self-marking true/false statement activity
- **vocabulary-builder**: Definition-matching vocabulary learning activity

## Adding a New Template

1. Create a folder: `<template-name>/`
2. Create the blank template: `<template-name>/<template-name>-template.html`
3. Add a worked example: `<template-name>/examples/<topic-name>.html`
4. Follow all Template Design Conventions below
5. Update the Current Templates list in this file

## Other Directories

- `agent/`: Claude agent definition for generating learning tasks (`learning-task-generator.md`)
- `docs/`: Planning documents

## Template Design Conventions

- CSS custom properties in `:root` for easy student customisation (colours, fonts)
- Inline `<style>` and `<script>` blocks — everything in one file
- Verbose HTML comments explaining how to add/modify elements (these are instructional)
- Vanilla JS only (`var`, `querySelectorAll`, `addEventListener`) — no ES6 modules or frameworks
- Percentage-based positioning for responsive layout
- Built-in placeholder/fallback content so templates work before student customisation
