# ARSO Nowcast (Slovenia)

## What this model is
ARSO Nowcast is the Slovenian Environment Agency's gridded weather-analysis and very-short-range forecast (nowcast) product over Slovenia. It provides 1 km fields of near-surface air temperature, near-surface wind, precipitation, and sky openness (clear-sky fraction), refreshing every 30 minutes to one hour. ARSO describes it as numerical model computations of weather analysis and very-short-range forecasts; the cited documentation does not specify the underlying nowcasting method (observation extrapolation, NWP blend, or rapid-update NWP), so that is left as TBD below.

---

## Who runs it
- **Organization:** Slovenian Environment Agency (ARSO)
- **Country / region:** Slovenia

---

## What area it covers
- **Coverage:** Slovenia (geographic area of Slovenia)
- **Domain details:** Regular 1 km grid over Slovenia. Exact bounds, grid dimensions, and projection are not stated in the cited documentation (TBD).

---

## Basic details
- **System type:** Nowcasting
- **Nowcasting method:** Numerical analysis + very-short-range forecast. The source characterizes the product as "numerical model computations" for weather analysis and very-short-range forecasts; whether it uses observation extrapolation, NWP blending, or rapid-update NWP is not stated (TBD).
- **Technique / algorithm:** TBD (not documented in the cited sources)
- **Underlying / driving model:** TBD. ARSO operates a separate convection-permitting rapid-update nowcasting system, ALARO-RUC (NWCRUC), noted in the [ALADIN Slovenia](../../../nwp_models/regional/slovenia/aladin-slovenia.md) entry; any derivation/blending relationship with this 1 km product is not documented (see Notes).
- **Probabilistic / ensemble:** No — single deterministic gridded fields; no probabilistic products documented
- **Horizontal resolution:** 1 km
- **Vertical structure:** 2D single-level surface fields (near-surface temperature and wind, precipitation, sky openness)
- **Lead time:**
  - Precipitation and sky openness: up to **+1 h**
  - Temperature and wind: up to **+6 h**
- **Update frequency:**
  - Precipitation and sky openness: every **30 min**
  - Temperature and wind: every **1 h**
- **Temporal output resolution:** TBD (the 30-min product implies 30-min steps; step spacing of the hourly product not documented)
- **Latency:** ~15 min after nominal time (observed from directory timestamps — e.g. the file valid 13:00 appears ~13:15; not formally documented)

---

## Inputs
The observations and/or model fields the nowcast is built from are not specified in the cited ARSO documentation.
- **Radar:** TBD
- **Satellite:** TBD
- **Lightning:** TBD
- **Surface / other observations:** TBD
- **NWP fields:** TBD

---

## What it provides
Gridded 1 km fields of:
- near-surface air temperature
- near-surface horizontal wind components (u, v)
- precipitation amount
- sky openness — fraction of clear sky (the inverse of cloud cover)

Most fields follow the WMO GRIB parameter table; **sky openness is the exception, encoded in local table 219, parameter number 149.**

---

## Data availability
- **Is the data free?** Yes
- **License:** Open with mandatory attribution. ARSO publishes information from meteo.arso.gov.si as freely reusable on the condition that the source is cited as "Vir: Agencija Republike Slovenije za okolje" or "Vir: ARSO" (in English: "Source: Slovenian Environment Agency" or "Source: ARSO"). Mandatory attribution is stipulated by **Article 14 of the Slovenian Act on the State Meteorological, Hydrological, Oceanographic and Seismological Service** (Official Gazette of the Republic of Slovenia, No. 60/17). No specific named open-data licence (e.g., CC BY) is asserted; reuse terms are governed by the ARSO site policy and Slovenian law.
- **Is the data downloadable?** Yes
- **Data formats:** GRIB (distributed as compressed ZIP archives)
- **File naming:**
  - `nowcast_30min_yyyyMMdd-HHmm.zip` (e.g. `nowcast_30min_20220523-0800.zip`) — the 30-min stream (precipitation, sky openness)
  - `nowcast_yyyyMMdd-HHmm.zip` (e.g. `nowcast_20220523-0800.zip`) — the hourly stream (temperature, wind)
- **Official download location:**
  https://meteo.arso.gov.si/uploads/probase/www/nowcast/data/
- **Retention:** Files are kept for roughly the last **24 hours** (the directory listing spans a rolling 24 h window).

---

## Notes
- The download endpoint carries **two distinct file streams**: a 30-min stream (`nowcast_30min_*`, ~0.1 MB each, precipitation + sky openness to +1 h) and an hourly stream (`nowcast_*`, ~1.2–1.4 MB each, temperature + wind to +6 h).
- **GRIB coding:** parameters follow the WMO GRIB Edition 1 parameter table, with the sole exception of sky openness (local table 219, parameter 149). Decode with `wgrib` or ECMWF's ecCodes.
- **Relationship to ARSO's other nowcasting work is unconfirmed.** The [ALADIN Slovenia](../../../nwp_models/regional/slovenia/aladin-slovenia.md) entry documents a separate ARSO rapid-update nowcasting system, **ALARO-RUC (NWCRUC)** — a non-hydrostatic 1.3 km, North Adriatic, 5-min-output system running to +36 h. That is a different product from this 1 km Slovenia nowcast; whether this 1 km product is derived from or blended with NWCRUC (or ALADIN-SI) is not stated in the cited sources.
- ARSO publishes the companion 4 km ALADIN-SI NWP feed at a sibling endpoint (`.../www/model/data/`); see the [ALADIN Slovenia](../../../nwp_models/regional/slovenia/aladin-slovenia.md) entry.

---

## Official documentation
- ARSO — GRIB output documentation (weather analysis & very-short-range forecasts):
  https://meteo.arso.gov.si/uploads/meteo/help/sl/NumericniRezultatiGRIB.html
- ARSO website — Conditions of re-use:
  https://meteo.arso.gov.si/met/sl/about/
