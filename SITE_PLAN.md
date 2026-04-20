# Site Plan

Planning document for Tucker Lancaster's personal portfolio website. This file complements `CLAUDE.md` — where CLAUDE.md defines conventions and voice, this file defines concrete content and structure. Claude Code should reference both when implementing.

---

## Goal and audience

A portfolio site aimed at industry hiring managers in tech, ML/CV, biotech, and digital health. The site frames Tucker's PhD work as general computer vision and machine learning engineering, validated in a specific application domain (animal behavior analysis), rather than as fish-specific research. A recruiter scanning the site in 60 seconds should come away with: "computer vision and ML engineer, builds production open-source tools, shipped hardware + models to the edge, works with multi-camera 3D systems."

---

## Site map

Four pages, flat hierarchy.

```
/                         index.qmd              Landing — bio, photo, links
/projects/                projects.qmd           Grid of project cards
/projects/aquapose/       projects/aquapose/index.qmd
/projects/oksort/         projects/oksort/index.qmd
/projects/aquamvs/        projects/aquamvs/index.qmd
/projects/aquacal/        projects/aquacal/index.qmd
/projects/sartab/         projects/sartab/index.qmd
/cv/                      cv.qmd (deferred)      CV — PDF link, decision pending
```

No blog. No publications page. No separate about page — the landing serves as about.

---

## Global elements

### Navbar
Right-aligned. Four items followed by two social tool icons:
- Home · Projects · CV · | GitHub · LinkedIn

### Footer
- Left: "© 2026 Tucker Lancaster"
- Right: email icon linking to mailto:
- Nothing else.

### Theme
- Start with Quarto's built-in `cosmo` theme.
- Enable dark mode via `theme: [cosmo, cosmo-dark]` so a toggle appears in the navbar.
- Defer `_brand.yml` custom colors until after initial launch.

### Social / OG metadata
Set `site-url`, `description`, and a default preview `image` in `_quarto.yml` so that links shared on LinkedIn, Twitter, and Slack render with a correct preview card. This is easy to forget and noticeable when missing.

---

## Landing page (`index.qmd`)

Template: `trestles` (photo and social links on left rail, content on right).

### Content

**Title:** Tucker Lancaster

**Tagline (subtitle or first line):**
> I build computer vision and machine learning systems for animal behavior analysis — multi-camera 3D reconstruction, pose estimation and tracking, and edge-deployed action recognition.

**Bio paragraph:**
> I recently completed a PhD in Quantitative Biosciences at Georgia Tech, with a minor in Machine Learning, where I built open-source Python libraries for 3D animal tracking in naturalistic environments. My work spans multi-camera geometry, 3D reconstruction, multi-object tracking, pose estimation, and edge deployment — shipping tools that run in production across dozens of research tanks and scale from Raspberry Pi units to 12-camera refractive arrays. I'm looking for industry roles applying computer vision to real-world problems.

**Calls to action:** two buttons or prominent links — "See projects →" and "Download CV →" (CV link can be a placeholder until the CV is finalized).

**Optional "Selected work" section (defer to v2 if time-pressed):** three highlighted cards pulled from the projects listing — AquaPose first, then OKSort, then SARTAB. Purpose: give landing-page visitors a taste of the work without requiring a click to the listings page.

### Left rail
- Headshot (`profile.jpg` at project root). Placeholder OK initially.
- Social/contact buttons: GitHub, LinkedIn, Google Scholar, Email, CV.

---

## Projects listing page (`projects.qmd`)

Quarto `listing` with `type: grid`, auto-generated from `projects/` directory.

### Display
- `grid-columns: 3` on desktop, collapses to 1 on mobile
- Sort: `date desc` (newest first)
- Show on each card: image, title, description, categories, date
- `categories: true` to enable tag-based filtering in the sidebar

### Explicit sort override
Since AquaPose is the flagship and sort is by date, ensure date fields on project pages are set so AquaPose lands first in the grid. If dates naturally land it there, no action needed. If not, either nudge dates or set `sort: false` and rely on explicit ordering.

---

## Individual project pages

Structure per page, following the convention in CLAUDE.md:

### Front matter
```yaml
---
title: "Project Name"
description: "One-sentence card description (see below for each project)."
date: YYYY-MM-DD
image: hero.png
categories: [...]
---
```

### Body (400–700 words per page)
1. **Hook** — one sentence expanding the description; lead with capability or result.
2. **The problem** — 2–3 sentences framed for an industry reader (scale, cost, throughput, reproducibility). Where possible, name the general computational problem first and the biological context second.
3. **What I built** — the technical contribution. Concrete. Figures where available.
4. **Results / impact** — numbers. Downloads, benchmark scores, deployment scale, continuous operating duration, anything quantifiable.
5. **Stack** — one-liner at bottom. "Python, PyTorch, OpenCV, NumPy" etc. Recruiters ctrl-F for these.
6. **Links block** — GitHub, PyPI, paper, docs. Flat bulleted list, bottom of page.

### Handling the AquaPose/OKSort overlap
Both pages should include one sentence acknowledging the relationship: OKSort is a standalone component of the AquaPose pipeline, packaged separately because it generalizes beyond this application. On the AquaPose page: link to OKSort where its role is introduced. On the OKSort page: link to AquaPose as the "how it's used in context" reference.

---

## Project content specs

All five projects are pip-installable Python libraries with public GitHub repos and documentation. These specs give Claude Code enough content to scaffold placeholder pages; Tucker will fill in the bodies.

