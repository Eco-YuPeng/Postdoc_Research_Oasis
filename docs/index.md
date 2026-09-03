# Tracking Cover Crop at Scale

## Unlocking field-scale cover crop dynamics through deep learning-driven satellite harmonization

![Homepage overview image][slot-hero]

--8<-- "_generated/slot_notes/hero.md"

## Project Abstract

Field-scale monitoring of cover crops is vital for assessing soil organic carbon (SOC) sequestration and greenhouse gas (GHG) dynamics across agroecosystems, yet reliable wall-to-wall information at that scale remains largely unavailable. Existing products are limited by coarse resolution, sparse ground truth, and inconsistent revisit across sensors, so the timing of establishment and termination — the part that matters most for carbon and nitrogen outcomes — is rarely resolved. This project leverages deep learning architectures to harmonize multi-sensor satellite observations (PlanetScope, Sentinel-2, Landsat, and thermal data from ECOSTRESS), enabling high-resolution tracking of cover crop establishment, biomass accumulation, and termination dates across broad geographic domains. The resulting maps and time series are designed to feed directly into biogeochemical and policy-relevant assessments of agricultural conservation practice.

This is a postdoc research project led by Dr. Yu Peng, supported by the Environmental Data Science Innovation & Impact Lab (ESIIL). The aim of this project is to integrate multi-scale field observations, satellite data fusion, and spatial-temporal modeling to map cover cropping across the continental US. and to quantify the ecosystem services that agricultural conservation delivers. 

Since, it runs as an open-science system: a GitHub repository where this project is organized, analyzed, and versioned, and a public website where results are explained and shared with collaborators, mentors, and community audiences.

The Repo and the website can acceess via following links:

[Open the GitHub repository](https://github.com/Eco-YuPeng/YuPeng_Postdoc_OASIS){.md-button}

## Research Objectives

1. **Synthesizing ground truths via LLMs.** Generate a training and validation database of cover crop presence, timing, and species by combining field records, producer surveys, and LLM-assisted extraction from the published literature. *(PD-C)*
   
2. **Sensor harmonization.** Fuse multi-source optical, thermal, and earth-embedding observations into a consistent, gap-filled field-scale time series. *powered by Dr. Cibele Amaral*
 
3. **Phenology retrieval.** Train and evaluate AI models that retrieve establishment, peak biomass, and termination dates at the field level. *(PD-D, PD-E)*
   
4. **Continental mapping and ecosystem services.** Scale the retrieval across the continental US and link the resulting maps to SOC and GHG outcomes for conservation assessment. *(PD-E, PD-F)*



## Start Here

New to the project? These five steps orient a collaborator in about fifteen minutes.

1. Read the Project Abstract and Research Objectives above to understand the question and what the project intends to produce.
2. Review the [Work Plan](work-plan.md) for the current phase, active tasks, and open blockers.
3. Check the data inventory and access notes before requesting or moving any dataset.
4. Run or adapt one analysis workflow from the repository, and record what you decided and why.
5. Commit figures, tables, notes, and summaries so the work stays versioned and reproducible.


[Plan the work](work-plan.md){.md-button} [Document data and resources](how-this-postdoc-project-works.md#data){.md-button .md-button--secondary} [Set community expectations](community-care.md){.md-button .md-button--secondary}

## How This Repo Is Organized

The repository has two connected layers. Top-level files configure the project and its automation. The `docs/` folder contains the website content. `mkdocs.yml` tells MkDocs how to turn that content into the public site. Analysis folders hold the working scientific materials that generate the results shown on the website.

| Part of the repo | What it does | What usually belongs there |
|------------------------|------------------------|------------------------|
| Top-level files and folders | Configure the project and keep shared repository guidance in one place | `README.md`, `LICENSE`, workflows, containers, templates, environment setup, and repo-wide metadata |
| `docs/` | Stores the source content for the public website | Homepage text, summaries, methods, community-facing documentation, and website assets |
| `mkdocs.yml` | Controls how the site is rendered | Navigation, theme settings, plugins, and GitHub edit links |
| Working folders | Hold the science-in-progress | Data references, notebooks, scripts, workflows, figures, outputs, and reproducibility materials |

## Repository Side: Do The Research

![Repository side of the workflow][slot-repository-side]

--8<-- "_generated/slot_notes/repository-side.md"

Related landmarks: PD-C Data and access; PD-D Methods and workflows.

The repository is the working record of the project: it tracks what changed, why it changed, and how results were produced.

- Data sources and metadata
- Notebooks and scripts
- Workflows and reproducible analysis
- Meeting notes and decisions
- Figures, tables, manuscripts, and other outputs

## Website Side: Share The Research

![Website side of the workflow][slot-website-side]

--8<-- "_generated/slot_notes/website-side.md"

Related landmarks: PD-E Results and synthesis; PD-F Outputs and handoff.

The website turns the research record into a readable public report.

- Plain-language summaries
- Methods documentation
- Figures, maps, and visualizations
- Project updates and synthesis products
- Manuscripts, reports, dashboards, or educational materials

## How The Two Sides Connect

The repository and website are not separate products. When the postdoc updates data, analysis code, figures, or written summaries in GitHub, those changes can be rendered through the website. Commits are the bridge between doing the research and sharing the research.

## When This Postdoc Project Is Live

A postdoc project is live when:

- The research question is stated
- Data sources are linked or documented
- At least one analysis or workflow is runnable
- Outputs are committed to the repository
- The website explains what the project is doing and why it matters

For guidance on turning this scaffold into a public scientific record, see the [Public-Facing Site Guide](public-facing-site-guide.md).

## Early Process Gallery

Use this section to show how the project gets started without manually editing image links one by one.

--8<-- "_generated/galleries/root/start-here/index.md"

## Key Links

The resources collaborators should actually use. Replace any remaining `[link]` placeholder as soon as the resource exists.

Landmark: PD-C Data and access.

- Main Working Document: [link]
- GitHub Repository: [Eco-YuPeng/YuPeng_Postdoc_OASIS](https://github.com/Eco-YuPeng/YuPeng_Postdoc_OASIS)
- Analysis Code: [link]
- Data / Resources: [link]
- Outputs / Dashboard: [link]

## Current Phase

**Working Phase: Active analysis — data harmonization and ground-truth assembly.**

Current focus is building the multi-sensor fused time series over the test region and assembling the labelled cover crop database that model training depends on. Update this line whenever the phase changes (onboarding, data access, active analysis, writing, or handoff).

Landmark: PD-B Question and scope.

## People

![Project identity and collaboration image][slot-group-photo]

--8<-- "_generated/slot_notes/group-photo.md"

Landmark: PD-A People and roles.

| Name | Role | Institution | Responsibilities |
|------------------|------------------|------------------|------------------|
| Yu Peng | Postdoctoral Researcher | ESIIL, University of Colorado Boulder | Project lead: data fusion, model development, analysis, and public reporting |
| Cibele Amaral | Supervisor | ESIIL | Oversees daily research operations and remote sensing integration |
| Timothy Bowles | Mentor | UC Berkeley | Guidance on agroecology and academic development |
| Lixin Wang | PhD Advisor | IU Indianapolis | Long-term research continuity and hydroecology expertise |

--8<-- "_generated/image_slots.md"
