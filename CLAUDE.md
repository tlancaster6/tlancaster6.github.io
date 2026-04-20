# Project: Personal Portfolio Website

A Quarto-based personal website showcasing PhD research projects, aimed at industry hiring managers (tech, pharma/biotech, applied ML). The goal is accessibility — a non-academic reader should grasp what each project does and why it matters in under 30 seconds per project.

## Owner context

- **Field:** ML tools for advanced animal behavior analysis. Crossroads of CS, data science, ML, and biology.
- **Output:** open-source Python libraries and pipelines (computer vision, time-series ML on behavioral data).
- **Target roles:** industry (tech, pharma, biotech, applied-ML). NOT academia.
- **Audience priority:** industry hiring managers and recruiters first, collaborators second, academics third. If a framing choice trades academic precision for industry legibility, take the industry framing.

## Stack

- Quarto (static site generator)
- GitHub Pages deployment via `docs/` output directory
- Custom domain (configured via `CNAME` file at project root)
- Python / Jupyter notebooks may be embedded in project writeups
- Runs on Windows; owner uses VS Code with the Quarto extension

## Repository layout

- `_quarto.yml` — site config (navbar, theme, site-url). Edit here for site-wide changes.
- `index.qmd` — landing page. Uses the `trestles` about-page template.
- `projects.qmd` — listing page; auto-generates a grid of cards from `projects/`.
- `projects/` — one subdirectory per project, each containing an `index.qmd` and assets.
- `cv.qmd` or `cv.pdf` — CV / resume.
- `styles.css` — custom CSS overrides.
- `profile.jpg` — headshot used on the landing page.
- `docs/` — rendered output. Committed to git; served by GitHub Pages. Do not hand-edit.
- `.nojekyll` — required at repo root so GitHub Pages doesn't run Jekyll on the output.

## Common commands

- `quarto preview` — live-reload preview at localhost. Use while editing.
- `quarto render` — full build into `docs/`. Run before committing deploy-affecting changes.
- `quarto publish gh-pages` — alternative deploy path (publishes to a `gh-pages` branch). Currently using the `docs/` folder method instead; prefer that unless we decide to switch.

## Planning document

See @SITE_PLAN.md for the full site specification — page structure, project shortlist with card descriptions and key numbers, tag vocabulary, and implementation order. When working on content or structure, defer to SITE_PLAN.md. When it's silent on something, fall back to the conventions in this file. If SITE_PLAN.md and CLAUDE.md conflict, flag it rather than picking one.

## Content conventions

### Project writeups
Each project lives at `projects/<project-slug>/index.qmd` with front matter that includes `title`, `description`, `date`, `image`, and `categories`. The image and description are what show up on the projects listing grid, so they carry real weight.

Structure every project page in this order:
1. **One-sentence hook** — what the project does, in plain English, no jargon. A pharma hiring manager should understand it.
2. **The problem** — why this matters. Ideally framed in terms an industry reader cares about (scale, cost, reproducibility, throughput), not just academic novelty.
3. **What I built** — the technical contribution. Be concrete: libraries, models, datasets, pipelines. Link to GitHub repos and papers.
4. **Results / impact** — numbers where possible (users, downloads, citations, performance gains, time saved).
5. **Tech stack** — a short list. Recruiters scan for keywords.

Avoid: thesis-style background sections, literature reviews, equations without context, passive voice ("it was shown that..."), and "future work" sections.

### Writing voice
First person, active, direct. "I built X to solve Y" not "X was developed for Y." Short paragraphs. Technical depth is fine — the reader can be assumed to be smart — but never assume they share your subfield's vocabulary. If a term is standard in animal-behavior ML but not in general ML, define it on first use.

### Images and figures
Every project page needs at least one visual in the front matter `image:` field (this shows on the grid). Prefer real results — a behavior track, a pose estimation output, a benchmark plot — over stock imagery or logos.

## What not to do

- Don't add a "Publications" page as a navbar item. Publications should live on the CV and be linked from individual project pages where relevant. Industry readers don't browse pub lists.
- Don't add a blog unless explicitly asked. It becomes stale inventory.
- Don't change the `output-dir: docs` setting in `_quarto.yml` without flagging it — it's load-bearing for the GitHub Pages deploy.
- Don't hand-edit files in `docs/`. They're regenerated.
- Don't introduce new dependencies (new themes, extensions, JS libraries) without asking first. Quarto's built-ins are usually enough.

## Open questions / decisions pending

- Final domain name and when to swap from `yourname.github.io` default to custom domain.
- CV page: render from `.qmd` or host a PDF? (Leaning PDF linked from navbar for simplicity.)
- Final shortlist of projects (aiming for 3–6).
