# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

Simple Presence is a single-page static website that displays a centered logo (trashpanda.png) on a gradient background. This is a pure HTML/CSS project with no build process, dependencies, or JavaScript.

## Development

### Running the Site

Open `index.html` directly in a web browser. No server or build process is required.

### File Structure

- `index.html` - Main HTML page
- `style.css` - Styling with flexbox centering and gradient background
- `trashpanda.png` - Logo image asset

## Architecture

The site uses a simple centered layout with flexbox:
- Body element uses `display: flex` with `justify-content: center` and `align-items: center` to center content
- Viewport height set to `100vh` for full-screen vertical centering
- Background uses a linear gradient from darker to lighter gray (`#212121` to `#424242`)
- Logo scales responsively with `max-width: 50%` and `max-height: 50%`
