# Agent Guidelines for Simple Presence

## Project Overview
Static single-page site: `index.html` + `style.css` + `trashpanda.png` logo centered on gradient background.

## Development Commands
- `open index.html` — quick browser preview
- `python3 -m http.server 4173` — local dev server
- `npx serve . --listen 4173` — alternative static server
- `npx prettier --write index.html style.css` — format before committing

## Code Style
- **HTML**: Semantic tags, 2-space indentation, attributes single-line unless >80 chars
- **CSS**: 4-space indentation, group declarations (layout/typography/color), descriptive class names (`.hero-logo` not `.img1`)
- **Assets**: New media in `assets/`, use relative paths, optimize images ≤1 MB

## Testing
- No automated tests; manually verify in Chrome + Safari/WebKit
- Check responsive behavior down to 320px width
- Capture before/after screenshots for visual changes

## Commits & PRs
- Commit format: concise subject + body with `Tasks:` heading listing changes
- Keep commits single-purpose, reference issues (`Closes #12`)
- PRs need test steps + screenshots for style/asset changes
