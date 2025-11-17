# Repository Guidelines

## Project Structure & Module Organization
- `index.html` renders the single-page layout; keep new sections modular by grouping related markup in self-contained `<section>` blocks.
- `style.css` holds all styling; add new rules near related selectors and prefer shared utility classes for repeated patterns.
- `trashpanda.png` is the primary asset; place new media under an `assets/` directory and reference it with relative paths so the static server picks it up.

## Build, Test, and Development Commands
- `open index.html` — fast manual preview using the system browser.
- `python3 -m http.server 4173` — serves the site locally, enabling relative asset paths and easy testing on other devices via LAN.
- `npx serve . --listen 4173` — alternative static server when you need consistent caching headers; installable via npm without project-level deps.

## Coding Style & Naming Conventions
- Follow semantic HTML, two-space indentation inside elements, and keep attributes on one line until they exceed ~80 chars.
- CSS currently uses four-space indentation; match that style and group related declarations (layout, typography, color).
- Prefer descriptive class names (`.hero-logo`) over positional ones, and consolidate shared values into custom properties when repetition appears.
- Run `npx prettier --write index.html style.css` before committing to ensure consistent whitespace.

## Testing Guidelines
- Smoke-test in at least one Chromium- and one WebKit-based browser; confirm the gradient background and centered logo render identically.
- When adding responsive tweaks, use browser devtools to verify breakpoint behavior down to 320px width and document any limitations in the PR.
- No automated tests exist; capture before/after screenshots for visual changes to aid reviewers.

## Commit & Pull Request Guidelines
- Commits must include a concise subject plus a body with a `Tasks:` heading followed by Markdown bullets enumerating what changed and any verification.
- Reference related issues in the body (`Closes #12`) and keep commits scoped to a single concern.
- Pull requests should summarize intent, list manual test steps, and attach updated screenshots whenever styles or assets change.
- Request review once CI (if any) and manual checks pass, and note any follow-up work explicitly.

## Security & Configuration Tips
- Do not commit API keys or service credentials; this repo is entirely static.
- Optimize images (≤1 MB) and use relative paths so deployments remain portable across hosting targets.
