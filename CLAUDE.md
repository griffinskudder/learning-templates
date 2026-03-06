# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a collection of standalone HTML learning templates designed for students. Each template is a single self-contained HTML file (no build tools, no dependencies, no internet required). Templates are meant to be copied and customised by students.

## Repository Structure

- Each template lives in its own folder (e.g., `interactive-image-map/`)
- Templates are single-file HTML with inline CSS and JS — no external dependencies
- No build system, package manager, or test framework

## Current Templates

- **interactive-image-map**: A clickable image map where students place pin buttons over a background image. Buttons open positioned popups with descriptive content. Students customise by editing CSS variables (`:root`), adding `<button class="pin-btn">` elements with `top`/`left` percentage positioning, and creating matching `<div class="popup">` blocks.

## Template Design Conventions

- CSS custom properties in `:root` for easy student customisation (colours, fonts)
- Inline `<style>` and `<script>` blocks — everything in one file
- Verbose HTML comments explaining how to add/modify elements (these are instructional)
- Vanilla JS only (`var`, `querySelectorAll`, `addEventListener`) — no ES6 modules or frameworks
- Percentage-based positioning for responsive layout
- Built-in placeholder/fallback content so templates work before student customisation
