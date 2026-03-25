# Coming Soon Splash Page — Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Deploy a minimal "Coming Soon" splash page for cultivatedgroup.ca with brand logos, social links, and the Astro + Cloudflare foundation.

**Architecture:** Static Astro site with Tailwind CSS. Single page (`index.astro`) using a `BaseLayout.astro` for head/meta. Logo assets copied from sibling project. Deployed to Cloudflare Pages.

**Tech Stack:** Astro 5, Tailwind CSS 3, `@astrojs/tailwind`, Google Fonts (Cormorant Garamond, Libre Baskerville)

**Spec:** `docs/superpowers/specs/2026-03-24-coming-soon-splash-design.md`

**Reference project:** `../cultivated_conference/` uses the same Astro + Tailwind + Cloudflare stack. Follow its patterns for config files.

---

### Task 1: Initialize git repo and scaffold Astro project

**Files:**
- Create: `package.json`
- Create: `astro.config.mjs`
- Create: `tailwind.config.mjs`
- Create: `tsconfig.json`
- Create: `.gitignore`

- [ ] **Step 1: Initialize git repo**

```bash
cd /Users/owen/personal/cultivated_group
git init
```

- [ ] **Step 2: Create `package.json`**

```json
{
  "name": "cultivated-group",
  "type": "module",
  "version": "0.0.1",
  "scripts": {
    "dev": "astro dev",
    "build": "astro build",
    "preview": "astro preview",
    "astro": "astro"
  },
  "dependencies": {
    "@astrojs/tailwind": "^6.0.2",
    "astro": "^5.1.1",
    "tailwindcss": "^3.4.17"
  }
}
```

Note: No `@astrojs/cloudflare`, `react`, or `lucide-react` — not needed for a static splash page. Add when SSR/interactivity is introduced.

- [ ] **Step 3: Create `astro.config.mjs`**

```js
import { defineConfig } from 'astro/config';
import tailwind from '@astrojs/tailwind';

export default defineConfig({
  integrations: [tailwind()],
  output: 'static',
});
```

- [ ] **Step 4: Create `tailwind.config.mjs`**

```js
/** @type {import('tailwindcss').Config} */
export default {
  content: ['./src/**/*.{astro,html,js,jsx,md,mdx,ts,tsx}'],
  theme: {
    extend: {
      colors: {
        forest: '#2F5233',
        cream: '#F9F9F7',
        charcoal: '#333333',
      },
      fontFamily: {
        heading: ['"Cormorant Garamond"', 'Georgia', 'serif'],
        body: ['"Libre Baskerville"', 'Georgia', 'serif'],
      },
    },
  },
  plugins: [],
};
```

- [ ] **Step 5: Create `tsconfig.json`**

```json
{
  "extends": "astro/tsconfigs/strict"
}
```

No React JSX config needed — pure Astro.

- [ ] **Step 6: Create `.gitignore`**

```gitignore
node_modules/
dist/
.astro/
.env
.env.*
.vscode/
.idea/
.DS_Store
.playwright-mcp/
.superpowers/
```

- [ ] **Step 7: Install dependencies**

Run: `npm install`

Expected: `node_modules/` created, `package-lock.json` generated.

- [ ] **Step 8: Verify Astro builds empty project**

Run: `npm run build`

