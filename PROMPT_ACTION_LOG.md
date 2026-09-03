# Prompt Action Log

## 2026-06-26

### Prompt

User asked: "This repo is behind on some of the standard updates we've been making to other template repos like it needs the agents.md file and the prompt log and the ESIIL branding. Can you do a comprehensive update of the postdoc oasis so that it matches the working group oasis in style and setup but still geared toward postdocs instead of working groups. CU-ESIIL/Working_group_OASIS"

### Files and folders inspected

- Local Postdoc_OASIS repository structure
- Working_group_OASIS reference repository
- `mkdocs.yml`
- `README.md`
- `.github/workflows/`
- `docs/`
- `docker/`

### Actions taken

- Added `AGENTS.md` with postdoc-specific agent guidance.
- Added this prompt action log.
- Updated MkDocs navigation, branding paths, Material theme settings, CSS, and ESIIL visual styling.
- Reworked the homepage around the same repository-plus-website model used by Working_group_OASIS, adapted for postdoctoral researchers.
- Added postdoc work-plan and "how this postdoc project works" pages.
- Added ESIIL and team resource pages, public-facing site guidance, cloud triangle guidance, cite/reuse guidance, and practical GitHub/storage instructions.
- Added semantic image slots, process gallery folders, generated include files, and image slot generation scripts.
- Added non-blocking site health checks and a local template integrity check.
- Moved the JupyterLab runtime folder from `docker/` to `containers/` and updated the Docker image workflow.
- Updated GitHub Pages automation to generate image references, generate the site health report, and build with strict MkDocs before deploying.
- Added local artifact ignores for `.DS_Store`, `site/`, virtual environments, Node modules, and test results.
- Removed tracked `.DS_Store` files from the template.

### Verification

- Ran `python3 scripts/generate_image_slots.py`.
- Ran `python3 scripts/site_health.py`; remaining warnings are expected template placeholders.
- Ran `python3 scripts/check_template.py`.
- Ran `/private/tmp/postdoc_oasis_mkdocs_venv/bin/mkdocs build --strict --clean`.
- Confirmed strict build succeeds after fixing legacy missing asset links in older resource pages.

### Open questions and follow-up

- Confirm whether this template should keep the older resource guides in the main navigation or leave them available under the broader resources folder only.

## 2026-09-03 — Personalize homepage and work plan for the Yu Peng postdoc project

### Prompt

Adapt the Postdoc OASIS scaffold into Yu Peng's personal research site: update project
framing and research goals, correct personal/team information, and apply landmark labels.

### Changes

- `docs/index.md`
  - Fixed typos and grammar in the opening summary ("aims t integrates",
    "continetial-boudaries", "remians") and rewrote it as a project description.
  - Fixed the two homepage buttons: owner slug was `Eco_YuPeng` (underscore) instead of
    `Eco-YuPeng`, and the repository button pointed at a nonexistent
    `/doc/main/Index.md` path.
  - Expanded the Project Abstract with the knowledge gap and the sensor set actually used
    (PlanetScope, Sentinel-2, Landsat, ECOSTRESS).
  - Replaced the partially filled "Landmarks" section with a four-item **Research
    Objectives** list, each tagged with its PD landmark. The template's landmarks are
    labels, not milestones, so the milestone content was renamed to avoid the collision.
  - Rewrote **Start Here** as onboarding steps for a new collaborator instead of template
    instructions to the postdoc.
  - Renamed "Key Links To Replace" to **Key Links**, filled in the repository link, and
    added an Analysis Code row.
  - Set **Current Phase** to active analysis with a one-line status.
  - Updated the People table: added Yu Peng's institution, clarified the lead role,
    reordered by day-to-day involvement, fixed "Phd" to "PhD".
  - Replaced "Placeholder image ..." alt text on the four image slots with descriptive alt text.
  - Added `Landmark:` lines to Project Abstract (PD-B), Start Here (PD-B), Key Links
    (PD-C), Current Phase (PD-B), and People (PD-A).
- `docs/work-plan.md`
  - Added a `Landmark:` line under each phase heading (PD-A/B, PD-B/C, PD-D, PD-E, PD-F)
    plus a pointer to the landmark guide at the top.

### Not changed

- Navigation, theme, scripts, and workflows are untouched.
- Image slot folders are untouched; `repository-side`, `website-side`, `data`, `analysis`,
  and `outputs` still hold `placeholder.svg` and need real images uploaded.
- `[link]` placeholders remain in `docs/work-plan.md` and in three Key Links rows; these
  are tracked by the site health report.

### Verification

- Ran `python3 scripts/generate_image_slots.py`.
- Ran `python3 scripts/site_health.py`.
- Ran `python3 scripts/check_template.py`.
- Ran `mkdocs build --strict --clean`.

## 2026-09-03 — Fix broken image rendering on the site

### Prompt

Images were not loading on the published site. Diagnose and fix.

### Root cause 1: escaped snippet directives in `docs/index.md`

All six snippet includes on the homepage had been written with literal backslashes:

    --8\<-- "\_generated/slot_notes/hero.md"

instead of `--8<-- "_generated/..."`. The `pymdownx.snippets` extension therefore never
matched those lines, so `_generated/image_slots.md` — the file that defines every
`[slot-*]` reference target — was never included. With no link definition, Markdown
emitted the reference-style image lines as literal text, and the homepage showed
`![Placeholder image for the homepage overview][slot-hero]` verbatim instead of an image.
No image file was missing; nothing was ever referenced.

Note that `mkdocs build --strict` does NOT catch this: an unmatched snippet directive
degrades to plain text rather than failing the build. Local `mkdocs serve` is the only
reliable check.

Fixed as part of the homepage rewrite in the previous entry.

### Root cause 2: wrong gallery variant included on sub-pages

`scripts/generate_image_slots.py` emits each process gallery twice — `galleries/root/`
with bare `assets/...` hrefs, and `galleries/child/` with `../assets/...` hrefs — because
the gallery markup is raw HTML and MkDocs does not rewrite relative paths inside raw HTML.

With `use_directory_urls` (the default), only `index.md` is served at site root. Every
other top-level page is served one directory deep (`work-plan.md` -> `/work-plan/`), so it
needs the `child` variant. Three pages were including `root` and their gallery images
resolved to `/work-plan/assets/...` and 404'd.

- `docs/work-plan.md`: `galleries/root/outputs` -> `galleries/child/outputs`
- `docs/how-this-postdoc-project-works.md`: `root` -> `child` for `data`, `methods`,
  and `exploration`
- `docs/index.md`: unchanged, correctly uses `root`

Rule for future pages: `index.md` uses `galleries/root/...`; every other page uses
`galleries/child/...`.

### Also changed

- `mkdocs.yml`: added `exclude_docs: _generated/`. The generated fragments were being
  built as standalone orphan pages with unresolvable image paths. They are pulled into
  real pages by the snippets extension, which reads them from the filesystem, so
  excluding them from the page build is safe and removes twenty phantom pages.

### Verification

- Ran `python3 scripts/generate_image_slots.py`, `scripts/site_health.py`,
  `scripts/check_template.py`.
- Ran `mkdocs build --strict --clean`.
- Scanned every `<img>` in the built site and confirmed each relative `src` resolves to a
  real file; the only unresolved entries are the absolute-path logos in `404.html`, which
  are correct once served.
