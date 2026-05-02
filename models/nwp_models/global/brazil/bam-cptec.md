# BAM (Brazilian Global Atmospheric Model)

## What this model is
The Brazilian Global Atmospheric Model (BAM) is a global numerical weather prediction (NWP) model developed and operated by CPTEC/INPE for medium-range weather forecasting.

BAM provides deterministic global forecasts and is part of Brazil's national operational weather prediction suite. It also serves as the atmospheric component of the Brazilian Earth System Model (BESM), used for seasonal climate prediction and climate change projections.

---

## Who runs it
- **Organization:** CPTEC/INPE (Center for Weather Forecasting and Climate Studies / National Institute for Space Research)
- **Country / region:** Brazil

---

## What area it covers
- **Coverage:** Global

---

## Basic details
- **Model type:** Deterministic global NWP
- **Model system / core:** Spectral dynamical core with monotonic two-time-level semi-Lagrangian scheme
- **Operational configuration:** TQ0666L64 (triangular truncation T666, 64 vertical levels)
- **Horizontal resolution:** ~20 km (T666 spectral truncation)
- **Vertical levels:** 64
- **Forecast length:** Up to ~11 days (264 hours)
- **Update frequency / cycles:** 1× daily (00 UTC)
- **Temporal output resolution:** 6-hourly

---

## What it provides
Deterministic global forecasts of core atmospheric variables, including:
- Temperature
- Wind
- Atmospheric pressure
- Humidity
- Precipitation

BAM output is used for medium-range forecasting and as background guidance within CPTEC's operational workflow.

---

## Data assimilation
BAM is initialized from analyses produced by **SMNA** (Sistema de Modelagem Numérica e Assimilação de dados — Numerical Modeling and Data Assimilation System), CPTEC's operational data assimilation system. SMNA pairs BAM with the **GSI (Gridpoint Statistical Interpolation)** variational data assimilation system, with development work on a hybrid 3DEnVar configuration documented in CPTEC research literature.

SMNA is the Brazilian counterpart to systems like NOAA's GDAS (which initializes GFS) — it is not a separate forecast model, but rather the analysis system whose output provides BAM's initial conditions.

---

## Data availability
- **Is the data free?** Yes
- **Is the data downloadable?** Yes
- **Data formats:** GRIB1 (note: GRIB edition 1, not GRIB2)
- **Official download location:**  
  https://dataserver.cptec.inpe.br/dataserver_modelos/bam/TQ0666L064/brutos/

Each forecast cycle directory contains:
- `.grb` — GRIB1 forecast data (~1.5 GB per timestep)
- `.ctl` — GrADS descriptor file
- `.idx` — GrADS index file
- `.inv` — wgrib inventory listing
- `.lst` — file listing
- `.icn` / `.inz` files at lead 0 — initial conditions / initialized analysis

Most users will want the `.grb` files for forecast data.

---

## Notes
- BAM data is currently published only for the 00 UTC model cycle.
- Forecast files are produced at 6-hourly intervals out to +264 h.
- Output is distributed in **GRIB1**, not GRIB2 — users with GRIB2-only pipelines may need additional decoder support (e.g. wgrib, pygrib, cfgrib all handle GRIB1).
- The model configuration and resolution may evolve over time as part of CPTEC's operational development.
- Public documentation on detailed model physics and data assimilation is limited; CPTEC has published research on a GSI-based hybrid 3DEnVar data assimilation system for BAM, but operational specifics are not extensively documented in English.

---

## Official information
- CPTEC/INPE model data server  
  https://dataserver.cptec.inpe.br/

### Key references
- Figueroa, S.N., et al. (2016). *The Brazilian Global Atmospheric Model (BAM): Performance for Tropical Rainfall Forecasting and Sensitivity to Convective Scheme and Horizontal Resolution.* Weather and Forecasting, 31(5). https://journals.ametsoc.org/view/journals/wefo/31/5/waf-d-16-0062_1.xml
