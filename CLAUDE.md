# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this is

A personal portfolio website for Ikshvaku ("Iksh") — a high school junior, first-ever coding project. Plain static site: `index.html` + `style.css`, no build step, no package manager, no framework, and intentionally so (the goal is for the site owner to understand every line, not hide it behind tooling).

## Running / previewing

There is no build, lint, or test command — this is a static site.

- Open directly: `open index.html` (macOS) or any `file://` URL in a browser.
- Deployed via GitHub Pages: pushing to `main` on the `origin` remote (`github.com/ikshsri/my-website`) triggers a Pages rebuild of the live site.
- To screenshot/verify changes headlessly (no Playwright/Puppeteer installed in this repo), Chrome's built-in headless screenshot mode works: `"/Applications/Google Chrome.app/Contents/MacOS/Google Chrome" --headless --disable-gpu --screenshot=out.png --window-size=1440,900 "file:///path/to/index.html"`. Note: this Chrome binary appears to enforce a viewport width floor somewhere around 500px when `--window-size` requests something narrower — for genuine narrow-mobile verification, request 600px+ and reason about the CSS breakpoint (640px) rather than trusting sub-500px screenshots literally.

## Architecture

Single HTML page, three sections in document order: `.hero`, `.interests`, `.about`. See `STATUS.md` for current progress and design decisions.

**`.hero`** is the most structurally involved part. It's a layered composition, not a normal document-flow layout:
- `.hero` is `position: relative` and is the positioning anchor for every absolutely-positioned child.
- `.portrait-slot` (the photo) is full-width but only ~76% of the hero's height (`76vh` on desktop) — it deliberately does NOT fill the whole hero. That gap is what lets `.giant-name` straddle the photo/background boundary (positioned with `top: 76vh; transform: translate(-50%, -50%)`, so it's centered exactly on that edge).
- z-index stacking (low to high): background vignette (`.hero-bg-glow`, z-index 0) → photo (`.portrait-slot`, z-index 1) → giant name (`.giant-name`, z-index 2) → nav/badge/text/CTA (z-index 5, always on top).
- A separate, simpler mobile layout kicks in at `max-width: 640px`: text elements switch from `position: absolute` to normal flow, and the photo is pushed down (`top: 300px`) to sit *below* the text block instead of layering under it — there's no horizontal room on narrow screens for the two to share space like they do on desktop.

**Design tokens** live in `:root` in `style.css`: a color palette (`--bg`, `--surface`, `--lavender`, `--purple-mid`, `--purple-deep`, `--off-white`), a type scale (`--font-label` through `--font-heading`), and a radius scale (`--radius-md`, `--radius-lg`, `--radius-pill`). New styles should pull from these tokens rather than introducing one-off values.

**CSS section comments are numbered against a component map** (e.g. "Component 6: portrait photo") from an earlier planning pass — the numbering isn't sequential in the file, it maps to a design doc, not file order.

## Known project-specific constraints

- No emoji-as-icons — replaced with inline SVGs (see `.menu-btn`, `.connect-arrow`) after a design-taste audit flagged emoji-heavy UI as looking unfinished. Keep new icons as minimal inline SVGs, not emoji or a hand-rolled icon font.
- No em-dashes anywhere in visible copy (headlines, body text, captions) — a prior pass had to strip these out per project convention; use periods or commas instead.
- Avoid uniform "N identical cards in a row" layouts (see `.interests-grid`'s asymmetric bento layout for the pattern actually in use) — vary card sizes instead of repeating equal-width boxes.
- Two local skill packs are installed at `~/.claude/skills` (global, not part of this repo) that are actively used for this project's design decisions: `taste-skill` (anti-generic-AI-design ruleset — read `~/.claude/skills/taste-skill/SKILL.md` before making visual design calls) and a UX/design-review pack from `awesome-ux-skills` + `ui-ux-pro-max-skill`.
