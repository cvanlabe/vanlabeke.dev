# Website Redesign Implementation Plan

> **For agentic workers:** REQUIRED: Use superpowers:subagent-driven-development (if subagents available) or superpowers:executing-plans to implement this plan. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Replace the placeholder `index.html` with a professional, minimalist personal website for Cedric Van Labeke.

**Architecture:** Single static HTML file with embedded CSS. Adaptive light/dark theme via `prefers-color-scheme`. No JavaScript, no build tools, no frameworks. Hosted on GitHub Pages (already configured).

**Tech Stack:** HTML5, CSS3 (custom properties, media queries, grid)

**Spec:** `docs/superpowers/specs/2026-03-10-website-redesign-design.md`

---

## Chunk 1: Implementation

### Task 1: Rewrite index.html

**Files:**
- Modify: `index.html`

**Reference mockup:** `.superpowers/brainstorm/1202-1773139337/layout-clean.html` (approved design)

- [ ] **Step 1: Write the complete index.html**

Replace the placeholder content with the full site. The file must include:

**CSS custom properties (light theme defaults):**
```css
:root {
    --bg: #fafafa;
    --fg: #111;
    --muted: #666;
    --faint: #999;
    --border: #e0e0e0;
    --card-bg: #fff;
}
```

**Dark theme override:**
```css
@media (prefers-color-scheme: dark) {
    :root {
        --bg: #0e0e0e;
        --fg: #e8e8e8;
        --muted: #999;
        --faint: #666;
        --border: #1e1e1e;
        --card-bg: #161616;
    }
}
```

**Layout:**
- `.container` at `max-width: 680px`, centered, `padding: 0 24px`
- `header` with 72px top padding, 56px bottom padding
- `section` elements with 44px vertical padding, separated by `1px solid var(--border)` top borders

**Typography:**
- System font stack: `-apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, 'Helvetica Neue', Arial, sans-serif`
- `.logo`: 13px, weight 600, letter-spacing 0.05em, lowercase, color `var(--faint)`
- `h1`: 38px, weight 700, line-height 1.15, letter-spacing -0.025em
- `h2` (section labels): 11px, weight 600, letter-spacing 0.12em, uppercase, color `var(--faint)`
- Body text: 15px, line-height 1.8
- `.intro`: 17px, color `var(--muted)`, max-width 540px, line-height 1.7

**Sections in order:**

1. **Header**: `vanlabeke.dev` logo mark, `Cedric Van Labeke` as h1, intro paragraph
2. **What I Do**: Two paragraphs of positioning text (exact copy from spec)
3. **Background**: Timeline grid — label column (140px) + text column. Three items: Now (Elisity), Previously (Cisco), Education
4. **Tools & Projects**: Card grid. Each card: `var(--card-bg)` background, `1px solid var(--border)`, 8px border-radius. Card contains: header row (name + platform), description. Athan card links to `/athan/`. Viso card has no link (use `<div>` not `<a>`)
5. **Connect**: Flex row with LinkedIn (`https://linkedin.com/in/cevala`) and GitHub (`https://github.com/cvanlabe`) links. Underline via `border-bottom: 1px solid var(--border)`, hover: `border-color: var(--fg)`
6. **Footer**: `© 2024–2026 vanlabeke.dev`, 12px, color `var(--faint)`

**Responsive (max-width: 600px):**
- Header padding: 48px top, 36px bottom
- h1: 28px
- `.intro`: 15px
- Timeline: single column (no label column)
- Connect links: stack vertically, gap 14px
- Tool cards: already single column, no change needed

**Meta tags:**
```html
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<meta name="description" content="Cedric Van Labeke — software engineering leader and independent tool maker.">
<title>Cedric Van Labeke — vanlabeke.dev</title>
```

- [ ] **Step 2: Visual verification**

Open `index.html` in a browser and verify:
- Light mode renders correctly (use system light mode or browser override)
- Dark mode renders correctly (use system dark mode or browser override)
- Mobile layout works (resize browser to < 600px width)
- Athan card links to `/athan/`
- LinkedIn and GitHub links point to correct URLs
- All text matches the spec copy exactly

- [ ] **Step 3: Commit**

```bash
git add index.html
git commit -m "Redesign homepage with professional single-page layout

Replaces placeholder with adaptive light/dark personal site featuring
bio, background, tools portfolio (Athan, Viso), and connect links."
```
