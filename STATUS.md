# Project Status — Iksh's Portfolio Site

Last updated: 2026-07-03. This is a paused-work snapshot so a future session (or a future you) can pick this up without re-reading the whole conversation.

## What this project is

A personal portfolio website for Ikshvaku ("Iksh"), a Bay Area high school junior. First-ever coding project — plain HTML/CSS on purpose, no framework, so every line is understandable. Meant to double as something real enough to link in college/internship applications, while still feeling personal and fun, not like a corporate résumé.

Repo: `github.com/ikshsri/my-website`, deployed via GitHub Pages (pushing to `main` triggers a rebuild of the live site).

## Design direction (decided, not open questions)

- **Visual reference**: a dark, premium portfolio hero template (nicknamed "Deneiel" in our notes) — full-width photo, giant name overlapping the photo's bottom edge, floating nav/badge/CTA. See `inspiration/hero-reference-deneiel.png` if present (may need to be added manually — see note at bottom).
- **Color palette**: dark navy/purple background with a lavender accent (Iksh's explicit choice, inspired by Warriors/49ers fandom filtered through a "premium dark" aesthetic). Tokens are in `style.css`'s `:root`.
- **Photo**: Iksh tried to get a real background-removed cutout of his actual photo; the AI tool instead generated a stylized cartoon/illustrated avatar with its own background. After discussion, **he chose to deliberately lean into the illustrated-avatar look** rather than get a real photo cutout. This is a known, accepted tradeoff — not an oversight. File: `images/iksh-avatar.png`. (`images/iksh.jpeg` is his real original photo, unused in the current build but kept in case direction changes later.)
- **Tone**: fun and personal, explicitly *not* "bragging portfolio" — Iksh's words: he doesn't want it to feel like showing off.

## What's built

Single page, three sections in order: Hero → Interests ("A few things I love") → About Me.

The hero is a layered composition (photo + floating UI on top, giant name straddling the photo/background boundary) — see `CLAUDE.md` for the technical structure. It went through several real iterations:
1. First attempt: full-bleed photo, felt generic.
2. Rebuilt as a small centered placeholder box — wrong scale, looked nothing like the reference.
3. Fixed to full-width photo that stops at 76% of the hero height, so the name straddles the boundary — this matched the reference structurally.
4. Real photo (the illustrated avatar) dropped in; had to fix mobile text/photo overlap and a button-sizing bug along the way (both verified with real headless-Chrome screenshots, not assumed).
5. **A design-taste audit pass** (using the `taste-skill` skill installed at `~/.claude/skills/taste-skill`) caught and fixed real "looks AI-generated" issues: two em-dashes in the body copy, a rotated vertical text + scroll-cue arrow (both explicitly-named cliché patterns), emoji used as the entire icon system, and four identical cards in the Interests section (replaced with an asymmetric bento layout, Biology featured as the large card).

## What's NOT done yet (the milestone list)

From the task tracker, still pending:
- **Milestone 4**: Build the Contact section (currently only the hero's "Let's Connect" button exists; no dedicated closing contact section yet).
- **Milestone 5**: Accessibility pass (focus states on all interactive elements, aria-label audit, contrast re-check).
- **Milestone 6**: Motion — entrance fade/stagger on load, scroll-reveal on the Interest cards, and the hero scroll handoff (name slides up, photo fades) as originally planned. Nothing animated yet; this is plain static HTML/CSS so far.
- **Milestone 8**: Final QA pass (Lighthouse/accessibility check in the browser, one more taste-skill pass).

## Known open tensions (flagged, not resolved — Iksh's call)

- The illustrated-avatar photo and the lavender/purple palette both still read as somewhat "generic AI aesthetic" on their own merits, independent of code quality. Iksh knows this and chose to keep both anyway. Worth revisiting if he ever wants to swap in a real photo.
- No icon library is installed (plain inline SVGs are hand-written for the couple of icons in use — menu, arrow). Fine at current scale; would need a real approach if icon usage grows.

## Tooling / environment notes

- Two skill packs live at `~/.claude/skills` (global to this machine, not part of the repo): `taste-skill` (anti-generic-design ruleset, actively used) and a UX/design-review pack (`awesome-ux-skills` + `ui-ux-pro-max-skill`). See `CLAUDE.md`.
- No Playwright/Puppeteer in this repo; screenshots for verification were taken via Chrome's built-in `--headless --screenshot` flag directly. That Chrome binary seems to enforce a viewport-width floor somewhere around 500px when a narrower `--window-size` is requested for screenshots — don't trust a sub-500px screenshot literally; verify the CSS breakpoint logic instead, or test at 600px+.
- If `inspiration/hero-reference-deneiel.png` is missing: the sandboxed shell in this environment could not read pre-existing files directly from the Desktop folder (a macOS permissions quirk, not a code issue) — the file may need to be added manually via Finder.

## Suggested next step when resuming

Pick up at Milestone 4 (Contact section) or Milestone 6 (motion) — both are self-contained and don't depend on anything unresolved.
