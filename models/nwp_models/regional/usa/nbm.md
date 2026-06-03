# NBM (National Blend of Models)

## What this model is
The National Blend of Models (NBM) is a statistically post-processed forecast system that blends and calibrates output from many different numerical weather prediction models into a single, nationally consistent set of deterministic and probabilistic forecast guidance.

Rather than running its own atmospheric simulation, the NBM applies bias correction, weighting, and blending techniques to both NWS and non-NWS model guidance to produce a highly skillful starting point for official National Weather Service forecasts and the National Digital Forecast Database (NDFD).

---

## Who runs it
- **Organization:** NOAA / National Weather Service (Meteorological Development Laboratory)
- **Country / region:** United States

---

## What area it covers
- **Coverage:** United States and surrounding regions
- **Domain details:**  
  Multiple operational domains, including CONUS, Alaska, Hawaii, Puerto Rico, Guam, Oceanic regions, and a coarse global domain

---

## Basic details
- **Model type:** Statistical multi-model blend (post-processed guidance)
- **Horizontal resolution:**  
  - CONUS: ~2.5 km  
  - Alaska: ~3.0 km  
  - Hawaii: ~2.5 km  
  - Puerto Rico: ~1.25 km  
  - Oceanic: ~10 km  
  - Global: ~50 km
- **Forecast length:**  
  Up to ~264 hours (11 days), depending on variable
- **Update frequency / cycles:** Hourly (24× daily)
- **Temporal output resolution (NBM v5.0, operational May 5, 2026):**  
  - Hourly out to 48 h for most parameters  
  - 3-hourly through ~192 h  
  - 6-hourly thereafter  
  - Some aviation and severe-weather fields (ceiling, visibility, cloud base, cloud layers, low-level wind shear, turbulence, Ellrod Index, solar radiation, thunderstorm coverage, thunderstorm probability, Craven-Wiedenfeld Aggregate Severe Parameter) continue to use the pre-v5.0 hourly-to-36 h cadence

---

## What it provides
Statistically calibrated deterministic and probabilistic forecast guidance for:
- Near-surface temperature, dewpoint, apparent temperature, wind, and pressure
- Precipitation amount, type, and probability-matched mean (PMM) QPF (24h, CONUS/AK)
- Snow and ice guidance, including PMM snow/ice, calibrated snow exceedance (24h/48h/72h, CONUS), and deterministic snow depth (CONUS/AK)
- Sky cover, ceilings, and visibility
- Wind gusts and hazardous weather parameters
- Fire weather joint probabilities (wind speed × relative humidity combinations)
- Deterministic and probabilistic precipitable water
- Probabilistic surface-based CAPE (10th/50th/90th percentiles, CONUS)
- Wet bulb globe temperature in NBM text products (NBH/NBS/NBE/NBX)
- Probabilistic guidance (percentiles, thresholds, and risk-based products)

NBM guidance is the primary foundation for official NWS gridded forecasts in the National Digital Forecast Database.

---

## Input models

NBM ingests a large, evolving set of deterministic, ensemble, statistical, and marine inputs from five operational centres: **NOAA/NWS**, the **Canadian Meteorological Centre (CMC / ECCC)**, **ECMWF**, the **U.S. Navy (FNMOC)**, and the **Australian Bureau of Meteorology (BoM)**. The list below reflects the v5.0 input set; resolutions are the input resolution NBM ingests, not necessarily the source model's native grid.

**Global (deterministic)**
- GFS ~0.125° (NOAA)
- ECMWF (HRES) 0.10° (ECMWF) — was 0.25° before v5.0
- GDPS 0.25° (CMC)
- NAVGEM 0.5° (FNMOC)
- ACCESS-G (BoM)
- ECAIFS — ECMWF AI/IFS hybrid (added in v5.0; temperature, wind, QPF)
- AIGFS — NOAA AI global model (added in v5.0; temperature, wind, QPF)

**Regional (deterministic)**
- RAP ~13 km (NOAA)
- RDPS ~10 km (CMC)
- NAM 12 km (NOAA) — *to be removed in v5.0.1*

**High-resolution / convection-allowing (deterministic)**
- HRRR 3 km (NOAA)
- HAFS (NOAA, tropical cyclone) — supersedes the HWRF/HMON 2 km labels still shown on the legacy inputs graphic
- NAM Nest 3 km (NOAA) — *to be removed in v5.0.1*
- HiResARW 2.5 km, HiResARW2 2.5 km, HiResFV3 2.5 km (NOAA HiResW family) — *to be removed in v5.0.1*
- GTCM / wTCM tropical-cyclone wind module (NHC) — supplies 10 m wind in v5.0

