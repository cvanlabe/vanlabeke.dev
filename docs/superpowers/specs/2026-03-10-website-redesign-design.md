# vanlabeke.dev Website Redesign

## Overview

Replace the placeholder `index.html` with a professional, minimalist personal website for Cedric Van Labeke. The site positions vanlabeke.dev as a person-first presence — Cedric is the lead, vanlabeke.dev is the domain/brand.

## Audience

Both potential consulting clients (CTOs, VPs Eng, founders) and the developer/tech community (peers, open source users, tool users). Equal weight to both.

## Design Decisions

### Visual tone: Adaptive (light + dark)
Uses `prefers-color-scheme` media query. Light mode: white background, dark text, subtle gray borders. Dark mode: near-black background, light text, dark borders. System font stack (`-apple-system`, etc.).

### Structure: Single-page scroll
One `index.html`, no navigation bar. Sections separated by thin horizontal borders. Content is modest — multi-page would feel empty. Tools have their own subpages (`/athan/`, eventually `/viso/`) for detail.

### Future extensibility
The section-based structure makes it trivial to add a "Writing" / blog section later — just another `<section>` block between Tools and Connect, linking out to individual posts.

## Sections (top to bottom)

### 1. Header
- `vanlabeke.dev` as a subtle brand mark (small, lowercase, muted)
- `Cedric Van Labeke` as the page `h1`
- One-liner intro: "Software engineering leader with ~20 years in the industry. I build and scale engineering teams, and I make tools that solve real problems."

### 2. What I Do
- Two short paragraphs. First: leadership positioning (teams, culture, systems at scale, enterprise networking, zero trust, data plane). Second: independent software through vanlabeke.dev.
- No "hire me" or "available for consulting" language. Positions what vanlabeke.dev does, not availability.
- Exact copy for second paragraph: "Through vanlabeke.dev, I also build and publish independent software — tools and apps that I use myself, and that others (hopefully!) find useful too."

### 3. Background
Minimal timeline, narrative not resume:
- **Now**: Director of Software Engineering at Elisity — leading teams across data plane and analytics for an enterprise zero trust platform.
- **Previously**: 15 years at Cisco — engineering management, innovation R&D, and developer tooling that served thousands of engineers.
- **Education**: Master's in Software Engineering

No CCIE number. Cisco mentioned for industry narrative context only.

### 4. Tools & Projects
Card-based portfolio. Each card shows: name, platform badge, one-line description. Cards link to subpages where available.

Current tools:
- **Athan** (macOS) — "Prayer times in your menu bar. Accurate calculations, multiple conventions, and a built-in Book of Prayers." Links to `/athan/`.
- **Viso** (iOS, Android, Google TV) — "Cast video calls to TV screens. Big-screen video calling for families, friends, and students." No link yet.

Easy to add more cards as new tools are built.

### 5. Connect
Two links: LinkedIn (`linkedin.com/in/cevala`) and GitHub (`github.com/cvanlabe`). No email, no contact form.

### 6. Footer
Simple copyright: `© 2024–2026 vanlabeke.dev`

## Technical Details

- Pure HTML + CSS, no build tools, no JavaScript, no frameworks
- Single `index.html` file (replaces current placeholder)
- CSS custom properties for theme switching via `prefers-color-scheme`
- Responsive: mobile breakpoint at 600px (timeline collapses to single column, links stack vertically)
- Max content width: 680px, centered
- Hosted on GitHub Pages (existing setup, CNAME already configured)

## Typography & Spacing

- System font stack: `-apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, 'Helvetica Neue', Arial, sans-serif`
- Section headings: 11px uppercase, letter-spaced, muted color
- Page title: 38px, weight 700, tight letter-spacing
- Body text: 15px, line-height 1.8
- Generous vertical padding between sections (44px)
- Header has extra top padding (72px)

## What's NOT in scope

- No analytics or tracking
- No JavaScript interactions or animations
- No consulting pricing, packages, or CTAs
- No blog/writing section (designed to be easy to add later)
- No favicon (unless one already exists)
