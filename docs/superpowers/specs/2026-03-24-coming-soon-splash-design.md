# Coming Soon Splash Page — Design Spec

## Overview

A minimal "Coming Soon" splash page for cultivatedgroup.ca. This is the first deployment of the CultivatED Group website, establishing the Astro + Cloudflare foundation that the full site will build upon.

## Tech Stack

- **Framework:** Astro (static output mode — switch to SSR with `@astrojs/cloudflare` adapter when backend features are needed)
- **Styling:** Tailwind CSS
- **Deployment:** Cloudflare Pages (static deploy for now, Workers-based SSR later)
- **Future backend:** Cloudflare D1 (database), R2 (file storage), Workers (API logic)

## Brand Identity

- **Logos:** NFG circular seal + "R/N CULTIVATED" horizontal text mark, separated by a vertical `#2F5233` divider bar
  - Source files: `../cultivated_values/assets/Nurturing family growth logo.jpg` and `../cultivated_values/assets/R N Cultivated 4.jpg`
  - Manually copy into `public/` as `nurturing-family-growth-logo.jpg` and `rn-cultivated-logo.jpg` (kebab-case rename)
- **Primary color:** `#2F5233` (forest green)
- **Background:** `#F9F9F7` (cream)
- **Text:** `#333333` (charcoal)
- **Heading font:** Cormorant Garamond — load weights 300 and 600 from Google Fonts CDN
- **Body font:** Libre Baskerville — load weight 400 from Google Fonts CDN

## Layout — Centered Minimal

Full-viewport centered layout with the following vertical stack:

1. **Logo pair** — NFG circular seal (left) + `#2F5233` vertical divider bar (2px wide, ~80% of seal height) + "R/N CULTIVATED" text mark (right), horizontally centered
2. **Horizontal accent line** — subtle gradient divider (`transparent → #2F5233 → transparent`)
3. **"Coming Soon"** — Cormorant Garamond 300, uppercase, `letter-spacing: 0.3em`, `color: #2F5233`
4. **Social links** — inline SVG icons (LinkedIn and Instagram) as minimal 36px circular outlined buttons

### Specifications

- Background: full-page cream (`#F9F9F7`), `min-height: 100vh`
- Vertically and horizontally centered (flexbox)
- NFG seal: ~120px height, `alt="Nurturing Family Growth"`
- R/N CULTIVATED: auto width, matched vertical center, `alt="R/N Cultivated"`
- Social icons: inline SVG, 36px circles with 1px `#999` border, `#999` icon fill. Links:
  - LinkedIn: https://www.linkedin.com/company/cultivatedgroup (`target="_blank"`, `rel="noopener"`)
  - Instagram: https://www.instagram.com/cultivatedbyrn/ (`target="_blank"`, `rel="noopener"`)
- Responsive: logos stack vertically on mobile (`< 640px`), reduce seal to ~80px

## SEO & Meta

- `<title>`: "CultivatED Group — Coming Soon"
- Meta description: "CultivatED Group — Empowering parents and educators to understand each child's unique developmental journey. Coming soon."
- OG tags: title + description only (no OG image for Coming Soon — will add with full site launch)
- Favicon: 32x32 PNG derived from NFG seal, plus 180x180 apple-touch-icon. Generate manually from the seal image and place in `public/`.

## Files to Create

```
cultivated_group/
├── astro.config.mjs
├── tailwind.config.mjs
├── tsconfig.json
├── package.json
├── .gitignore
├── public/
│   ├── favicon.png                          (32x32, derived from NFG seal)
│   ├── apple-touch-icon.png                 (180x180, derived from NFG seal)
│   ├── nurturing-family-growth-logo.jpg     (copied from ../cultivated_values/assets/Nurturing family growth logo.jpg)
│   └── rn-cultivated-logo.jpg               (copied from ../cultivated_values/assets/R N Cultivated 4.jpg)
├── src/
│   ├── layouts/
│   │   └── BaseLayout.astro                 (head, meta, Google Fonts links, body wrapper)
│   └── pages/
│       └── index.astro                      (Coming Soon page)
└── CLAUDE.md                                (project guidance for future development)
```

Note: `wrangler.toml` is not needed for Cloudflare Pages static deploys — configuration is handled via the Cloudflare dashboard or `wrangler pages` CLI.

## Deployment

- Cloudflare Pages project linked to the repo
- Custom domain: cultivatedgroup.ca
- Build command: `npm run build`
- Output directory: `dist/`

## What This Does NOT Include

- No tagline or mission statement text (kept minimal per decision)
- No email subscribe form (deferred)
- No navigation (single page)
- No backend/D1 setup (not needed yet)
- No OG image asset (deferred to full site launch)

## Future Considerations

The full site will expand this foundation with:
- 6 main sections (Root, Heart, Stems, CultivatE, Harvest, Connect)
- Switch to SSR mode with `@astrojs/cloudflare` adapter
- D1 database for Talent Hub job board and Event Board
- R2 for resume uploads
- Blog/Journal with subscriber emails
- Contact form with role-based routing