**Ensembles**
- GEFS 0.25° (NOAA)
- ECMWF Ensemble (ECMWFE) 0.20° (ECMWF) — was 0.5° before v5.0
- GEPS 0.5° and REPS ~10 km (CMC)
- NAVGEM ensemble (FNMOC)
- ACCESS ensemble (BoM)
- GEWPS / REWPS wave ensembles
- 10 Canadian members (mix of GDPS/GEPS/RDPS/REPS) added to the winter suite in v5.0
- SREF — *removed in v5.0* (was 16 km CONUS / 30 km AK)

**Statistical / MOS**
- GFS-MOS, GFS GMOS, NAM GMOS, ECMWF MOS, ECMWFE MOS
- LAMP / gridded LAMP (GLMP)

**Marine / ocean**
- RTOFS ~0.3° (NOAA) and RIOPS (CMC) — marine and ice/ocean inputs

> **Note on the webinar "NBM Inputs" graphic:** The April 15, 2026 NBM user webinar (slide 4) shows a pre-v5.0 input graphic that still lists SREF, lower-resolution ECMWF (0.25°/0.5°), and HWRF/HMON, and omits ECAIFS and AIGFS. Treat that graphic as a historical snapshot; the list above reflects the v5.0 operational set.

---

## Data availability
- **Is the data free?** Yes
- **License:** Public domain (U.S. government work; CC0-equivalent)
- **Is the data downloadable?** Yes
- **Data formats:** GRIB2 and ASCII/text bulletins (NBH/NBS/NBE/NBX/NBP)
- **Official download locations:**  
  https://registry.opendata.aws/noaa-nbm/ (realtime + archive back to May 2020)  
  https://nomads.ncep.noaa.gov/pub/data/nccf/com/blend/prod/ (realtime; ~2-day retention)

### Text bulletin products
| Label | Product | Time step | Forecast hours |
|---|---|---|---|
| NBH | Hourly | Hourly | 1–25 h |
| NBS | Short | 3-hourly | 6–72 h |
| NBE | Extended | 12-hourly | 24–192 h |
| NBX | Super-Extended | 12-hourly | 204–264 h |
| NBP | Probabilistic (Extended) | 12-hourly | 24–264 h |

---

## Notes
- **Operational role:** NBM is the starting point for the NWS gridded forecast in the Days 0–3 period (with WFOs making local edits) and now serves directly as the Days 4–7 CONUS gridded forecast, with only minor edits from the Weather Prediction Center in consultation with WFOs; the same Days 4–7 role is being extended to Alaska. NBM also supplies key tools for NWS Impact-Based Decision Support Services (IDSS).
- NBM blends input from a large and evolving set of deterministic and ensemble models, including global, regional, and convection-allowing systems.
- Bias correction methods include decaying averages, quantile mapping (QM), and dynamically weighted model contributions. NBM v5.0 expands QM usage to instantaneous temperature, dew point, apparent temperature, relative humidity, 12-hour max/min RH, and significant wave height — replacing decaying-average logic for those fields.
- NBM v5.0 introduces a new "percentile picking" method for deterministic wind speed and gust forecasts, modulating away from the mean QM value based on where the mean sits relative to the model distribution and climatology.
- NBM produces both deterministic and probabilistic guidance, and v5.0 increases consistency between them by applying shared input weighting and derivation methods.
- The Haines Index is no longer produced in NBM v5.0 (CONUS, AK, HI, PR domains).

---

## Planned future versions

> **Status:** Unofficial. As of June 2026 there is no NWS Service Change Notice or PNS for either release below, and neither appears on the NBM versions page. The details here come from NOAA/NWS presentation decks and should be treated as a planning signal, not a commitment. No implementation date is recorded here pending a formal SCN.

### PRO-GUIDES (long-term successor program)
The NBM user webinar (April 15, 2026) describes the **Next-Generation Probabilistic Forecast Guidance Suite (PRO-GUIDES)** as the longer-term direction beyond the v5.x line. A blueprint has been drafted and a technical feasibility team tasked with assessing current vs. needed resources. The stated intent is to apply modern machine-learning / AI techniques to perform smarter blending and bias correction and to better predict high-impact events. This is a program-level aspiration with no version number, schedule, or SCN.

### NBM v5.1 (planned, not scheduled)
Outlined in the NBM user webinar (April 15, 2026) and the WPC/HMT "NBMv5 Winter Overview" (February 10, 2026). Planned scope:
- **12-hour winter products** added to the winter suite
- Continued work on **remaining winter-weather guidance issues** left after v5.0
- Fixes for **"blocky" percentile fields** and **precipitation-type (ptype) fields**
- A **new NBM domain in the Southwest Pacific** — a coverage expansion rather than a methodology change

The source decks referenced a tentative timeframe, but no date is recorded here until an SCN or the NBM versions page makes it official.

