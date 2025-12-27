# Ionut Ciobotaru - Personal Link Page

A static, Bento-style personal link page built as a self-hosted alternative to Bento.me (which is shutting down Feb 13, 2025).

## 🚀 Live Site

**URL:** https://ionut85.github.io

## 📁 Project Structure

```
/
├── index.html              # Main page (single-file, inline CSS)
├── links.json              # Source data for all links and sections
├── assets/
│   └── images/             # All downloaded images (86 total, 7.2MB)
│       ├── image_001.png through image_086.png
│       └── profile.jpg     # ⚠️ ADD YOUR PROFILE PHOTO HERE
├── bento_source.html       # Original Bento page HTML (for reference)
├── urls.txt                # Organized list of all URLs by section
└── README.md               # This file
```

## ✅ What's Complete

- ✅ **86 images downloaded** from original Bento page (7.2MB total)
- ✅ **Responsive HTML/CSS** with mobile-first design
- ✅ **Complete links.json** with all URLs from your Bento page
- ✅ **6 sections:** Entrepreneuring, Blogging, Media, Events, Investments, Past Projects
- ✅ **Graceful fallbacks** for missing images (icons hide, text remains)
- ✅ **Fast loading** with lazy image loading
- ✅ **Accessible** with semantic HTML and proper alt tags

## ⚠️ TODO: Add Your Profile Photo

The site currently shows "IC" initials as a fallback. To add your profile photo:

```bash
# Copy your profile photo to the assets folder:
cp /path/to/your/photo.jpg assets/images/profile.jpg
```

Your profile photo should be:
- Square aspect ratio (e.g., 400x400px or 800x800px)
- JPG or PNG format
- Under 500KB for fast loading

## 🔧 How to Update Links

### Method 1: Edit links.json (Recommended)

1. Open `links.json`
2. Add/edit/remove entries in the `sections` array
3. Each card needs:
   ```json
   {
     "title": "Card Title",
     "url": "https://example.com",
     "domain": "example.com",
     "size": "small",  // small, medium, large, or hero
     "icon": "assets/images/icon.png"  // optional
   }
   ```
4. Save the file
5. **Currently:** links are hardcoded in index.html
6. **Future:** You can add JavaScript to dynamically load from links.json

### Method 2: Edit index.html Directly

1. Open `index.html`
2. Find the section you want to modify (search for `<section class="section">`)
3. Add/edit card HTML:
   ```html
   <a href="URL" class="card card--small" target="_blank" rel="noopener noreferrer">
       <img src="assets/images/icon.png" alt="" class="card__icon">
       <div class="card__content">
           <h3 class="card__title">Title</h3>
           <span class="card__domain">domain.com</span>
       </div>
   </a>
   ```
4. Card size classes: `card--small`, `card--medium`, `card--large`, `card--hero`

## 🎨 Customization

### Colors

Edit CSS variables in `<style>` section of `index.html`:

```css
:root {
    --bg-color: #f5f5f5;           /* Page background */
    --card-bg: #ffffff;            /* Card background */
    --text-primary: #1a1a1a;       /* Main text */
    --text-secondary: #666666;      /* Secondary text */
    --border-radius: 16px;         /* Card roundness */
}
```

### Layout

- **Desktop:** 4-column grid for cards
- **Tablet:** 2-column grid
- **Mobile:** 1-column stack
- Breakpoints: `640px` (tablet), `1024px` (desktop)

### Profile Section

Edit the profile sidebar in `index.html` around line 354:

```html
<h1 class="profile__name">Your Name</h1>
<p class="profile__bio">Your bio...</p>
```

## 🌐 Deploying to GitHub Pages

Your site is already set up for GitHub Pages!

### Initial Setup (if not done):

```bash
# 1. Ensure you're in the repository
cd /Users/ionutciobotaru/Documents/GitHub/ionut85.github.io

# 2. Add all files
git add .

# 3. Commit
git commit -m "Update personal link page"

# 4. Push to GitHub
git push origin master
```

### Enable GitHub Pages (if not enabled):

1. Go to https://github.com/ionut85/ionut85.github.io/settings/pages
2. Under "Source", select `master` branch
3. Click "Save"
4. Your site will be live at https://ionut85.github.io within a few minutes

### Update the Live Site:

```bash
# After making changes:
git add .
git commit -m "Describe your changes"
git push origin master

# GitHub Pages will automatically rebuild (usually takes 1-2 minutes)
```

## 📸 Adding New Images

### For Card Icons/Logos:

```bash
# 1. Download the image
curl -o assets/images/mycompany-icon.png https://example.com/logo.png

# 2. Reference in links.json:
"icon": "assets/images/mycompany-icon.png"

# 3. Or in HTML:
<img src="assets/images/mycompany-icon.png" alt="" class="card__icon">
```

### For Card Previews/Thumbnails:

```bash
# 1. Add image to assets/images/
cp ~/Downloads/preview.jpg assets/images/my-preview.jpg

# 2. Reference in links.json:
"preview": "assets/images/my-preview.jpg"

# 3. Or in HTML:
<img src="assets/images/my-preview.jpg" alt="" class="card__preview" loading="lazy">
```

## 🔍 Image Organization

The 86 downloaded images from Bento are numbered `image_001.png` through `image_086.png`. These include:

- Company/service favicons and icons
- Preview images from social media posts
- OG images from linked articles
- Thumbnails for media appearances

**Recommended:** Rename images to be more descriptive:

```bash
# Example:
mv assets/images/image_042.jpeg assets/images/incrmntal-preview.jpg
mv assets/images/image_015.png assets/images/hypd-icon.png
```

Then update references in `index.html` accordingly.

## 📊 Site Performance

- **Size:** ~40KB HTML + 7.2MB images
- **Loading:** Images lazy-load as you scroll
- **Caching:** Browsers cache images automatically
- **Mobile-friendly:** Responsive design, tested on iOS/Android

## 🛠 Technical Details

- **Zero JavaScript:** Pure HTML + CSS
- **Single file deployment:** All CSS inline in `index.html`
- **Responsive Grid:** CSS Grid with mobile-first breakpoints
- **Image fallbacks:** `onerror` handlers hide broken images
- **Accessibility:** Semantic HTML, proper heading structure, adequate contrast

## 📝 Maintenance Schedule

- **Weekly:** Review analytics (if you add them)
- **Monthly:** Check for broken links
- **Quarterly:** Update bio, add new projects
- **Before Bento shutdown (Feb 13):** Download any remaining images!

## 🆘 Troubleshooting

### Images Not Loading

1. Check file paths are correct (case-sensitive!)
2. Verify images exist in `assets/images/`
3. Check browser console for 404 errors

### Layout Broken on Mobile

1. Test in Chrome DevTools mobile view
2. Check viewport meta tag is present:
   ```html
   <meta name="viewport" content="width=device-width, initial-scale=1.0">
   ```

### Profile Photo Not Showing

1. Ensure file is named exactly `profile.jpg`
2. Check it's in `assets/images/` directory
3. Verify file isn't corrupted (open it locally)

## 📞 Questions?

This README was generated during the Bento → Static Site migration on Dec 26, 2025.

For technical issues, refer to:
- HTML structure in `index.html`
- Original data in `links.json` and `urls.txt`
- Original Bento page in `bento_source.html`

---

**Built as a static alternative to Bento.me** | Deployed on GitHub Pages
