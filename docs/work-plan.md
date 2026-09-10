# Work Plan

Landmark: all landmarks apply here, depending on the phase. See the [landmark guide](instructions/postdoc-landmarks.md).

## Onboarding

Landmark: PD-A People and roles; PD-B Question and scope.

- Confirm project goals and research questions
- Identify mentors, collaborators, and communication channels
- Identify key data, compute, and storage needs
- Decide what belongs in GitHub versus persistent storage

## Project Scoping

Landmark: PD-B Question and scope; PD-C Data and access.

- Project summary: [link]
- Research question: [link]
- Data inventory: [link]
- Decision notes: [link]
- Mentor or collaborator notes: [link]

## Active Research

Landmark: PD-D Methods and workflows.

Current focus is **Objective 2 (sensor harmonization)**: building a gap-free, evenly spaced fused image cube from HLS optical data and ECOSTRESS thermal data, then compressing it into per-pixel phenology layers that the [FireRX ML](https://github.com/j-gams/firerx_ml) sampler can ingest. The pilot below is the first end-to-end run of that pipeline.

- Active work: [CoverCrop_Fusion_v5_2.ipynb](https://github.com/Eco-YuPeng/firerx_ml_CC/blob/main/app_py/CoverCrop_Fusion_v5_2.ipynb) (HLS × ECOSTRESS fusion, phenology layers, layer diagnostics)
- Data and code updates: [firerx_ml_CC repository](https://github.com/Eco-YuPeng/firerx_ml_CC); fused cube and per-scene caches archived in the CyVerse Data Store (`CoverCrop_Fusion/`)
- Compute: CyVerse JupyterLab container; NASA Earthdata (`earthaccess`) for HLS and ECOSTRESS access
- Open questions or blockers: no labelled cover-crop fields yet for this ROI, so the greenness threshold that drives the phenology layers is still provisional (see below)

### Preliminary Results: HLS × ECOSTRESS Fusion Pilot

**Study area and period.** A 10 × 10 km square near the Purdue ACRE farm, West Lafayette, Indiana (center −86.995°, 40.482°; UTM 16N), covering September 2024 through October 2025 — one full off-season between the 2024 harvest and the 2025 main crop. Cropland is defined from the USDA Cropland Data Layer (corn and soybeans), which covers 54% of the ROI.

**Inputs.**

| Source | Product | Variable | Native resolution |
|---|---|---|---|
| NASA HLS (Landsat 8/9 + Sentinel-2 A/B) | HLSL30 / HLSS30 v2.0 | NDVI and NDTI (Fmask-masked for cloud, shadow, snow, water) | 30 m |
| NASA ECOSTRESS | ECO_L3T_JET | Daily evapotranspiration (ET), QC-masked | 70 m |
| USDA NASS | Cropland Data Layer | Corn / soybean mask | 30 m |

**What was built.** 116 usable HLS observation days were composited into 27 consecutive 16-day windows (NDVI by median, ET by mean), and remaining temporal gaps were filled by per-pixel linear interpolation. Both reference grids are derived from the ROI polygon itself — not from whichever scene downloads first — so the 30 m and 70 m layers stay registered and span the full study area. The resulting cube is 99.7% valid for NDVI/NDTI and 98.6% valid for ET.

![Nine-panel quality-control view of the fused cube: NDVI in summer and winter, NDTI in winter, spring ET, CDL cropland mask, winter NDVI × NDTI plane, and NDVI / NDTI / ET trajectories over the season](assets/images/results/fused_cube_qc.png)

*Figure 1. Visual QC of the fused cube. Top: NDVI at summer peak (2025-07-10) and mid-winter (2025-01-15), and NDTI in mid-winter. Middle: spring ET (2025-05-07, 99% valid), the CDL cropland mask, and the winter NDVI × NDTI plane for cropland pixels. Bottom: seasonal trajectories of NDVI, NDTI, and ET for cropland versus other land, with the off-season (blue) and termination window (red) shaded.*

Key checks all pass: summer cropland NDVI averages 0.84 (bright, healthy main crop), mid-winter cropland NDVI drops to 0.23, and winter cropland NDTI sits at 0.10 — the residue-sensitive range. In the winter NDVI × NDTI plane, green pixels (candidate cover crops, right of the dashed line) separate from residue-covered bare soil (top-left), which is exactly the ambiguity NDVI alone cannot resolve. The ET trajectory is flat and near zero through winter and rises sharply from March into the termination window, consistent with the expectation that thermal data earns its place in April–May rather than mid-winter.

**Phenology layers for FireRX.** Because the FireRX sampler has no time dimension, the 27-step cube was compressed into five per-pixel scalars, each a hypothesis about what separates a cover-cropped field from a bare one:

| Layer | What it measures | Cropland mean |
|---|---|---|
| `n_green` | Number of off-season windows with NDVI above the greenness threshold | 1.11 |
| `ndvi_int` | Green-days accumulated over the off-season | 0.35 |
| `up_slope` | Steepest spring green-up (window to window) | 0.05 |
| `term_drop` | Sharpest drop during the termination window | 0.04 |
| `peak_win` | When greenness peaked between harvest and planting | 6.6 |

![Three-panel layer diagnostics: map of n_green aggregated to fields, log histogram of n_green over cropland pixels, and a threshold sensitivity curve for the off-season NDVI maximum](assets/images/results/layer_diagnostics.png)

*Figure 2. Layer diagnostics. Left: `n_green` aggregated to field objects (whole-field patterns indicate real signal; speckle would indicate noise). Center: distribution of `n_green` over cropland pixels — 15.9% of cropland was green for more than two off-season windows. Right: share of cropland whose off-season NDVI maximum exceeds a given threshold; the current provisional threshold of 0.30 is marked.*

No pair of layers is redundant (largest correlation: `up_slope` / `term_drop`, r = 0.79), so all five are kept. The `n_green` map shows coherent whole-field patches rather than speckle, and roughly 16% of cropland stayed green through more than two off-season windows — a plausible cover-crop adoption rate for this part of Indiana.

**Caveats to keep in mind.** The greenness threshold (NDVI > 0.30) is provisional and will be tuned once labelled fields exist for this ROI. Part of the cube is interpolated rather than measured, and gap-filled winter windows carry less evidence than clear ones. Mid-winter ET is close to zero for both cover crops and bare soil, so the thermal signal is most informative around termination.

**Next steps.**

- Export the five layers as GeoTIFFs and point the FireRX `batch_align_raster` config at them
- Assemble labelled cover-crop fields for the ROI (Objective 1) and sweep the greenness threshold against them
- Add ECOSTRESS LST with overpass-time normalisation as a second thermal channel
- Repeat the pipeline on additional ROIs before scaling to regional tiles

## Synthesis And Writing

Landmark: PD-E Results and synthesis.

- Results summary: [link]
- Figures or tables: [link]
- Manuscript, report, or product draft: [link]
- Reuse and citation notes: [link]

## Outputs And Handoff

Landmark: PD-F Outputs and handoff.

For outputs from the postdoc project, list the full authors or contributors for each product. If you list a paper, presentation, dataset, dashboard, package, report, or educational material, include enough information that future readers can understand who contributed and how to cite or reuse it.

- Final outputs: [link]
- Manuscripts or products: [link]
- Archive resources: [link]
- Handoff notes: [link]

![Placeholder image representing final outputs and synthesis products][slot-outputs]{ .slot-square-image }

--8<-- "_generated/slot_notes/outputs.md"

### Outputs Gallery

--8<-- "_generated/galleries/child/outputs/index.md"

--8<-- "_generated/image_slots.md"
