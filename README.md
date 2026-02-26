# Precigen Internship Site

This repository contains the internship program website content and configuration.

## Project structure map

> Current repo status: minimal scaffold. Expand using the suggested structure below.

```text
precigen/
├─ README.md                      # This guide
├─ public/                        # Static assets (images, icons, files)
│  ├─ logo.svg                    # Primary logo used in header/footer
│  └─ og-cover.jpg                # Social preview image
├─ src/
│  ├─ data/
│  │  ├─ site.ts|js               # Global site config (title, description, links)
│  │  ├─ tracks.ts|js             # Internship tracks/modules content
│  │  └─ faq.ts|js                # FAQ content
│  ├─ components/                 # Reusable UI components (cards, sections, navbar)
│  ├─ sections/                   # Page-level sections (Hero, Tracks, Apply, etc.)
│  ├─ pages/                      # Route/page files (if framework uses pages)
│  └─ styles/                     # Global and component styles
├─ netlify.toml                   # Netlify deployment config (optional)
├─ vercel.json                    # Vercel deployment config (optional)
└─ package.json                   # Scripts/dependencies (if Node-based)
```

If you are using a non-Node static site setup, keep the same logical grouping (`assets`, `content/data`, `components`, `sections`) even if file names differ.

---

## How to run locally

Use the flow that matches your stack.

### Option A: Node-based app (Vite / Next.js / Astro / React)

1. Install dependencies:
   ```bash
   npm install
   ```
2. Start dev server:
   ```bash
   npm run dev
   ```
3. Open the local URL printed in terminal (commonly `http://localhost:3000` or `http://localhost:5173`).

### Option B: Static HTML/CSS/JS only

Use a local static server from project root:

```bash
python3 -m http.server 8080
```

Then open `http://localhost:8080`.

### Recommended scripts (if missing)

Add these to `package.json` for consistency:

```json
{
  "scripts": {
    "dev": "vite",
    "build": "vite build",
    "preview": "vite preview"
  }
}
```

---

## How to replace logo/images

1. Put new media files in `public/` (or your framework's static asset folder).
2. Replace references in header/footer/SEO files.

### Common replacement points

- **Logo in header/footer:**
  - `src/components/Navbar.*`
  - `src/components/Footer.*`
- **Hero/banner images:**
  - `src/sections/Hero.*`
- **Open Graph/Twitter social image:**
  - metadata config in `src/data/site.*` or page head config.

### Tips

- Keep logo as SVG when possible for crisp scaling.
- For social cards, use ~`1200x630` image.
- Compress large images before commit.
- Use descriptive names (`track-ai.jpg`, `mentor-team.webp`) for maintainability.

---

## Where to edit each section’s content

Use this content ownership map:

- **Hero section** (headline, subheadline, CTA):
  - `src/sections/Hero.*` and/or `src/data/site.*`
- **About / Program overview:**
  - `src/sections/About.*`
- **Tracks/modules list:**
  - `src/data/tracks.*` (data) + `src/sections/Tracks.*` (rendering)
- **Eligibility / Requirements:**
  - `src/sections/Eligibility.*`
- **Timeline / Important dates:**
  - `src/sections/Timeline.*`
- **FAQ:**
  - `src/data/faq.*`
- **Apply CTA / form links:**
  - `src/sections/Apply.*` and `src/data/site.*`
- **Footer and social links:**
  - `src/components/Footer.*` and `src/data/site.*`

If content is currently hardcoded in component files, move repeatable text into `src/data/*.ts|js` to make non-dev updates easier.

---

## How to add new internship tracks/modules

Use a data-driven approach.

### 1) Add a track object in `src/data/tracks.*`

Example:

```ts
export const tracks = [
  {
    id: "ai-research",
    title: "AI Research",
    duration: "12 weeks",
    level: "Intermediate",
    summary: "Work on model prototyping and evaluation.",
    skills: ["Python", "ML fundamentals", "Experiment tracking"],
    outcomes: ["Portfolio project", "Research presentation"]
  }
];
```

### 2) Ensure section renderer maps all fields

In `src/sections/Tracks.*`, render each property (title, duration, summary, skills, outcomes).

### 3) Add route/filter support (optional)

If tracks have dedicated pages, create slug-based pages such as:

- `src/pages/tracks/[id].*`

### 4) Validate

- Track appears on listing page.
- Detail page (if any) resolves correctly.
- No layout overflow on mobile.

---

## Deployment instructions

### Netlify

1. Push repository to GitHub.
2. In Netlify, **Add new site** → **Import from Git**.
3. Configure:
   - Build command: `npm run build` (if applicable)
   - Publish directory: `dist` (Vite) or framework output
4. Add environment variables (if needed).
5. Deploy.

Optional `netlify.toml`:

```toml
[build]
  command = "npm run build"
  publish = "dist"
```

### Vercel

1. Import repo in Vercel.
2. Framework preset is usually auto-detected.
3. Configure build/output if prompted.
4. Add env vars.
5. Deploy.

Optional `vercel.json` for static output:

```json
{
  "cleanUrls": true,
  "trailingSlash": false
}
```

### Static hosting (GitHub Pages / S3 / any CDN)

1. Build static assets (`npm run build`) or use raw static files.
2. Upload output folder (`dist`, `build`, or site root) to host.
3. Configure redirects for SPA routing if needed.
4. Set custom domain + HTTPS.

---

## Basic SEO checklist

Before release, verify:

- [ ] Unique `<title>` for homepage and important pages.
- [ ] Meta description (120–160 chars) describing the internship clearly.
- [ ] Open Graph tags:
  - [ ] `og:title`
  - [ ] `og:description`
  - [ ] `og:image` (`1200x630` recommended)
  - [ ] `og:type` (`website`)
- [ ] Twitter card tags:
  - [ ] `twitter:card` (`summary_large_image`)
  - [ ] `twitter:title`
  - [ ] `twitter:description`
  - [ ] `twitter:image`
- [ ] Canonical URL configured.
- [ ] Favicon + app icons set.
- [ ] Semantic headings (`h1` once per page, nested headings correct).
- [ ] Meaningful alt text for all non-decorative images.
- [ ] `sitemap.xml` and `robots.txt` present.
- [ ] Performance basics: optimized images, lazy loading, minimal JS.

---

## Maintenance notes

- Keep all business/content text in `src/data/` where possible.
- Keep sections componentized in `src/sections/`.
- When adding a new section, update this README so future editors know where content lives.
