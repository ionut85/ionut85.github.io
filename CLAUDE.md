# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

**ionut85.github.io** is a static, single-page personal link portfolio (Bento-style) built as a self-hosted alternative to Bento.me. The site is deployed via GitHub Pages and contains a curated collection of professional links, projects, and achievements.

**Live Site:** https://ionut85.github.io

## Key Files

- **index.html** - Main production file (654 lines). All CSS inlined, zero JavaScript. Fixed-dimension card layout.
  - Path: `/Users/ionutciobotaru/Documents/GitHub/ionut85.github.io/index.html`
- **DEVELOPMENT_LOG.md** - Complete development history, workflows, and decisions.
  - Path: `/Users/ionutciobotaru/Documents/GitHub/ionut85.github.io/DEVELOPMENT_LOG.md`
- **index-simple.html** - Alternative simpler version used as reference/backup.
- **bento_source.html** - Original Bento.me export (12,511 lines). Reference archive only.
- **links.json** - Data source for all card content with structured JSON. Not currently used for dynamic rendering.
- **assets/images/** - 139 image files (12MB) including icons, previews, and profile photo.
  - Path: `/Users/ionutciobotaru/Documents/GitHub/ionut85.github.io/assets/images/`
  - **image-mapping.json** - Maps Bento URLs to local filenames for traceability

## Architecture

**Type:** Zero-JavaScript static site
**Styling:** Inline CSS (no external stylesheets)
**Deployment:** GitHub Pages (automatic via `master` branch)
**Framework:** Pure HTML + CSS Grid

### Layout System

The site uses CSS Grid with fixed-dimension cards that reflow based on screen width:

- **Desktop (>= 900px):** Two-column layout (320px sidebar + 1fr content). Card grid: 4 columns (189px each) centered. Sticky profile sidebar.
- **Tablet (640px - 899px):** Full-width single column layout. Card grid: 2 columns (189px each) centered.
- **Mobile (< 640px):** Full-width single column. Card grid: auto-fit 189px columns, centered.

**IMPORTANT:** Cards maintain fixed pixel dimensions at all screen sizes. They do NOT resize - only their grid positioning changes. This creates a Bento-like reflow behavior where cards stack vertically as the screen narrows.

### Card System

Three card variants with exact dimensions:

1. **card--small** (default, 1 column)
   - Dimensions: 189px × 175px (fixed)
   - Padding: 24px all sides
   - Icon: 40x40px positioned on top
   - Layout: Icon + Title + Domain (vertical stack)

2. **card--wide** (2 columns span)
   - Dimensions: 390px × 175px (fixed)
   - Padding: 24px all sides
   - Layout: Icon/Title/Domain on left (in .card__body wrapper), preview image (178px × 127px) on right
   - Preview images have rounded corners (border-radius: 10px)
   - Icon positioned ON TOP of title/domain (not inline)
   - **Does NOT reflow on mobile** - maintains horizontal layout

3. **card--compact** (2 columns span)
   - Dimensions: 390px × 67.5px (fixed)
   - Padding: 20px top/bottom, 24px left/right
   - Horizontal layout with icon (40x40px, same size as other cards)
   - Domain hidden (.card__domain { display: none; })

4. **card--background** (special modifier for Open Internet card)
   - Full background image spanning entire 390px × 175px card
   - Hides .card__body and .card__image elements
   - Applied via inline style: `style="background-image: url('...')"`

### CSS Grid Calculation

- Grid columns: 189px each with 12px gap
- Wide/compact cards: span 2 columns = 189px + 12px + 189px = 390px total width
- Mobile grid: `repeat(auto-fit, 189px)` with `justify-content: center`
- All grids use `justify-content: center` to center content

### Image Specifications

**Icons (all card types):**
- Size: 40px × 40px (consistent across small, wide, and compact cards)
- Border-radius: 10px
- Padding: 8px
- Background: var(--bg-color)
- Format: SVG preferred, PNG/JPG acceptable

**Preview images (wide cards only):**
- Size: 178px × 127px
- Border-radius: 10px
- Object-fit: cover
- Position: Right side of card
- Format: PNG, JPG, or JPEG

**Naming convention:**
- Icons: `{project-name}-icon.{ext}` (e.g., `hypd-icon.png`)
- Previews: `{project-name}-preview.{ext}` (e.g., `hypd-preview.png`)
- Screenshots: `{project-name}-screenshot.{ext}` (e.g., `noah-screenshot.jpeg`)

## Content Sections

The site contains 49 link cards organized into 6 sections:

1. **Entrepreneuring** (4 cards) - Current projects and ventures
2. **Blogging, Marketing, Developing** (6 cards) - Social media and platforms
3. **Media** (8 cards) - Press mentions and speaking appearances
4. **Events & Memberships** (5 cards) - Professional affiliations
5. **Investments** (14 cards) - Portfolio companies
6. **Past Projects** (12 cards) - Historical ventures

## Build & Deployment

**There is no build step.** The site is entirely static.

### Deployment Workflow

```bash
# Make changes locally
git add .
git commit -m "Your changes"
git push origin master

# GitHub Pages automatically deploys from master branch
# Site is live at https://ionut85.github.io within 1-2 minutes
```

**GitHub Pages Configuration:**
- Repository: `ionut85/ionut85.github.io`
- Branch: `master`
- Serves `index.html` directly as-is

## How to Update Content

### Edit Links

1. Open `index.html`
2. Find the relevant section (search for section title)
3. Edit the `<a class="card card--{size}">` block
4. Update href, title, domain, and image src as needed
5. Test in browser: `open -a "Google Chrome" index.html`
6. Commit and push

### Add New Card

Copy an existing card structure and modify:

```html
<!-- Small card example -->
<a href="https://example.com" class="card card--small" target="_blank" rel="noopener">
    <img src="assets/images/icon.png" alt="" class="card__icon" loading="lazy">
    <h3 class="card__title">Title</h3>
    <span class="card__domain">example.com</span>
</a>

<!-- Wide card example -->
<a href="https://example.com" class="card card--wide" target="_blank" rel="noopener">
    <div class="card__body">
        <img src="assets/images/icon.png" alt="" class="card__icon" loading="lazy">
        <h3 class="card__title">Title</h3>
        <span class="card__domain">example.com</span>
    </div>
    <img src="assets/images/preview.png" alt="" class="card__image" loading="lazy">
</a>

<!-- Compact card example -->
<a href="https://example.com" class="card card--compact" target="_blank" rel="noopener">
    <img src="assets/images/icon.png" alt="" class="card__icon" loading="lazy">
    <h3 class="card__title">Title</h3>
</a>
```

**Important structural differences:**
- Wide cards use `.card__body` wrapper div to position icon on top of title/domain
- Wide cards have `.card__image` for preview on the right side
- Compact cards hide domain text but keep title and smaller icon

### Update Profile

Edit around line 237-250 in `index.html`:

```html
<h1 class="profile__name">Your Name</h1>
<p class="profile__bio">Your bio text here...</p>
<div class="profile__meta">
    <div class="profile__meta-item">📍 Location</div>
    <div class="profile__meta-item">💼 Role</div>
    <div class="profile__meta-item">🌍 Origin</div>
</div>
```

Replace profile photo: `assets/images/profile.jpg`

## Customization

### Change Colors

Edit CSS variables in the `<style>` section (around line 12):

```css
:root {
    --bg-color: #f5f5f5;        /* Page background */
    --card-bg: #ffffff;         /* Card background */
    --text-primary: #1a1a1a;    /* Main text color */
    --text-secondary: #666666;  /* Secondary text */
    --text-tertiary: #999999;   /* Tertiary text */
    --border-radius: 16px;      /* Card corner radius */
    --shadow-sm: 0 1px 3px rgba(0, 0, 0, 0.08);
    --shadow-hover: 0 8px 24px rgba(0, 0, 0, 0.12);
}
```

### Adjust Grid Layout

Modify grid columns around line 87-88:

```css
@media (min-width: 640px) { .cards { grid-template-columns: repeat(2, 189px); } }
@media (min-width: 900px) { .cards { grid-template-columns: repeat(4, 189px); } }
```

To change number of columns, update the `repeat()` count. Remember: 2 columns + gap (12px) = 390px for wide/compact cards.

## Image Management

### Adding Images

1. Save to `assets/images/` with descriptive name (e.g., `company-logo.png`)
2. Reference in HTML: `<img src="assets/images/company-logo.png">`
3. Use `loading="lazy"` attribute for performance
4. Icons should be 40x40px (28x28px for compact cards)
5. Preview images for wide cards should be approximately 110px wide

### Image Formats

- **Icons:** PNG or SVG preferred
- **Previews:** PNG or JPG
- **Profile photo:** JPG (120x120px, circular crop via CSS)

## Technical Notes

### HTML Structure
- All external links use `target="_blank" rel="noopener noreferrer"` for security
- Images use `loading="lazy"` for performance
- Semantic HTML: `<aside>` for sidebar, `<main>` for content, `<section>` for groups
- Alt text is empty (`alt=""`) on decorative images (correct per accessibility guidelines)

### Performance
- **Page Size:** ~35KB HTML + 12MB images (lazy loaded)
- **No JavaScript:** Immediate time to interactive
- **Inline CSS:** Single HTTP request for styles
- **No external dependencies:** No CDNs, fonts, or scripts

### Card Sizing Reference

These exact dimensions match the original Bento layout:

| Card Type | Width | Height | Padding | Grid Span |
|-----------|-------|--------|---------|-----------|
| Small | 189px (1 col) | 175px | 24px all | 1 column |
| Wide | 390px (2 cols) | 175px | 24px all | 2 columns |
| Compact | 390px (2 cols) | 67.5px | 20px top/bottom, 24px left/right | 2 columns |

Grid: 189px columns with 12px gap → 2 columns + gap = 390px

## Quick Reference

| Task | Command/Location |
|------|-----------------|
| Preview locally | `open -a "Google Chrome" index.html` |
| Deploy to production | `git push origin master` |
| Edit profile | index.html lines 237-250 |
| Add new card | Copy existing card structure in relevant section |
| Change colors | Edit CSS variables in :root (line 12-21) |
| Update grid layout | Edit @media queries (line 87-88) |
| Add images | Save to assets/images/, reference in HTML |

## Known Limitations

- **Static HTML:** All links hardcoded (links.json exists but not consumed)
- **Manual updates:** Content changes require HTML edits
- **No analytics:** No built-in tracking
- **No build process:** No image optimization or minification
