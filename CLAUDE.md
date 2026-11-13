# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Overview

This is the static landing page for **DST Management Platform (DMP/饥荒管理平台)** — a Don't Starve Together dedicated server management tool. The site is hosted on GitHub Pages and links out to the actual platform repositories.

The platform itself is split across three separate repos:
- **Frontend:** Vue 3 + Vite (`miracleEverywhere/dst-management-platform-web`)
- **Backend API:** Go + Gin (`miracleEverywhere/dst-management-platform-api`)
- **Desktop App:** Electron (`miracleEverywhere/dst-management-platform-desktop`)

## Project structure

```
index.html   # The entire site: HTML markup, inline CSS, and inline JS (~850 lines)
img/         # Static images (favicon, logo, sponsor logos)
```

There is no build step, no package manager, no dependencies. The site is a single `index.html` file with inline `<style>` and `<script>` blocks.

## Running locally

Serve the directory with any static file server:

```bash
python3 -m http.server 8080
# or
npx serve .
```

## Key implementation details

- **Theme (light/dark/system):** The navbar toggle cycles on click: light → dark → follow-system (default). The chosen mode is stored in `data-theme-mode` and drives the sun / moon / monitor icon plus a small status pill; the resolved theme is applied via `data-theme` on `<html>`. All colors are defined as CSS custom properties on `:root` and `[data-theme="light"]`. Preference is persisted in `localStorage` under `theme-mode`, and `system` follows `prefers-color-scheme` live. A tiny inline script in `<head>` applies the saved mode before first paint.
- **Canvas particles:** A floating ember particle system rendered on a `<canvas>` element with mouse repulsion effect. Particle color is driven by the `--particle-rgb` CSS variable.
- **Navbar:** Fixed position with glassmorphism effect (`backdrop-filter`). Transitions to a scrolled state (different glass variables) after 40px scroll.
- **Scroll reveal:** `IntersectionObserver`-based fade-in-up animations on section elements.
- **Feature cards:** Mouse-position-tracked radial gradient glow effect.
- **All styling is inline in `<style>` tags**, using CSS custom properties for theming. The design relies on CSS grid, flexbox, `backdrop-filter`, and CSS animations — no CSS-in-JS or CSS modules.
- **All JS is inline** — one tiny `<script>` in `<head>` for pre-paint theme initialization and the main `<script>` block at the end of the file for everything else. No modules, no transpilation.
