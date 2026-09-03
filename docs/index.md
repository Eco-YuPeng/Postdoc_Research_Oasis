# Tracking Cover Crops at Scale: unlocking field-scale cover crop dynamics via AI-driven satellite harmonization


This is a postdoc research project led by Dr. Yu Peng, supported by the Environmental Data Science Innovation & Impact Lab (ESIIL). The aim of this project is to integrate multi-scale field observations, satellite data fusion, and spatial-temporal modeling to map cover cropping across the continental US. and to quantify the ecosystem services that agricultural conservation delivers. 

Since, it runs as an open-science system: a GitHub repository where this project is organized, analyzed, and versioned, and a public website where results are explained and shared with collaborators, mentors, and community audiences. The Repo and the website can acceess via following links.
![Homepage overview image][slot-hero]

[Open the GitHub repository](https://github.com/Eco-YuPeng/YuPeng_Postdoc_OASIS){.md-button}

## Research Abstract

Field-scale monitoring of cover crops is vital for assessing soil organic carbon (SOC) sequestration and greenhouse gas (GHG) dynamics across agroecosystems, yet reliable wall-to-wall information at that scale remains largely unavailable. Existing products are limited by coarse resolution, sparse ground truth, and inconsistent revisit across sensors, so the timing of establishment and termination — the part that matters most for carbon and nitrogen outcomes — is rarely resolved. This project leverages deep learning architectures to harmonize multi-sensor satellite observations (HLS,ECOSTRESS, NISAR), enabling high-resolution tracking of cover crop establishment, biomass accumulation, and termination dates across broad geographic domains. The resulting maps and time series are designed to feed directly into biogeochemical and policy-relevant assessments of agricultural conservation practice.


## Research Objectives

1. **Synthesizing ground truths via LLMs.** Generate a training and validation database of cover crop presence, timing, and species by combining field records, producer surveys, and LLM-assisted extraction from the published literature. *(PD-C)*
   
2. **Sensor harmonization.** Fuse multi-source optical, thermal, and earth-embedding observations into a consistent, gap-filled field-scale time series. *(Powered by the [FireRX ML model](https://github.com/j-gams/firerx_ml), credited to Dr. Cibele Amaral.)*
 
3. **Phenology retrieval.** Train and evaluate AI models that retrieve establishment, peak biomass, and termination dates at the field level. 
   
4. **Continental mapping and ecosystem services.** Scale the retrieval across the continental US and link the resulting maps to SOC and GHG outcomes for conservation assessment. 



## Research Resources

The repository has two connected layers. Top-level files configure the project and its automation. The `docs/` folder contains the website content. `mkdocs.yml` tells MkDocs how to turn that content into the public site. Analysis folders hold the working scientific materials that generate the results shown on the website.

| Items | Types | What usually provides there |
|------------------------|------------------------|------------------------|
| Data sources and metadata | Configure the project and keep shared repository guidance in one place | (1) global-scale yield responce to cover crops; (2) Field-scale soil GHG responces to cover crops |
| Tools and scripts | Notebooks and scripts | (1) Access cloud computuer-CyVerse ; (2)|
| `mkdocs.yml` | Workflows and reproducible analysis | Navigation, theme settings, plugins, and GitHub edit links |
| Working folders | Figures, tables, manuscripts, and other outputs | Data references, notebooks, scripts, workflows, figures, outputs, and reproducibility materials |

[Research Progress](work-plan.md){.md-button} [Data & Resources](how-this-postdoc-project-works.md#data){.md-button .md-button--secondary} [Science Sharing](community-care.md){.md-button .md-button--secondary}


## Early Process Gallery

Use this section to show how the project gets started without manually editing image links one by one.

--8<-- "_generated/galleries/root/start-here/index.md"



## Project Members

![Project identity and collaboration image][slot-group-photo]


| Name | Role | Institution | Responsibilities |
|------------------|------------------|------------------|------------------|
| [Yu Peng](https://esiil.org/about/yu-peng) | Postdoc Researcher | ESIIL | Project lead: data fusion, model development, analysis, and public reporting |
| [Cibele Amaral](https://cires.colorado.edu/people/cibele-hummel-do-amaral) | Project Supervisor | ESIIL | Oversees daily research operations and remote sensing integration |
| [Timothy Bowles](https://vcresearch.berkeley.edu/faculty/timothy-bowles) | Academic Mentor | UC Berkeley | Guidance on agroecology and academic development |
| [Lixin Wang](https://science.indianapolis.iu.edu/people-directory/people/wang-lixin.html) | Advisory Expert | IU Indianapolis | Long-term research continuity and hydroecology expertise |

--8<-- "_generated/image_slots.md"
