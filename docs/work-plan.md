# Work Plan

Landmark: all landmarks apply here, depending on the phase. See the [landmark guide](instructions/postdoc-landmarks.md).

## Onboarding

Landmark: PD-A People and roles; PD-B Question and scope.

**Kick-off meeting — August 28, 2026**

| Attendee | Role | Institution |
|---|---|---|
| Yu Peng | Postdoc, project lead | ESIIL |
| Cibele Amaral | Project supervisor | ESIIL |
| Timothy Bowles | Academic mentor | UC Berkeley |
| Lixin Wang | Advisory expert | IU Indianapolis |

- Introductions and roles → [Project Members](index.md#project-members)
- Roadmap set around the four [research objectives](index.md#research-objectives)
- Notes: `templates/meeting-notes/` in the repository

## Active Research

Landmark: PD-D Methods and workflows.

| Workstream | Objective | Repo | Status |
|---|---|---|---|
| Automated ground-truth labels | 1 — LLM-synthesized training data | [LLM_AutoExtracting_CC](https://github.com/Eco-YuPeng/LLM_AutoExtracting_CC) | Pipeline built; first end-to-end run next |
| HLS × ECOSTRESS fusion | 2 — sensor harmonization | [firerx_ml_CC](https://github.com/Eco-YuPeng/firerx_ml_CC) · [notebook](https://github.com/Eco-YuPeng/firerx_ml_CC/blob/main/app_py/CoverCrop_Fusion_v5_2.ipynb) | Pilot cube + phenology layers done |

- Compute: CyVerse (JupyterLab, VS Code); NASA Earthdata via `earthaccess`; Claude models via CyVerse AI-VERDE
- Storage: CyVerse Data Store `CoverCrop_Fusion/` (fused cube, caches, figures)
- Blocker: no labelled fields yet → greenness threshold in the phenology layers is provisional

### Progress 1 · Automated Extraction of Field Ground-Truth Labels

**Goal** — turn published cover crop field studies into labelled fields (where · when · which species · which cash crop) for training and validating the detection model.

**Pipeline** (one PDF in → rows out)

| Step | What happens | Who does it |
|---|---|---|
| 1. Parse | PDF → Markdown; locate Methods and Results | code |
| 2. Block 1 | Location, species, cash crop, planting/termination dates from Methods | regex + gazetteer; LLM only for leftovers |
| 3. Block 2 | Classify response types (yield / GHG / SOC / N); pull values from text, tables, or figure-referenced charts | LLM (vision only when figure-referenced) |
| 4. Normalize | Controlled vocabularies; response ratios | pure math, never LLM |
| 5. Validate | Species → GBIF / WFO; coordinates → GeoNames | external APIs |
| 6. Emit | One row per (treatment × species × year × response), each field tagged high / medium / low confidence with source note; never fabricates | code |

- Built on ESIIL's [LLM_lesson_exemplar](https://github.com/CU-ESIIL/LLM_lesson_exemplar) agentic-repo pattern (`AGENTS.md`, run log)
- Model calls go through an OpenAI-compatible API → portable across providers

**Label schema**

| Design choice | Decision |
|---|---|
| One row | (site, field/plot, cover-crop season, treatment) |
| Negatives | No-cover-crop control plots kept as explicit negatives |
| Season key | `cc_season` = cover crop **termination** year |
| Required fields | location · CC year and period on the surface · species / type (legume vs. non-legume, interseeded) · cash crop with planting / harvest dates · biomass when available |
| Spatial confidence | Tiered — most records are small plots < 30 m pixel with CC / control adjacent |
| Coordinate QC | Doubtful coordinates re-checked against multiple sources |

- Seed data: hand-built GHG table (295 records / 40 papers) + yield table (1,026 records / 91 papers)

**Status & next**

- ✅ Scaffold, schema (GHG in long format), gazetteer, validation, model-calling stages, multi-row extraction, CLI runner, tests passing
- ⬜ First end-to-end run on one paper → field-by-field review
- ⬜ Batch the ~94-paper corpus (auto-download open access; list paywalled for manual retrieval)
- ⬜ Merge with hand-built tables → publish labelled dataset with confidence tiers
- ⬜ Use labels to tune the greenness threshold in Progress 2

### Progress 2 · HLS × ECOSTRESS Fusion Pilot

**Setup**

| Item | Value |
|---|---|
| ROI | 10 × 10 km near Purdue ACRE farm, West Lafayette, IN (−86.995°, 40.482°; UTM 16N) |
| Period | Sep 2024 – Oct 2025 · 27 × 16-day windows |
| Cropland mask | USDA CDL corn + soybean · 54% of ROI |

| Source | Product | Variable | Resolution |
|---|---|---|---|
| NASA HLS (Landsat 8/9 + Sentinel-2) | HLSL30 / HLSS30 v2.0 | NDVI, NDTI (Fmask-masked) | 30 m |
| NASA ECOSTRESS | ECO_L3T_JET | Daily ET (QC-masked) | 70 m |
| USDA NASS | Cropland Data Layer | Corn / soybean mask | 30 m |

**Fused cube**

- 116 HLS observation days → 16-day composites (NDVI median, ET mean) → per-pixel linear gap-fill
- Grids derived from the ROI polygon, so 30 m and 70 m layers stay registered over the full area
- Valid coverage: NDVI / NDTI 99.7% · ET 98.6%

![Nine-panel QC view of the fused cube](assets/images/results/fused_cube_qc.png)

*Figure 1. Fused cube QC. Top: NDVI summer peak (2025-07-10), NDVI mid-winter (2025-01-15), NDTI mid-winter. Middle: spring ET (2025-05-07), CDL cropland mask, winter NDVI × NDTI plane. Bottom: NDVI / NDTI / ET trajectories; off-season (blue) and termination window (red) shaded.*

| Check | Value | Reads as |
|---|---|---|
| Summer cropland NDVI | 0.84 | healthy main crop |
| Winter cropland NDVI | 0.23 | mostly bare; green patches = candidate cover crops |
| Winter cropland NDTI | 0.10 | residue-sensitive range |
| Winter NDVI × NDTI plane | green vs. residue separate | ambiguity NDVI alone cannot resolve |
| ET trajectory | ~0 in winter, sharp rise Mar–May | thermal signal earns its place at termination |

**Phenology layers for FireRX** (time axis compressed to five per-pixel scalars)

| Layer | Measures | Cropland mean |
|---|---|---|
| `n_green` | Off-season windows with NDVI > threshold | 1.11 |
| `ndvi_int` | Green-days accumulated over the off-season | 0.35 |
| `up_slope` | Steepest spring green-up | 0.05 |
| `term_drop` | Sharpest drop in the termination window | 0.04 |
| `peak_win` | When greenness peaked (harvest → planting) | 6.6 |

![Layer diagnostics](assets/images/results/layer_diagnostics.png)

*Figure 2. Layer diagnostics. Left: `n_green` per field (whole-field patches = real signal). Center: `n_green` distribution — 15.9% of cropland green for > 2 off-season windows. Right: threshold sensitivity of the off-season NDVI maximum; provisional threshold 0.30 marked.*

- No redundant layers (max |r| = 0.79, `up_slope` / `term_drop`) → all five kept
- ~16% of cropland stayed green > 2 windows — plausible adoption rate for this part of Indiana

**Caveats**

- Greenness threshold 0.30 is provisional until labelled fields exist
- Part of the cube is interpolated, not measured; gap-filled winter windows carry less evidence
- Mid-winter ET ≈ 0 for both cover crop and bare soil — thermal is most informative Apr–May

**Next**

- ⬜ Export five layers as GeoTIFFs → FireRX `batch_align_raster`
- ⬜ Add ECOSTRESS LST with overpass-time normalisation
- ⬜ Repeat on additional ROIs before scaling to regional tiles
