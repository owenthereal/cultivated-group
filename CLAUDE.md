# CLAUDE.md

This file provides guidance to Claude Code when working with code in this repository.

## Project Overview

CultivatED Group company website — ECE consulting services and platform. Domain: cultivatedgroup.ca

## Tech Stack

- **Framework:** Astro (static output, will switch to SSR with `@astrojs/cloudflare` when backend needed)
- **Styling:** Tailwind CSS
- **Deployment:** Cloudflare Pages
- **Future backend:** Cloudflare D1 (database), R2 (file storage), Workers (API)

## Build & Development Commands

npm install          # Install dependencies
npm run dev          # Start dev server
npm run build        # Build for production (output: dist/)
npm run preview      # Preview production build

## Design System

**Colors:**
- Primary (Forest Green): `#2F5233`
- Background (Cream): `#F9F9F7`
- Text (Charcoal): `#333333`

**Typography:**
- Headings: Cormorant Garamond (serif) — weights 300, 600
- Body: Libre Baskerville (serif) — weight 400
- Loaded via Google Fonts CDN

**Brand:** Nature-inspired, earth tones, grounding aesthetic. Must NOT come across as admissions consulting.

## Architecture

- `src/layouts/BaseLayout.astro` — shared head, meta, fonts
- `src/pages/` — Astro pages
- `public/` — static assets (logos, favicons)

## Related Projects

- `../cultivated_conference/` — CultivatED Conference site (Astro + Tailwind + Cloudflare)
- `../cultivated_values/` — Core values sign (logo assets sourced from here)
