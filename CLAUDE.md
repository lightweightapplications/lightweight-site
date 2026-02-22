# CLAUDE.md — Lightweight Applications Site

This file provides context and conventions for AI assistants working on this repository.

## Project Overview

This is the marketing/product website for **Lightweight Applications**, a company building efficient Shopify apps. The site is hosted at `lightweightapplications.online` via GitHub Pages.

The site intentionally mirrors the company's philosophy: minimal, performant, zero unnecessary dependencies.

## Architecture

The entire site is a **single HTML file** (`index.html`) — 8.6 KB, no build step, no external dependencies.

```
lightweight-site/
├── index.html    # Complete website: HTML + embedded CSS + embedded JS
└── CNAME         # GitHub Pages custom domain: lightweightapplications.online
```

There is no `package.json`, no build system, no transpiler, no CDN links, no npm packages. This is intentional and must be preserved.

## Tech Stack

- **HTML5** — semantic markup
- **CSS3** — Flexbox, media queries, CSS custom properties (variables), transitions
- **Vanilla JavaScript ES6+** — client-side hash routing, DOM manipulation
- **Zero external dependencies** — no libraries, no frameworks, no CDN assets

## Client-Side Routing

Navigation is handled by a simple hash-based router in the `<script>` block at the bottom of `index.html` (lines 263–282).

### Routes

| Hash | Section shown | DOM element |
|------|--------------|-------------|
| `#` (empty / default) | Home | `#home-section` |
| `#cs-shield` | CS Shield product page | `#cs-shield-section` |
| `#cs-shield/privacy-policy` | Privacy Policy | `#privacy-policy-section` |

### How routing works

- `handleRoute()` reads `window.location.hash` and calls `showSection(name)`.
- `showSection(name)` toggles `display` on the three container `<div>`s.
- Listeners are attached to `hashchange` and `DOMContentLoaded`.

### Adding a new page/route

1. Add a new `<div class="container" id="my-section" style="display:none">` inside `<main id="main-content">`.
2. Add a new `if` branch in `handleRoute()` mapping a hash to `showSection('my-section')`.
3. Add `document.getElementById('my-section').style.display = section === 'my-section' ? '' : 'none';` inside `showSection()`.
4. Add navigation links as needed in the `<nav>` block.

## CSS Architecture

All styles are in a single `<style>` block in `<head>` (lines 8–199).

### CSS Custom Properties (theme variables)

Defined on `:root`:

| Variable | Value | Usage |
|----------|-------|-------|
| `--primary` | `#2563eb` | Brand blue, links, logo |
| `--primary-dark` | `#1e40af` | Darker blue for hover states |
| `--text` | `#1f2937` | Primary body text |
| `--text-light` | `#6b7280` | Secondary/muted text |
| `--bg` | `#ffffff` | Page background |
| `--bg-alt` | `#f9fafb` | Alternate background (hover, cards) |
| `--border` | `#e5e7eb` | Borders and dividers |

Always use these variables instead of hardcoded colors.

### Responsive breakpoint

One breakpoint at `768px` (tablet/mobile). Mobile layout collapses the header to column layout, full-width nav links, and dropdown menus become static (no absolute positioning).

### Typography

System font stack: `-apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, 'Helvetica Neue', Arial, sans-serif`

No web fonts — this is intentional for performance.

## Navigation / Dropdown Pattern

The nav uses CSS-only hover dropdowns (no JavaScript). On desktop, `.dropdown` children are `position: absolute` with opacity/visibility transitions. On mobile, they collapse inline via `max-height` transition.

## Development Workflow

No build, no install, no tooling required.

**To make changes:**
1. Edit `index.html` directly.
2. Open it in a browser to verify (file:// works).
3. Commit and push — GitHub Pages deploys automatically on push to `main`.

**To preview locally:**
```sh
# Any of these work:
open index.html
python3 -m http.server 8080
npx serve .
```

## Deployment

- **Platform:** GitHub Pages
- **Domain:** `lightweightapplications.online` (configured via `CNAME` file)
- **Trigger:** Automatic on push to `main` branch
- **No CI/CD pipelines** are configured

## Key Constraints and Conventions

These must be maintained when making changes:

1. **No external dependencies.** Do not add CDN links, npm packages, or third-party scripts.
2. **Single file.** Keep everything in `index.html`. Do not split into separate CSS/JS files unless the project significantly grows in complexity.
3. **No build step.** The site must remain deployable by opening the HTML file directly.
4. **Use CSS variables for all colors.** Never hardcode hex/rgb values in new CSS rules.
5. **Preserve privacy policy content accuracy.** The CS Shield privacy policy text has legal significance — confirm with stakeholders before changing it.
6. **Mobile-first.** All new styles should work on mobile; use the 768px breakpoint for desktop enhancements.
7. **Semantic HTML.** Use appropriate elements (`<section>`, `<nav>`, `<header>`, `<footer>`, `<main>`).
8. **Accessibility.** Maintain `lang="en"` on `<html>`, descriptive `<meta>` tags, and proper heading hierarchy.

## Current Pages / Products

### Home (`#`)
Landing page with company name and tagline: "Creating efficient, performant apps across multiple marketplaces."

### CS Shield (`#cs-shield`)
A Shopify app for processing contact form submissions. Privacy-first: stores only an anonymous submission count, no PII.

### CS Shield Privacy Policy (`#cs-shield/privacy-policy`)
Legal privacy policy. CS Shield does not store names, email addresses, message content, or any PII — only an aggregate submission count.

## Git Branch Conventions

- `main` / `master` — production branch, auto-deploys to GitHub Pages
- `claude/<description>-<session-id>` — AI assistant working branches

## Commit Message Style

Commits in this repo use lowercase, imperative short messages (no period):
- `commit initial`
- `updated with privacy policy`
- `Update CNAME`