### NBM v5.0.1 (interim, tied to RRFS/REFSv1)
The same decks indicate that the RRFS/REFSv1 implementation will likely force an interim **v5.0.1**, in which the **NAM and HiResW inputs are dropped from the NBM** (the new RRFS deterministic run and REFS members take their place in the relevant suites, including winter). When the decks were prepared (Feb/Apr 2026), RRFS/REFSv1 was expected around mid-July 2026; it has since been formally scheduled for **August 31, 2026 at 12 UTC** under NWS SCN 26-48, so any v5.0.1 cutover would be expected to align with that date rather than the earlier deck estimate. The RRFS/REFS-era winter input weights were described as effectively finalized but subject to change pending evaluation of RRFS/REFS output.

**Sources (presentation decks, not formal notices):**
- NBM user webinar slides, April 15, 2026: https://www.weather.gov/media/wrn/calendar/NBMUserWebinar4-15-26.pdf
- WPC/HMT "NBMv5 Winter Overview" slides, February 10, 2026: https://www.wpc.ncep.noaa.gov/hmt/hmt_webpages/seminars/wwe/2026/slides_20260210.pdf

---

## Version history

### NBM v5.0 (operational May 5, 2026)
Implemented with the 13 UTC cycle on May 5, 2026, via NWS Service Change Notice 26-24. The original SCN (March 12, 2026) targeted April 15, 2026; subsequent updates rescheduled implementation to April 23, then April 30 (AAC revision, April 28, 2026). The April 30 attempt fell within a Critical Weather Day / Enhanced Caution Event window, triggering the SCN's contingency provision and pushing the actual cutover to May 5, 2026.

Key changes:
- Hourly guidance extended from 36 h to 48 h for most parameters
- New products: 24h PMM QPF and snow/ice (CO/AK), joint fire weather probabilities, quantile-mapped 10 m wind and gust for AK/HI/PR/GU/OC, precipitable water guidance, calibrated snow exceedance (CO), deterministic snow depth (CO/AK), apparent temperature, CAPE weighted percentiles (CO)
- Removed: Haines Index
- Methodology: Expanded QM usage, new percentile-picking wind/gust method, shared input weighting for deterministic and probabilistic outputs
- Ceiling and visibility guidance for Hawaii replaced with gridded LAMP "Meld" approach (incorporating gridded observational analysis, RAP, GFS, and ECMWF, with a random-forest "pseudo-observation" technique for Hawaiian island points)
- Significant wave height (SWH) bias correction now uses ~120 ensemble member inputs (up from 13 ensemble means)
- SBN/NOAAPORT changes: removed binary scaling factor for several wind and temperature elements; added new cycles, projections, and elements; **NBM global product no longer disseminated over SBN/NOAAPORT** (still produced and available via NOMADS)
- Text product changes: WBG (wet bulb globe temperature) added to NBH/NBS/NBE/NBX; SWH now available for all cycles in NBX; HID (Haines Index) removed from all text products

Input changes in v5.0:
- Higher-resolution ECMWF (0.10°) and ECMWF Ensemble (0.20°), replacing 0.25° and 0.50°
- Higher-resolution GDPS (15 km) and REPS (10 km), replacing 25 km and 15 km
- Added ECAIFS (ECMWF AI/IFS) and AIGFS as inputs for temperature, wind speed, and QPF (all domains except no QPF over GU)
- Higher-resolution GEFS surface guidance (0.25° through 240 h; vertical profile remains at 0.5°)
- WPC MMEBC QPF added as input (CO)
- SREF input usage eliminated (SREF itself is subsequently scheduled for retirement on August 31, 2026 under SCN 26-48; v5.0's removal of SREF as an input is what enabled folding SREF retirement into the first-wave RRFSv1 cutover rather than holding it for RRFSv2)
- Significant wave height bias correction now uses ~120 ensemble member inputs (up from 13 ensemble means)

### NBM v4.3 (operational May 27, 2025)
Intermediate release prior to v5.0, primarily focused on improvements to the NBM Tropical Cyclone Feature-Matched wind speed product (originally planned for v5.0 but pulled forward at the National Hurricane Center's request for the 2025 tropical season) and updates from the Storm Prediction Center to probabilistic severe weather products. Probabilistic severe hazards for tornado, hail, and wind expanded to Day 2 and recalibrated from a GEFS+HREF blend (replacing SREF usage).

## Official documentation
- https://vlab.noaa.gov/web/mdl/nbm
- https://vlab.noaa.gov/web/mdl/nbm-model-inputs
- NBM version history: https://vlab.noaa.gov/web/mdl/nbm-versions
- NWS SCN 26-24 (NBM v5.0 implementation, AAC revision dated April 28, 2026): https://www.weather.gov/media/notification/pdf_2026/scn26-24_Updated_NBM_V5.0_aac.pdf
- NWS SCN 25-34 (NBM v4.3 implementation): https://www.weather.gov/media/notification/pdf_2025/scn25-34NBM_V4.3.pdf
