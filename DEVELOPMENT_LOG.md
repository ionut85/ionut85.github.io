# Development Log - ionut85.github.io

## Project Summary

This is a static, single-page personal link portfolio (Bento-style) built as a self-hosted alternative to Bento.me before its shutdown (February 13, 2025). The site replicates the original Bento layout with custom styling and properly organized assets.

**Live Site:** https://ionut85.github.io

---

## Key Design Decisions

### 1. Card System Architecture

We implemented three distinct card types with fixed dimensions:

- **Small cards** (189px × 175px): Single column, vertical layout with icon on top
- **Wide cards** (390px × 175px): Two-column span, horizontal layout with icon/text on left, preview image (178px × 127px) on right
- **Compact cards** (390px × 67.5px): Two-column span, horizontal layout with icon on left, no domain text

**Grid Calculation:**
- Base column: 189px
- Gap: 12px
- Wide/compact width: 189px + 12px + 189px = 390px

### 2. Responsive Behavior

**Critical Decision:** All cards maintain fixed dimensions across all screen widths. They do not resize or reflow internally - only their grid positioning changes.

**Breakpoints:**
- **Mobile (< 640px):** Cards center-aligned, stack vertically in single column (auto-fit)
- **Tablet (640px - 899px):** 2-column grid (378px + 12px gap = 390px total)
- **Desktop (900px+):** 4-column grid (756px + 3 gaps = 792px total)

**Layout principle:** Cards reflow like Bento - they move under each other as screen narrows, but maintain exact pixel dimensions.

### 3. Image Management Strategy

**Workflow established:**
1. Download images from live Bento page or source URLs
2. Use descriptive naming: `{project}-icon.{ext}` and `{project}-preview.{ext}`
3. Replace all generic `image_XXX` references with named files
4. Store in `/assets/images/` directory

**Icon specifications:**
- Size: 40px × 40px (consistent across all card types)
- Border-radius: 10px
- Padding: 8px
- Background: var(--bg-color) with subtle shadow

**Preview image specifications (wide cards only):**
- Size: 178px × 127px
- Border-radius: 10px
- Object-fit: cover
- Position: Right side of card

### 4. Special Card: Open Internet

**Unique implementation:**
- Full background image spanning entire card (390px × 175px)
- No visible text or icon elements
- Uses `.card--background` modifier class with inline `background-image` style
- CSS hides `.card__body` and `.card__image` elements

---

## Complete Workflows Executed

### Workflow 1: Image Remapping from Bento

