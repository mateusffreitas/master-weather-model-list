# ICON-2I (Italy – High Resolution)

## What this model is
ICON-2I is a **high-resolution, convection-permitting regional numerical weather prediction (NWP) system** based on the ICON modeling framework and operated in Italy.

It represents the Italian implementation of ICON, designed to resolve small-scale phenomena such as deep convection, intense precipitation, and local wind systems over complex terrain.

---

## Who runs it
- **Organization:** Agenzia ItaliaMeteo, in collaboration with Arpae Emilia-Romagna and Cineca (HPC infrastructure)
- **Country / region:** Italy
- **Model core developed by:** Deutscher Wetterdienst (DWD)

---

## What area it covers
- **Coverage:** Entire Italian territory and surrounding areas (extends into neighbouring countries and the central Mediterranean)
- **Horizontal resolution:** **2.2 km**
- **Domain:** Single national domain, approximately 3°E–21°E and 35°N–47.5°N

---

## Basic details
- **Model type:** Regional deterministic NWP (non-hydrostatic, convection-permitting)
- **Model system:** ICON
- **Configuration:** Since 26 May 2025, the operational chain runs the **MICROFISICA-NEW** configuration, which updates the microphysics parameterization (notably supersaturation handling in convective updrafts and raindrop size distribution) to reduce overestimation of convective precipitation peaks under weak synoptic forcing. The previous configuration (model version 2.6.5.1) was used until that date.
- **Horizontal resolution:** 2.2 km
- **Forecast length:** Up to **72 hours** (main deterministic run)
- **Primary update times:** 00 and 12 UTC

The ICON-2I chain runs in three configurations:

| Configuration | Type | Forecast range | Initial times (UTC) |
|---|---|---|---|
| ICON-2I | deterministic | +72 h | 00, 12 |
| ICON-2I-RUC | deterministic, rapid-update | +24 h | 03, 06, 09, 15, 18, 21 |
| ICON-2I-EPS | ensemble (20 members) | +51 h | 21 |

---

## Data assimilation
Initial conditions are generated using the **KENDA-LETKF** ensemble data assimilation system (40 members + deterministic run) with **hourly assimilation cycles** employing Incremental Analysis Update (IAU) and Relaxation to Prior Spread (RTPS).

Assimilated observations include:
- surface observations (SYNOP — wind and surface pressure)
- radiosoundings (TEMP)
- aircraft observations (AMDAR, AIREP, ACARS)
- national radar network data:
  - reflectivity volumes (assimilated through KENDA)
  - radial wind (assimilated through KENDA)
- radar-derived precipitation fields via **Latent Heat Nudging (LHN)**, using the composite of all national radars

Assimilation of radar volumes in combination with LHN has been operational since 17 April 2024. Assimilation is performed up to approximately **200 hPa**.

---

## Boundary and initial conditions
- **Lateral boundary conditions:** ECMWF IFS-HRES
- **Initial background:** ICON-based ensemble provided through KENDA-LETKF

---

## Data availability
- **Is the data free?** Yes
- **Is the data downloadable?** Yes
- **Data formats:** GRIB2
- **Direct download (MeteoHub, raw cycle archives):**
  - ICON-2I (surface and pressure levels): https://meteohub.agenziaitaliameteo.it/nwp/ICON-2I_SURFACE_PRESSURE_LEVELS/
  - ICON-2I parameter list: https://meteohub.agenziaitaliameteo.it/nwp/ICON-2I_SURFACE_PRESSURE_LEVELS/ICON_2I_shortname_parameter_index.txt
  - ICON-2I-RUC: https://meteohub.agenziaitaliameteo.it/nwp/ICON_2I_RUC/
  - ICON-2I-RUC parameter list: https://meteohub.agenziaitaliameteo.it/nwp/ICON_2I_RUC/ICON_2I_RUC_shortname_parameter_index.txt
- **Open data catalog (dataset metadata and curated subsets):**
  https://dati.agenziaitaliameteo.it/dataset/previsioni-meteorologiche-modello-icon-2i

---

## Notes
ICON-2I became **fully operational on 18 June 2024** and represents Italy's primary national convection-permitting NWP system, comparable in role to ICON-D2 (Germany) or AROME (France). It replaced the previous COSMO-2I chain.

Cycle directories on MeteoHub follow the format `YYYYMMDDHH/` (e.g. `2026042700/` for the 27 April 2026 00 UTC run). Each cycle directory contains one subdirectory per output parameter (see parameter index files linked above for the full list, which includes precipitation, temperature, humidity, wind, radiation fluxes, CAPE/CIN, lightning potential index, and others).

---

## Official documentation
- https://dati.agenziaitaliameteo.it
- https://www.agenziaitaliameteo.it/
