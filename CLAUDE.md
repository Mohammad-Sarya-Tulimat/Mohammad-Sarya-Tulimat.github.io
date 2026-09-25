# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this is

A single-page personal portfolio for Mohammad Sarya Tulimat, served by GitHub Pages from this repo (`Mohammad-Sarya-Tulimat.github.io`, `main` branch). The page content is written by hand from the resume.

- `index.html` is the whole site: inline CSS, inline SVG diagrams, and a small inline script. There's no build step, package manager, framework, linter, or test suite.
- `resume.pdf` is the **source of truth** for the content. When the resume changes, update `index.html` to match. Don't invent claims, metrics, dates, or links that aren't in the resume or already on the page.
- `ICPCId.pdf` and `cert.jpg` are supporting documents (ICPC record, certificate). The page doesn't reference them right now.

## Previewing

Open `index.html` in a browser, or serve the folder locally:

```bash
python -m http.server 8000
```

To deploy, push to `main`. GitHub Pages serves the repo root.

## Structure of index.html

- **Odd head wrapper:** line 1 holds a minimal `<html><head>` with a base `<style>` from an export wrapper. The real `<meta>`, `<title>`, Google Fonts link, and main `<style>` block come after `<body>` opens. Browsers render this fine. If you restructure the head, keep the viewport and safe-area settings.
- **Theming:** colors are CSS custom properties on `:root` (`--ground`, `--surface`, `--ink`/`--ink-2`/`--ink-3`, `--rule`, `--accent`, `--signal`, plus `-soft` variants). Dark mode is defined **twice**, once under `@media (prefers-color-scheme: dark)` guarded by `:root:not([data-theme="light"])` and once under `:root[data-theme="dark"]`. When you change a dark color, update both blocks. Use the tokens, never hardcoded colors.
- **Fonts:** `--display` (Bricolage Grotesque), `--body` (IBM Plex Sans), `--mono` (JetBrains Mono), all from Google Fonts.
- **Sections, top to bottom:** hero (contact chips) → stats row + "at scale" line → Experience (`.role`) → Selected projects (`.proj`; `.proj.feature.has-diagram` gives a two-column card with a diagram) → Skills (`.skill-group`, each with a "used in" line) → Education → ICPC placings → footer. Each `<section>` is a two-column grid with a sticky `h2` on the left, and collapses to one column at 760px or narrower.
- **Diagrams:** the Xbox pipeline and MR40 VPN diagrams are hand-written inline SVGs inside `figure.pipe`. They're styled by class (`.node`, `.node.hot`, `.node.dlq`, `.node.ext`, `.edge`, `.edge.retry`, `.edge.cfg`, `.tun`, `.msg`), and markers are defined in each SVG's own `<defs>`. Marker ids (`ah`, `ahr`, `ah2`, `ahc`) must stay unique across the page. Animated `.msg` dots use `<animateMotion>` and are hidden under `prefers-reduced-motion`. When you edit a diagram, update the `figure`'s `aria-label` to match.
- **Tags:** `.tag` is a tech label. `.tag.m` (amber) is a metric or outcome label.
- **Script:** the only JS copies the `data-copy` value of each contact chip to the clipboard. On success it flashes "copied". If the clipboard fails, it selects the chip's text instead.

Layout must work at phone width (breakpoints at 1080, 900, 760, 640, 600 and 480px) with no horizontal scroll.
