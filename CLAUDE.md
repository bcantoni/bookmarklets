# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

A single-page web app for creating, editing, and managing browser bookmarklets. Hosted on GitHub Pages at https://bcantoni.github.io/bookmarklets/. No build step — open `index.html` directly in a browser to develop and test.

## Architecture

**Single-file application**: All HTML, CSS, and JavaScript live in `index.html` (no separate JS/CSS files). External dependencies loaded via CDN: Highlight.js (syntax highlighting) and js-beautify (code formatting).

**Two-panel layout**: Left panel is a list of saved bookmarklets; right panel is the editor with title, code (syntax-highlighted textarea), output, and action buttons.

**State management**: Three globals drive app state — `bookmarklets` (array), `currentId` (selected bookmarklet UUID or null), `isDirty` (unsaved changes flag). All data persists in `localStorage` under key `'bookmarklets'`.

**Code editor**: Layered approach — a transparent `<textarea>` over a `<pre>` element with Highlight.js rendering. `syncScroll()` keeps them aligned. `updateHighlight()` must be called whenever code content changes programmatically.

**Bookmarklet conversion pipeline**: Minify (strip whitespace/comments) → wrap in IIFE → `encodeURIComponent()` → prepend `javascript:`.

## Key Conventions

- Vanilla JavaScript only — no frameworks, no npm, no build tools.
- The `isDirty` flag must be checked (via `checkUnsavedChanges()`) before any navigation that would discard editor state (new, select, page unload).
- After programmatically setting the code textarea value, always call `updateHighlight()` to sync the syntax highlighting layer.
- The `bm/` directory contains standalone `.js` source files for example bookmarklets.
- SPEC.md contains the full functional specification for the application.