Expected: Build succeeds (may warn about no pages yet — that's fine).

- [ ] **Step 9: Commit scaffold**

```bash
git add package.json package-lock.json astro.config.mjs tailwind.config.mjs tsconfig.json .gitignore
git commit -m "chore: scaffold Astro + Tailwind project"
```

---

### Task 2: Copy logo assets into public/

**Files:**
- Create: `public/nurturing-family-growth-logo.jpg` (copied from `../cultivated_values/assets/Nurturing family growth logo.jpg`)
- Create: `public/rn-cultivated-logo.jpg` (copied from `../cultivated_values/assets/R N Cultivated 4.jpg`)

- [ ] **Step 1: Create `public/` directory and copy logos**

```bash
mkdir -p public
cp "../cultivated_values/assets/Nurturing family growth logo.jpg" public/nurturing-family-growth-logo.jpg
cp "../cultivated_values/assets/R N Cultivated 4.jpg" public/rn-cultivated-logo.jpg
```

- [ ] **Step 2: Generate favicon files from NFG seal**

Use `sips` (macOS built-in) to resize the NFG seal for favicon and apple-touch-icon:

```bash
sips -z 32 32 -s format png public/nurturing-family-growth-logo.jpg --out public/favicon.png
sips -z 180 180 -s format png public/nurturing-family-growth-logo.jpg --out public/apple-touch-icon.png
```

- [ ] **Step 3: Verify files exist**

Run: `ls -la public/`

Expected: 4 files — `nurturing-family-growth-logo.jpg`, `rn-cultivated-logo.jpg`, `favicon.png`, `apple-touch-icon.png`.

- [ ] **Step 4: Commit assets**

```bash
git add public/
git commit -m "chore: add logo assets and favicons"
```

---

### Task 3: Create BaseLayout and Coming Soon page

**Files:**
- Create: `src/layouts/BaseLayout.astro`
- Create: `src/pages/index.astro`

- [ ] **Step 1: Create directory structure**

```bash
mkdir -p src/layouts src/pages
```

- [ ] **Step 2: Create `src/layouts/BaseLayout.astro`**

This is the shared layout with `<head>`, meta tags, Google Fonts, and Tailwind body styling.

```astro
---
interface Props {
  title: string;
  description: string;
}

const { title, description } = Astro.props;
---

<!doctype html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>{title}</title>
    <meta name="description" content={description} />

    <!-- OG Tags -->
    <meta property="og:title" content={title} />
    <meta property="og:description" content={description} />
    <meta property="og:type" content="website" />
    <meta property="og:url" content="https://cultivatedgroup.ca" />

    <!-- Favicon -->
    <link rel="icon" type="image/png" href="/favicon.png" />
    <link rel="apple-touch-icon" href="/apple-touch-icon.png" />

    <!-- Fonts -->
    <link rel="preconnect" href="https://fonts.googleapis.com" />
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin />
    <link
      href="https://fonts.googleapis.com/css2?family=Cormorant+Garamond:wght@300;600&family=Libre+Baskerville&display=swap"
      rel="stylesheet"
    />
  </head>
  <body class="bg-cream text-charcoal">
    <slot />
  </body>
</html>
```

- [ ] **Step 3: Create `src/pages/index.astro`**

The Coming Soon splash page. Centered layout with logo pair, divider, "Coming Soon" text, and social links.

```astro
---
import BaseLayout from '../layouts/BaseLayout.astro';
---

<BaseLayout
  title="CultivatED Group — Coming Soon"
  description="CultivatED Group — Empowering parents and educators to understand each child's unique developmental journey. Coming soon."
>
  <main class="min-h-screen flex items-center justify-center px-6">
    <div class="flex flex-col items-center gap-8">

      <!-- Logo pair: stacks vertically on mobile, horizontal on sm+ -->
      <div class="flex flex-col sm:flex-row items-center gap-5 sm:gap-8">
        <!-- NFG Seal -->
        <img
          src="/nurturing-family-growth-logo.jpg"
          alt="Nurturing Family Growth"
          class="h-20 sm:h-[120px] w-auto rounded-full"
        />
        <!-- Green divider bar: hidden on mobile, vertical on sm+ -->
        <div class="hidden sm:block w-[2px] h-24 bg-forest"></div>
        <!-- R/N Cultivated text mark -->
        <img
          src="/rn-cultivated-logo.jpg"
          alt="R/N Cultivated"
          class="h-12 sm:h-16 w-auto"
        />
      </div>

      <!-- Horizontal accent line -->
      <div class="w-32 h-px bg-gradient-to-r from-transparent via-forest to-transparent"></div>

      <!-- Coming Soon -->
      <h1 class="font-heading font-light text-forest text-lg sm:text-xl tracking-[0.3em] uppercase">
        Coming Soon
      </h1>

      <!-- Social links -->
      <div class="flex gap-4 mt-2">
        <!-- LinkedIn -->
        <a
          href="https://www.linkedin.com/company/cultivatedgroup"
          target="_blank"
          rel="noopener"
          aria-label="LinkedIn"
          class="w-9 h-9 rounded-full border border-[#999] flex items-center justify-center text-[#999] hover:border-forest hover:text-forest transition-colors"
        >
          <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" viewBox="0 0 24 24" fill="currentColor">
            <path d="M20.447 20.452h-3.554v-5.569c0-1.328-.027-3.037-1.852-3.037-1.853 0-2.136 1.445-2.136 2.939v5.667H9.351V9h3.414v1.561h.046c.477-.9 1.637-1.85 3.37-1.85 3.601 0 4.267 2.37 4.267 5.455v6.286zM5.337 7.433a2.062 2.062 0 01-2.063-2.065 2.064 2.064 0 112.063 2.065zm1.782 13.019H3.555V9h3.564v11.452zM22.225 0H1.771C.792 0 0 .774 0 1.729v20.542C0 23.227.792 24 1.771 24h20.451C23.2 24 24 23.227 24 22.271V1.729C24 .774 23.2 0 22.222 0h.003z"/>
          </svg>
        </a>
        <!-- Instagram -->
        <a
          href="https://www.instagram.com/cultivatedbyrn/"
          target="_blank"
          rel="noopener"
          aria-label="Instagram"
          class="w-9 h-9 rounded-full border border-[#999] flex items-center justify-center text-[#999] hover:border-forest hover:text-forest transition-colors"
        >
          <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" viewBox="0 0 24 24" fill="currentColor">
            <path d="M12 2.163c3.204 0 3.584.012 4.85.07 3.252.148 4.771 1.691 4.919 4.919.058 1.265.069 1.645.069 4.849 0 3.205-.012 3.584-.069 4.849-.149 3.225-1.664 4.771-4.919 4.919-1.266.058-1.644.07-4.85.07-3.204 0-3.584-.012-4.849-.07-3.26-.149-4.771-1.699-4.919-4.92-.058-1.265-.07-1.644-.07-4.849 0-3.204.013-3.583.07-4.849.149-3.227 1.664-4.771 4.919-4.919 1.266-.057 1.645-.069 4.849-.069zM12 0C8.741 0 8.333.014 7.053.072 2.695.272.273 2.69.073 7.052.014 8.333 0 8.741 0 12c0 3.259.014 3.668.072 4.948.2 4.358 2.618 6.78 6.98 6.98C8.333 23.986 8.741 24 12 24c3.259 0 3.668-.014 4.948-.072 4.354-.2 6.782-2.618 6.979-6.98.059-1.28.073-1.689.073-4.948 0-3.259-.014-3.667-.072-4.947-.196-4.354-2.617-6.78-6.979-6.98C15.668.014 15.259 0 12 0zm0 5.838a6.162 6.162 0 100 12.324 6.162 6.162 0 000-12.324zM12 16a4 4 0 110-8 4 4 0 010 8zm6.406-11.845a1.44 1.44 0 100 2.881 1.44 1.44 0 000-2.881z"/>
          </svg>
        </a>
      </div>

    </div>
  </main>
</BaseLayout>
```

- [ ] **Step 4: Build and verify**

Run: `npm run build`

Expected: Build succeeds, `dist/index.html` exists.

- [ ] **Step 5: Preview locally**

Run: `npm run preview`

Open the local preview URL. Verify:
- Logos display side by side with green divider
- "Coming Soon" text in forest green, uppercase, wide letter-spacing
- LinkedIn and Instagram icons link correctly (open in new tab)
- On mobile viewport (< 640px): logos stack vertically, seal reduces to 80px, green divider hidden, page is centered
- Background is cream (#F9F9F7)
- Fonts load (Cormorant Garamond on heading)

- [ ] **Step 6: Commit page**

```bash
git add src/
git commit -m "feat: add Coming Soon splash page with logos and social links"
```

---

### Task 4: Add CLAUDE.md and final commit

**Files:**
- Create: `CLAUDE.md`

- [ ] **Step 1: Create `CLAUDE.md`**

```markdown
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
```

- [ ] **Step 2: Commit CLAUDE.md**

```bash
git add CLAUDE.md
git commit -m "docs: add CLAUDE.md project guidance"
```

- [ ] **Step 3: Verify final state**

Run: `npm run build && echo "Build OK"`

Expected: Clean build, no warnings. Project ready for Cloudflare Pages deployment.
