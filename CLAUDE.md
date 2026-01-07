# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

Static website for the AI & Open Government Workshop, co-located with ICAIL 2026 (International Conference on Artificial Intelligence and Law). The site is hosted at https://aiog.net via GitHub Pages.

## Development Commands

```bash
# Start local development server
python -m http.server 8000

# Then visit http://localhost:8000
```

**Deployment:** Automatic via GitHub Pages on push to main branch. No build step required.

## Architecture

This is a no-build static site:
- `index.html` - Single-page website with all content
- `style.css` - Complete styling (~800 lines, mobile-responsive)
- `favicon.svg` - SVG branding icon
- `header.jpg` - Header background image

### Key Technical Decisions

- **No JavaScript** - Pure HTML/CSS implementation
- **No build tools** - Direct editing, immediate deployment
- **GitHub Pages hosting** - Custom domain via CNAME file, `.nojekyll` disables Jekyll processing

### CSS Architecture

- Mobile-first responsive design (breakpoint: 768px)
- Google Fonts: Source Sans 3 (body), Space Grotesk (headings)
- Accessibility focus: WCAG AA contrast ratios, keyboard navigation support, focus-visible styles
- Print styles included

### SEO & Metadata

The site includes comprehensive SEO:
- Open Graph and Twitter Card meta tags
- JSON-LD structured data (Event schema with organizers)
- Canonical URL configuration
