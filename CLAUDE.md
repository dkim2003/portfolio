# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Overview

Static portfolio website — three standalone HTML files, no build system, no dependencies, no package manager. All CSS lives inline inside `<style>` tags in each file.

## Development

Serve locally with:
```
python3 -m http.server 8000
```
Then open `http://localhost:8000`. Path-based links (`/blogs`, `/blogs/scie1110`) resolve correctly only when served via HTTP, not via `file://`.

## File structure

```
index.html                  # Main portfolio (About, Experience, Skills, Projects, Contact)
blogs/index.html            # Blog listing page
blogs/scie1110/index.html   # Individual post page (SCIE 1110)
```

## CSS architecture

All three files share the same design system, duplicated inline. When changing any style, apply it to every file that uses it.

**CSS variables (`:root`):**
```css
--bg: #faf9f7      /* page background */
--white: #ffffff   /* card/panel backgrounds */
--dark: #1a1a1a    /* headings, nav logo, footer bg */
--mid: #4a4a4a     /* body text, secondary headings */
--light: #8a8a8a   /* labels, meta, muted text */
--rule: #e0ddd8    /* borders and dividers */
```

**Layout grid:** Most content sections use a two-column grid — a fixed 160px label column and a `1fr` content column (`grid-template-columns: 160px 1fr; gap: 60px`). On mobile (≤900px) this collapses to single-column and the label column is hidden.

**Sticky footer pattern (blog pages only):** `body` is `display: flex; flex-direction: column; min-height: 100vh` and `.section` has `flex: 1`. The main `index.html` has enough content that this isn't needed.

**Typography scale:**
- Page/section labels: `0.68rem`, `letter-spacing: 0.18em`, uppercase
- Nav links: `0.72rem`, `letter-spacing: 0.12em`, uppercase
- Body text: `0.85–0.92rem`
- Section headings: `clamp(1.8rem, 2.5vw, 2.5rem)` (index.html), `clamp(2.5rem, 4vw, 4rem)` (blog pages)

**Scroll behaviour:** `html` has `scroll-behavior: smooth; scrollbar-gutter: stable` — the latter prevents layout shift when the scrollbar appears or disappears.

## Navigation conventions

- `index.html` nav links use fragment-only hrefs (`#about`, `#experience`, etc.) and the logo points to `#`.
- `blogs/index.html` nav links use root-relative hrefs (`/#about`, `/#experience`, etc.) and the logo points to `/`. The active page link gets `class="active"`.
- `blogs/scie1110/index.html` nav follows the same root-relative pattern as `blogs/index.html` but with no active link (the Blogs entry just links back to `/blogs`).

## Adding a new blog post

1. Add a new `<a class="post-card-link" href="/blogs/slug">` card in `blogs/index.html`.
2. Create `blogs/slug/index.html` following the pattern of `blogs/scie1110/index.html` — copy the full file, update the `<title>`, page-header label, and h1.