**Steps:**
1. Used Task agent to fetch live Bento page (https://bento.me/ionut)
2. Extracted all image URLs using WebFetch and Grep
3. Downloaded 29+ properly-named images:
   - Social icons: twitter-icon.png, linkedin-icon.svg, instagram-icon.svg, medium-icon.svg, github-icon.svg
   - Project icons: hypd-icon.png, noah-icon.png, adcp-icon.svg, etc.
   - Preview images: hypd-preview.png, noah-screenshot.jpeg, incrmntal-preview.jpeg, etc.
4. Created `image-mapping.json` for traceability (Bento URL → local filename)
5. Updated all `image_XXX` references in index.html to named files

**Result:** Zero generic image references remain (verified with grep)

### Workflow 2: Social Media Icons (SVG Migration)

**Problem:** LinkedIn, Instagram, Medium showed broken images (tiny PNGs)

**Solution:**
1. Downloaded official SVG icons from SuperTinyIcons repository
2. Updated references from `.png` to `.svg`:
   - `linkedin-icon.png` → `linkedin-icon.svg`
   - `instagram-icon.png` → `instagram-icon.svg`
   - `medium-icon.png` → `medium-icon.svg`
   - `spotify-icon.png` → `spotify-icon.svg`

### Workflow 3: Social Media Card Title Updates

**Changed from generic to specific:**
- Twitter: "Twitter" → "Ionut Ciobotaru" (@weeb0)
- LinkedIn: "LinkedIn" → "Ionut Ciobotaru - iion | LinkedIn"
- Medium: "Medium" → "Ionut Ciobotaru – Medium"
- Replit: "Replit" → "cionut (cionut)"
- GitHub: "GitHub" → "ionut85"
- Instagram: "Instagram" → "@cionut"

### Workflow 4: Preview Image Assignments

**Added preview images to wide cards:**
- Noah Pretzels: `noah-large.jpg` → `noah-screenshot.jpeg` (corrected)
- AdCP: Added `adcp-preview.png`
- IndieGrow: Renamed `image_086.png` → `indiegrow-preview.png`
- First Party Capital: Renamed `image_081.jpeg` → `firstpartycapital-preview.jpeg`
- AdTech Cares: Renamed `image_038.jpeg` → `adtechcares-preview.jpeg` (also used as icon)
- Prebid: Renamed `image_080.jpeg` → `prebid-preview.jpeg`
- IAB Europe: Renamed `image_061.png` → `iab-preview.png`
- VentureBeat: Downloaded from `gamesbeat.com/wp-content/uploads/2025/05/titleimage.png`

### Workflow 5: Icon Replacements (Custom Downloads)

**Downloaded specific icons from source URLs:**
- Inspector Gadget: `https://inspectorgadget.info/favicon.ico`
- Windows PC Romania: Facebook CDN URL (8.5KB PNG)
- Take A Trip: Facebook CDN URL (29KB JPG)
- Applift: `https://media.pocketgamer.biz/images/55315/66078/applift-logo_s470.webp`
- VentureBeat: `https://images.icon-icons.com/2699/PNG/512/venturebeat_logo_icon_167688.png`
- Monetaria Statului: Downloaded from Bento (was 0 bytes, fixed to 13KB)

### Workflow 6: Card Dimension Fixes

**Issue:** Cards had inconsistent sizing, didn't match original Bento

**Solution applied exact specifications:**
```css
.card--wide {
    width: 390px;
    height: 175px;
    padding: 24px;
}

.card--compact {
    width: 390px;
    height: 67.5px;
    padding: 20px 24px; /* top/bottom: 20px, left/right: 24px */
}

.card--small {
    width: 189px;
    height: 175px;
    padding: 24px;
}

.card--wide .card__image {
    width: 178px;
    height: 127px;
}
```

**Grid columns adjusted:** 189px each with 12px gap

### Workflow 7: Content Centering

**Problem:** Content appeared left-aligned on wider screens

**Solution:**
1. Added `justify-content: center` to `.cards` grid
2. Made `.section` a flexbox column with centered alignment
3. Set section title max-widths to match grid width at each breakpoint:
   - Tablet: `max-width: calc(2 * 189px + 12px)` = 390px
   - Desktop: `max-width: calc(4 * 189px + 3 * 12px)` = 792px

### Workflow 8: Compact Card Icon Size Normalization

**Changed:** Compact card icons from 28px × 28px to 40px × 40px to match all other cards

**Before:**
```css
.card--compact .card__icon { width: 28px; height: 28px; }
```

**After:**
```css
.card--compact .card__icon { width: 40px; height: 40px; }
```

### Workflow 9: Fixed Card Dimensions Across All Screen Sizes

**Critical decision:** Remove all responsive card resizing

**Changes made:**
1. Removed mobile overrides that changed card dimensions
2. Set fixed widths on all card types
3. Changed grid from `1fr` to `repeat(auto-fit, 189px)` on mobile with `justify-content: center`
4. Cards now maintain exact dimensions and simply reflow into fewer columns as screen narrows

**Result:** Bento-like behavior where cards stack but don't resize

---

## File Structure

```
ionut85.github.io/
├── index.html                          # Main production file (654 lines, all CSS inline)
├── CLAUDE.md                          # Claude Code guidance (updated)
├── DEVELOPMENT_LOG.md                 # This file - complete development history
├── README.md                          # User-facing documentation
├── index-simple.html                  # Backup/reference version
├── bento_source.html                  # Original Bento export (12,511 lines)
├── links.json                         # Structured data (not currently consumed)
├── assets/
│   └── images/                        # 139 total images (12MB)
│       ├── profile.jpg                # Profile avatar
│       ├── banner.jpeg                # Cover image
│       ├── open-internet.jpeg         # Special background card
│       ├── *-icon.{svg,png,jpg,ico}   # 70+ named icon files
│       ├── *-preview.{png,jpg,jpeg}   # 25+ preview images
│       ├── *-screenshot.jpeg          # 15+ screenshot images
│       └── image-mapping.json         # Bento URL → local file mapping
├── urls.txt                           # Reference list of all URLs
└── extracted_images.txt               # Image inventory from Bento
```

---

## CSS Architecture Summary

### Color System
```css
--bg-color: #f5f5f5;        /* Light gray page background */
--card-bg: #ffffff;         /* White cards */
--text-primary: #1a1a1a;    /* Near-black text */
--text-secondary: #666666;  /* Gray text */
--text-tertiary: #999999;   /* Light gray */
--border-radius: 16px;      /* Card corners */
--shadow-sm: 0 1px 3px rgba(0, 0, 0, 0.08);
--shadow-hover: 0 8px 24px rgba(0, 0, 0, 0.12);
```

### Typography
- Font stack: `-apple-system, BlinkMacSystemFont, 'Segoe UI', 'Roboto'`
- Section titles: 20px, font-weight 700
- Card titles: Default (varies by card type)
- Compact card titles: 14px

### Layout Structure
```
.container (max-width: 1400px, centered)
  └── .layout (grid: sidebar + content)
      ├── .profile (320px sidebar, sticky on desktop)
      └── .content (1fr main content)
          └── .section (centered flex column)
              ├── .section__title
              └── .cards (grid: auto-fit → 2 cols → 4 cols)
                  └── .card variants (fixed dimensions)
```

---

## Content Organization

### 6 Main Sections (49 cards total)

1. **Entrepreneuring (current projects)** - 4 cards
   - HYPD.ai (wide) - AI for performance marketers
   - Noah Pretzels (wide) - Food product company
   - The Turing Marketer (wide) - Substack newsletter
   - AdCP (wide) - Open advertising standard

2. **Blogging, Marketing, Developing** - 7 cards
   - Open Internet (wide, background image) - Advocacy
   - Twitter (small) - Ionut Ciobotaru
   - LinkedIn (small) - Ionut Ciobotaru - iion
   - Medium, Replit, GitHub, Instagram (all compact)

3. **Media** - 8 cards
   - AdExchanger article (wide)
   - VentureBeat article (wide)
   - YouTube podcast (wide)
   - Slideshare white paper (wide)
   - ID5 IDENTITY 2023 (wide)
   - IAB UK article (wide)
   - IAB Europe trends (wide)
   - Spotify podcast (wide)

4. **Events & Memberships** - 5 cards
   - MMA Global Speaker (wide)
   - Meetup Berlin AdTech (wide)
   - AdTech Cares (wide)
   - Prebid.org (wide)
   - IAB Native Predictions (wide)

5. **Investments** - 14 cards
   - INCRMNTAL (wide) - Primary investment
   - NumberEight AI, IONIQ, Verve Group/MGI (wide) (small/wide)
   - Kayzen, Reflect, Take A Trip, Context SDK, iion, Substack, PubStation, FI Mentor, Replit, AI Fund (all small)
   - First Party Capital (wide)

6. **Past Projects** - 11 cards
   - PubNative, Verve, Applift, MGI, Inspector Gadget, IT Genetics, ITG Store, Bettahub, Gave.ro, Super Pantofi, Monetaria Statului, Windows PC Romania (all small)
   - IndieGrow (wide)

---

## Deployment

**No build process required** - site is pure static HTML/CSS

### Git Workflow
```bash
# Make changes
git add .
git commit -m "Description of changes"
git push origin master

# GitHub Pages auto-deploys from master branch
# Live within 1-2 minutes at https://ionut85.github.io
```

---

## Key Technical Decisions - Rationale

### Why inline CSS?
- Single HTTP request
- No build step needed
- Immediate time to interactive
- Easy to maintain for single-page site

### Why no JavaScript?
- Pure content display (no interactivity needed)
- Maximum performance
- SEO-friendly
- Works without JS enabled

### Why fixed card dimensions?
- Matches original Bento behavior
- Predictable layout across devices
- Professional appearance
- Easier to maintain (no complex responsive logic)

### Why descriptive image names?
- Maintainability: Easy to find/replace images
- Clarity: Obvious what each file is for
- Version control: Meaningful diffs
- Future-proofing: No conflicts with numbered files

---

## Common Maintenance Tasks

### Add a new wide card
```html
<a href="URL" class="card card--wide" target="_blank" rel="noopener">
    <div class="card__body">
        <img src="assets/images/project-icon.png" alt="" class="card__icon" loading="lazy">
        <h3 class="card__title">Project Title</h3>
        <span class="card__domain">domain.com</span>
    </div>
    <img src="assets/images/project-preview.png" alt="" class="card__image" loading="lazy">
</a>
```

### Add a new small card
```html
<a href="URL" class="card card--small" target="_blank" rel="noopener">
    <img src="assets/images/project-icon.png" alt="" class="card__icon" loading="lazy">
    <h3 class="card__title">Project Name</h3>
    <span class="card__domain">domain.com</span>
</a>
```

### Add a new compact card
```html
<a href="URL" class="card card--compact" target="_blank" rel="noopener">
    <img src="assets/images/project-icon.png" alt="" class="card__icon" loading="lazy">
    <h3 class="card__title">Project Name</h3>
</a>
```

### Update profile information
Edit `index.html` around lines 237-250

### Change color scheme
Edit CSS variables in `:root` (lines 12-21)

---

## Lessons Learned

1. **Start with exact specifications** - Getting pixel-perfect dimensions early saved multiple iterations
2. **Descriptive naming matters** - Generic image_XXX files caused confusion and errors
3. **Fixed layouts are simpler** - Less CSS complexity than fully responsive cards
4. **SVG icons > PNG** - Better scaling, smaller file sizes, crisper rendering
5. **Center alignment critical** - Fixed-width grids need explicit centering
6. **Document as you go** - This log captures decisions that would be lost otherwise

---

## Future Enhancements (Potential)

- [ ] Dynamic rendering from links.json using JavaScript
- [ ] Dark mode toggle
- [ ] Image optimization pipeline (build step)
- [ ] Analytics integration (Google Analytics / Plausible)
- [ ] Search functionality across links
- [ ] Admin panel for content management
- [ ] Automated link validation
- [ ] RSS feed generation

---

**Last Updated:** December 27, 2024
**Version:** 1.0 (Production)
