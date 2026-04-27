# AI-Based and Hybrid Weather Models

This page indexes all weather models in this repository that use machine learning or artificial intelligence as a core part of the forecast process.

AI-based weather models have rapidly moved from research into operational use. This index groups them together to make discovery easier, since they are otherwise distributed across the repository by operator and model type (global/regional, deterministic/ensemble).

---

## How AI is being used in operational NWP

Different meteorological centres are integrating AI into weather prediction in different ways. Four patterns have emerged:

1. **Standalone AI forecast models.** A trained neural network replaces the physics-based forecast integration entirely. The model starts from a physics-derived analysis (initial conditions) and rolls forward autoregressively using learned dynamics. *Examples: AIFS Single, AIGFS, GEML.*

2. **AI-based ensembles.** Same as above, but trained probabilistically to produce ensemble members, usually by injecting random noise during inference. *Examples: AIFS ENS, AIGEFS.*

3. **Hybrid physics–AI systems.** The physics-based model continues to run the forecast, but AI predictions guide or constrain parts of it — typically through spectral nudging toward AI-generated large-scale fields. *Examples: GDPS-EXP (experimental).*

4. **Grand ensembles combining physics and AI members.** Members from a physics-based ensemble and an AI-based ensemble are combined into a single larger ensemble for probabilistic guidance. *Examples: HGEFS.*

These approaches reflect genuinely different theories about how AI should enter operational forecasting. The physics-based NWP community has not converged on a single answer, and this repository documents multiple approaches side by side.

---

## Deterministic AI global models

### [AIFS Single (ECMWF)](./models/nwp_models/global/eu/aifs-single.md)
- **Operator:** ECMWF
- **Operational since:** February 25, 2025 (v1.0.0); currently v1.1.0 (August 27, 2025)
- **Next upgrade:** v2 scheduled for May 12, 2026
- **Approach:** Standalone AI, MSE-trained, encoder–processor–decoder architecture with graph neural networks and transformer
- **Resolution:** ~0.25° (Open Data)
- **Forecast length:** Up to 15 days

### [AIGFS (NOAA)](./models/nwp_models/global/usa/aigfs.md)
- **Operator:** NOAA / NCEP
- **Operational since:** December 17, 2025 (replaced EAGLE SOLO)
- **Approach:** Standalone AI (GraphCast-derived), emulates GFS behavior
- **Resolution:** 0.25°
- **Forecast length:** Up to ~16 days

### [GEML (ECCC)](./models/nwp_models/global/canada/gdps-geml.md)
- **Operator:** ECCC / Canadian Meteorological Centre
- **Status:** Experimental
- **Approach:** Standalone AI (GraphCast-derived), retrained on ERA5 + ECMWF HRES analyses; weights publicly distributed via HuggingFace
- **Resolution:** 0.25° (~28 km)
- **Forecast length:** 10 days
- **Note:** Also serves as the spectral nudging target for the experimental GDPS (GDPS-EXP), giving it a dual role as both a standalone product and a component of ECCC's hybrid forecasting system.

---

## Ensemble AI global models

### [AIFS ENS (ECMWF)](./models/ensemble_models/global/eu/aifs-ens.md)
- **Operator:** ECMWF
- **Operational since:** July 1, 2025
- **Next upgrade:** v2 scheduled for May 12, 2026
- **Approach:** Standalone AI, CRPS-trained for probabilistic forecasting; ensemble members generated via noise injection
- **Resolution:** ~0.25° (Open Data); native ~30 km
- **Members:** 51 (1 control + 50 perturbed)

### [AIGEFS (NOAA)](./models/ensemble_models/global/usa/aigefs.md)
- **Operator:** NOAA / NCEP
- **Operational since:** December 17, 2025 (replaced EAGLE Ensemble)
- **Approach:** Standalone AI, ensemble companion to AIGFS
- **Resolution:** ~0.25°

---

## Hybrid physics–AI systems

### [HGEFS (NOAA)](./models/ensemble_models/global/usa/hgefs.md)
- **Operator:** NOAA / NCEP
- **Operational since:** December 17, 2025
- **Approach:** Grand ensemble combining 31 GEFS (physics) members with 31 AIGEFS (AI) members
- **Total members:** 62
- **Note:** Does not replace either GEFS or AIGEFS; provides a larger, more diverse ensemble