### 1. AquaPose (flagship)
- **Card description:** A 3D multi-animal pose tracking pipeline for refractive multi-camera arrays, combining geometry-only cross-view association with sliding-window re-matching to stay accurate across hours of footage under heavy occlusion.
- **Categories:** `computer-vision`, `3d-reconstruction`, `multi-object-tracking`, `pose-estimation`
- **Key numbers to surface on the page:**
  - 12-camera synchronized array
  - 9 near-identical subjects in validation
  - 1 identity swap across 9,450 frames on a deliberately challenging 5-minute benchmark
  - 5.8-hour continuous recording with no systematic degradation (628,000 frames, 24.5M detections)
  - Median reprojection error 2.82 pixels
- **Technical highlights:** two-stage detection (oriented bounding box → pose), iterative pseudo-labeling for training efficiency, thin-plate-spline elastic augmentation for high-curvature poses, geometry-only cross-view re-association on sliding windows (contrast with standard lock-in approaches), multi-camera triangulation under Snell's law refraction.
- **Position:** first on the grid (flagship).

### 2. OKSort
- **Card description:** A multi-object tracker that replaces bounding-box IoU with keypoint similarity (OKS), outperforming box-based and appearance-based trackers on deformable subjects while staying an order of magnitude faster than appearance-based methods.
- **Categories:** `multi-object-tracking`, `pose-estimation`
- **Key numbers to surface:**
  - Highest HOTA score vs. 7 competing trackers (including appearance-based)
  - ~10× faster than appearance-based methods
  - ~2× faster than KeySort (only other keypoint-based tracker in the literature)
- **Technical highlights:** extends the SORT/ByteTrack/OC-SORT predict-match-update paradigm with OKS-based association; Kalman filter tracks all 6 keypoints jointly rather than bounding box; built for deformable/elongated subjects where box IoU poorly approximates body overlap.
- **Framing note:** emphasize transferability — any domain doing keypoint detection on deformable subjects (sports, medical, robotics, pedestrians) has the same problem OKSort solves.

### 3. AquaMVS
- **Card description:** Dense multi-view stereo reconstruction built on state-of-the-art feature matching (RoMa V2), extended with a refractive camera model for sub-millimeter environmental reconstruction through air-water interfaces.
- **Categories:** `computer-vision`, `3d-reconstruction`
- **Key numbers to surface:**
  - 14.3 million points in a reconstruction
  - <1% agreement with known tank measurements
  - Sub-millimeter frame-to-frame consistency
- **Technical highlights:** integration of RoMa V2 dense feature matching with a refractive projection pipeline; shares the AquaCal refractive model so environmental and animal data are in one coordinate system.

### 4. AquaCal
- **Card description:** A refraction-aware multi-camera calibration package that integrates Snell's law directly into bundle adjustment, achieving depth-independent sub-millimeter accuracy where standard pinhole models produce 20+ cm errors.
- **Categories:** `computer-vision`, `3d-reconstruction`
- **Key numbers to surface:**
  - Sub-millimeter reconstruction error on a submerged calibration object of known dimensions
  - ~20 cm error from standard pinhole model at tank bottom (for contrast)
  - Depth-independent accuracy vs. depth-growing error in the pinhole baseline
- **Technical highlights:** Snell's law embedded in the bundle adjustment objective; replaces the pinhole camera model rather than post-correcting. Foundation for AquaMVS and AquaPose (all three share one projection model).

### 5. SARTAB
- **Card description:** Real-time action detection deployed to a $153 Coral TPU edge unit, using quantized neural networks to monitor behavior continuously and trigger time-sensitive data collection workflows.
- **Categories:** `computer-vision`, `edge-ml`
- **Key numbers to surface:**
  - $153 total hardware cost per unit
  - 32 tissue samples collected in-window to date
  - Continuous 24/7 operation
- **Technical highlights:** model quantization for Coral TPU deployment; region-of-interest cropping + occupancy-based heuristic to skip tracking; end-to-end workflow (detection → decision → email notification with video attachment → human-in-the-loop action). Emphasize the edge-deployment engineering story — this is the "I ship things that run on real hardware in production" project.

---

## Tag vocabulary

Five tags across the shortlist:
- `computer-vision`
- `3d-reconstruction`
- `multi-object-tracking`
- `pose-estimation`
- `edge-ml`

`open-source` is intentionally omitted as a tag (it would apply to every project, diluting its signal). Instead, each project page surfaces its open-source status via a concrete links block at the bottom (GitHub, PyPI, docs).

---

## Deferred / pending decisions

- **CV format:** rendered page vs. linked PDF. Tucker doesn't have a polished CV yet; revisit before launch. Navbar should currently link to a placeholder (`cv.qmd` with "Coming soon" or similar) so the nav structure is stable.
- **Custom domain:** Tucker has purchased a domain; swap from the `github.io` default to the custom domain after initial deploy works. Requires `CNAME` file at project root and DNS configuration.
- **"Selected work" callout on landing page:** optional v2 enhancement. Launch without it; add if/when the rest of the site is polished.
- **Headshot:** placeholder acceptable at launch; replace with a real photo before sharing the site broadly.
- **Google Scholar link:** include on landing page if Tucker has a Scholar profile; otherwise omit.

---

## Implementation order (recommended)

1. Update `_quarto.yml` with full site config (navbar, theme, footer, site-url, OG metadata).
2. Update `index.qmd` with the `trestles` template, tagline, bio, and social links.
3. Create `projects.qmd` with the grid listing configuration.
4. Create `projects/<slug>/index.qmd` for all five projects with front matter and skeleton body (placeholder sections Tucker will fill in).
5. Create placeholder `cv.qmd`.
6. Add `.nojekyll` at project root.
7. Set `output-dir: docs` in `_quarto.yml` for GitHub Pages.
8. Initial commit, push, configure GitHub Pages to serve from `/docs` on `main`.
9. Once deploy works on the default `github.io` URL, add `CNAME` and configure DNS for the custom domain.