### [GDPS-EXP (ECCC) — experimental](./models/nwp_models/global/canada/gdps-exp.md)
- **Operator:** ECCC / Canadian Meteorological Centre
- **Status:** Experimental; expected to replace operational GDPS in 2026 per ECCC's own statement
- **Approach:** GEM physics model spectrally nudged toward GEML at large scales (>2750 km) in the mid-troposphere (250–850 hPa); physics handles everything else
- **Resolution:** ~15 km, 84 levels, 10-day forecast 2× daily
- **Note:** Beyond AI-physics hybridization, v9.0.9 includes substantial updates to data assimilation (4DEnVar with 256-member LETKF backgrounds from GEPS 8.0.0), sea ice physics (CICE 6.2.0), and other components.

---

## Experimental AI productionizations

The following systems are productionizations of research AI architectures that are run experimentally by NWP centres but have not (or have not yet) become operational forecast products.

### [GraphCastGFS (NOAA)](./models/nwp_models/global/usa/graphcastgfs.md)
- **Operator:** NOAA / NCEP
- **Status:** Experimental
- **Approach:** Productionization of Google DeepMind's GraphCast architecture, fine-tuned on GDAS+ERA5 data
- **Lineage:** Predecessor to the operational AIGFS

### [FourCastNetGFS (NOAA)](./models/nwp_models/global/usa/fourcastnetgfs.md)
- **Operator:** NOAA / NCEP
- **Status:** Experimental
- **Approach:** Productionization of NVIDIA's FourCastNet architecture using Spherical Fourier Neural Operators
- **Lineage:** No operational counterpart announced

---

## Models consuming AI-based inputs

Some operational models do not themselves use AI but incorporate AI-model outputs as inputs to their blending or post-processing logic.

### [NBM v5.0 (NOAA)](./models/nwp_models/regional/usa/nbm.md)
As of NBM v5.0 (operational April 15, 2026), the National Blend of Models incorporates ECAIFS (ECMWF's AI/IFS hybrid) and AIGFS as inputs for temperature, wind speed, and QPF products.

---

## Architectural lineages

Most operational AI weather models trace back to a small number of research architectures. This section maps the relationships:

### GraphCast lineage (Google DeepMind, 2023)
The GraphCast research architecture has been productionized by both NOAA and ECCC, with each centre fine-tuning it on different reanalysis and analysis data:

- **GraphCast** (research) → **[GraphCastGFS](./models/nwp_models/global/usa/graphcastgfs.md)** (NOAA experimental) → **[AIGFS](./models/nwp_models/global/usa/aigfs.md)** (NOAA operational)
- **GraphCast** (research) → **[GEML](./models/nwp_models/global/canada/gdps-geml.md)** (ECCC experimental)

The two operational lineages share architecture and a 13-pressure-level vertical structure but differ in training data, fine-tuning procedures, and operational status.

### FourCastNet lineage (NVIDIA, 2022)
- **FourCastNet** (research) → **[FourCastNetGFS](./models/nwp_models/global/usa/fourcastnetgfs.md)** (NOAA experimental, no operational descendant announced)

### ECMWF in-house architecture
- **[AIFS Single](./models/nwp_models/global/eu/aifs-single.md)** and **[AIFS ENS](./models/ensemble_models/global/eu/aifs-ens.md)** use ECMWF's own encoder–processor–decoder architecture with attention-based GNN encoder/decoder and sliding-window transformer processor. Not derived from GraphCast or FourCastNet.

---

## What this page does not cover

- Research-only AI models with no public data (e.g., Pangu-Weather in its original research form) unless they have been productionized by an NWP centre.
- AI post-processing and statistical correction systems that operate downstream of physics-based forecasts without participating in the forecast integration itself.
- Climate reanalysis ML applications.

---

## Further reading

For background on the transition from physics-based to AI and hybrid NWP, ECMWF Newsletters 181 (AIFS ensembles) and 185 (AIFS ENS operational, IFS Cycle 50r1) are useful starting points. For the Canadian hybrid approach, Husain et al. (2024) on AI-based spectral nudging via arXiv (arXiv:2407.06100) is the primary reference. For NOAA's direction, the NOAA press release on AI-driven global weather models (December 2025) covers AIGFS, AIGEFS, and HGEFS in one place.
