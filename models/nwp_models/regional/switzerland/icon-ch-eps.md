# ICON-CH-EPS (Switzerland)

## What this model is
ICON-CH-EPS is the operational **high-resolution regional ensemble** numerical weather prediction (NWP) system run by MeteoSwiss for Switzerland and the wider Alpine region. It comprises two nested convection-permitting ensemble configurations — **ICON-CH1-EPS** (~1 km) and **ICON-CH2-EPS** (~2.1 km) — both built on the ICON model and run as ensembles to represent forecast uncertainty explicitly.

The systems are designed for the topographically challenging Alpine region, where MeteoSwiss positions Switzerland at the centre of the model domain. They are run several times a day on the Alps supercomputer at the Swiss National Supercomputing Centre (CSCS).

---

## Who runs it
- **Organization:** MeteoSwiss (Federal Office of Meteorology and Climatology)
- **Country / region:** Switzerland
- **Compute:** Swiss National Supercomputing Centre (CSCS), "Alps" platform
- **Model development:** ICON is developed jointly by DWD and the Max Planck Institute for Meteorology; MeteoSwiss participates via the C2SM (ETH Zurich et al.) and the COSMO consortium

---

## What area it covers
- **Coverage:** The entire Alpine region, with Switzerland at the centre of the domain
- **Domain note:** Both configurations share the same geographic domain; they differ in horizontal resolution, ensemble size, forecast length, and update cadence

---

## Basic details
- **Model type:** Regional ensemble NWP (convection-permitting)
- **Model system / core:** ICON (Icosahedral Nonhydrostatic), limited-area configuration
- **Dynamical formulation:** Non-hydrostatic, on a triangular (icosahedral) horizontal grid
- **Convection-allowing:** Yes — deep convection is explicitly resolved at both 1 km and 2.1 km; no deep-convection parameterization
- **Horizontal resolution:** ~1 km (ICON-CH1-EPS) / ~2.1 km (ICON-CH2-EPS), on the native icosahedral grid
- **Vertical levels:** 80 full levels (81 half levels), terrain-following height-based coordinate (Lorenz staggering), model top at ~22 km
- **Model topography:** Reaches 4,440 m at its highest point (ICON-CH1-EPS)
- **Grid points:** ~91,838,400 (ICON-CH1-EPS) / ~22,710,080 (ICON-CH2-EPS)
- **Internal model time step:** 10 s (ICON-CH1-EPS) / 20 s (ICON-CH2-EPS)
- **Temporal output resolution:** Hourly (both)

### ICON-CH1-EPS
- **Ensemble members:** 11
- **Forecast length:** Up to 33 hours (the 03 UTC run is extended to 45 hours to cover the whole of the following day)
- **Update frequency:** Every 3 hours — 8× daily (00, 03, 06, 09, 12, 15, 18, 21 UTC)

### ICON-CH2-EPS
- **Ensemble members:** 21
- **Forecast length:** Up to 120 hours (5 days)
- **Update frequency:** Every 6 hours — 4× daily (00, 06, 12, 18 UTC)

---

## Data assimilation
- **System:** KENDA (Kilometre-scale Ensemble Data Assimilation)
- **Method:** Local Ensemble Transform Kalman Filter (LETKF), producing an analysis ensemble that supplies the slightly differing initial conditions for the forecast ensemble
- **Cadence / grid:** The analysis ensemble is cycled **hourly** on the ICON-CH1-EPS 1 km / 80-level grid. ICON-CH2-EPS initial conditions are obtained by **upscaling** (interpolating) the ICON-CH1-EPS analysis to the 2.1 km grid every 6 hours
- **Radar:** Weather-radar data are assimilated via **latent heat nudging (LHN)**, particularly valuable in the first forecast hours
- **Observations:** Ground-based stations, radiosoundings, aircraft (AMDAR and Mode-S), wind profilers/radar, wind lidar, and ship/buoy reports
- **Analysis product:** The KENDA-CH1 analysis is also published separately via Open Data (collection `ch.meteoschweiz.ogd-analysis-kenda-ch1`)

---

## Initial and boundary conditions
- **Initial conditions:** KENDA / LETKF analysis ensemble (see above)
- **Boundary conditions:** ECMWF **IFS ENS** global ensemble (~9 km), which drives both ICON-CH1-EPS and ICON-CH2-EPS

---

## What it provides
Probabilistic (ensemble) forecasts including:
- Surface and near-surface fields: temperature, humidity, wind, gusts, mean sea level pressure, precipitation, cloud cover
- Upper-air fields on 80 model levels (multi-level parameters)
- Both the **full ensemble** (perturbed members) and the **unperturbed control run**
- **Pollen concentrations** (ICON-CH2-EPS control run only): alder, ragweed, birch, hazel, and grasses, on the lowest full model level (`model_level=80`, ~first 20 m above ground), present only during each type's flowering season

Constant (per-run) fields are distributed as separate static files:
- **Horizontal constants:** `CLON`, `CLAT` (triangle-centre longitude/latitude), `FR_LAND` (land/sea), `HSURF` (surface height), `SOILTYPE`
- **Vertical constants:** `HHL` (geometric height of the half levels)

---

## Data availability
- **Is the data free?** Yes
- **License:** CC BY 4.0 (MeteoSwiss Open Data; attribution required) — https://opendatadocs.meteoswiss.ch/general/terms-of-use
- **Is the data downloadable?** Yes
- **Data formats:** GRIB2 (GRIB edition 2). Decoding multi-level and some surface fields requires the **COSMO consortium local definitions** for ecCodes (`eccodes-cosmo-resources`)
- **Grid:** Native icosahedral grid only — forecast GRIB files do **not** contain lat/lon or height; geolocation requires the static horizontal/vertical constants files
- **Retention:** **24 hours only.** Files are removed after the availability window; requests for older data return an empty response or HTTP 403. There is no official long-term archive — users building archives must retain files themselves
- **Boundary note:** Values at the spatial domain boundary may be unreliable
- **Official download location / docs:**
  - https://opendatadocs.meteoswiss.ch/e-forecast-data/e2-e3-numerical-weather-forecasting-model
  - STAC collections:
    - ICON-CH1-EPS: `ch.meteoschweiz.ogd-forecasting-icon-ch1`
    - ICON-CH2-EPS: `ch.meteoschweiz.ogd-forecasting-icon-ch2`

### Access methods
MeteoSwiss exposes the data through the Swiss federal geodata STAC API at `data.geo.admin.ch`. Three documented routes:
1. **Python API** — the `meteodata-lab` library (`meteodatalab.ogd_api`), which loads GRIB2 directly into xarray and supports `ref_time="latest"`.
2. **REST API (HTTP POST)** — the STAC search endpoint (used by Open-Meteo; see below).
3. **STAC Browser** — manual browsing/download per collection.

### How Open-Meteo retrieves the data
Open-Meteo uses the **REST/STAC search** route. Each forecast file is a single GRIB2 message for one collection, one reference time, one lead time, and one variable — as either the control run or the full ensemble.

1. **Resolve the download URL** with a POST to the STAC search endpoint:

```
   POST https://data.geo.admin.ch/api/stac/v1/search
   Content-Type: application/json

   {
     "collections": ["ch.meteoschweiz.ogd-forecasting-icon-ch1"],
     "forecast:reference_datetime": "2025-03-12T12:00:00Z",
     "forecast:variable": "TOT_PREC",
     "forecast:perturbed": false,
     "forecast:horizon": "P0DT12H00M00S"
   }
```

   - `forecast:reference_datetime` — model run initialization time (ISO 8601, `Z`)
   - `forecast:variable` — the parameter shortName (e.g. `T_2M`, `TOT_PREC`, `U_10M`, `PMSL`)
   - `forecast:perturbed` — `false` for the control run, `true` for the perturbed ensemble members
   - `forecast:horizon` — lead time as an ISO 8601 duration, e.g. `P0DT00H00M00S` for +0 h, `P0DT12H00M00S` for +12 h

   The response is a STAC FeatureCollection; the GRIB file is the pre-signed URL in the first feature's `assets[].href`. For ensemble domains, Open-Meteo issues two requests per timestep/variable — one with `forecast:perturbed=false` (control) and one with `true` (members) — and merges the results.

2. **Download** the GRIB2 file from the pre-signed `href` (plain GRIB2, no extra compression). Integrity can be checked against the `x-amz-meta-sha256` header.

3. **Fetch static grid info once per collection** via a GET request, and locate `horizontal_constants_icon-ch1-eps.grib2` (for `CLON`/`CLAT`/`FR_LAND`/`HSURF`) in the assets:

```
   GET https://data.geo.admin.ch/api/stac/v1/collections/ch.meteoschweiz.ogd-forecasting-icon-ch1/assets
```

4. **Remap to a regular grid.** Because the published data sits on the native icosahedral grid (one value per triangle centre), Open-Meteo builds a **nearest-neighbour mapping** from the `CLON`/`CLAT` coordinates onto an internal **rotated latitude–longitude grid** (rotated pole at 43.0°N, 190.0°E):
   - ICON-CH1-EPS: 1089 × 705 at 0.01°
   - ICON-CH2-EPS: 545 × 353 at 0.02°

   These mapping weights are computed once and cached; elevation is taken from `HSURF` (masked to land via `FR_LAND`).

### Parameter naming
GRIB shortNames follow DWD/COSMO conventions — e.g. `T_2M` (2 m temperature), `TD_2M` (2 m dew point, converted to relative humidity by Open-Meteo), `TOT_PREC` (total precipitation), `PMSL` (mean sea level pressure), `U_10M`/`V_10M` (10 m wind), `VMAX_10M` (gusts), `CLCT`/`CLCL`/`CLCM`/`CLCH` (cloud cover), `ASWDIR_S`/`ASWDIFD_S` (direct/diffuse shortwave radiation), `CAPE_ML`, `CIN_ML`, `HZEROCL`, `SNOWLMT`, `H_SNOW`, `T_G`. Each collection publishes a continuously updated "Overview of Parameters" CSV (long name, unit, level type, aggregation, etc.) in its STAC assets.

---

## Notes
- **Analysis equivalence:** MeteoSwiss does not distribute analysis fields within these forecast collections. The forecast at **lead time 0** (`P0DT00H00M00S`) is the analysis equivalent — available every 3 h (ICON-CH1-EPS) or 6 h (ICON-CH2-EPS). The full hourly analysis ensemble is published separately as KENDA-CH1.
- **Native grid only:** Unlike DWD's ICON Open Data (which also offers pre-interpolated lat/lon grids), MeteoSwiss publishes only the native icosahedral grid; any regular-grid product must be interpolated by the user.
- **Relationship to other ICON systems:** ICON-CH-EPS shares the ICON dynamical core and non-hydrostatic formulation with DWD's [ICON Global](../../global/germany/icon-global.md) family. It is comparable in role and resolution to DWD's [ICON-D2-EPS](../../../ensemble_models/regional/de/icon-d2-eps.md) convection-allowing ensemble (2.2 km, 20 members), though ICON-CH1-EPS pushes to ~1 km.
- **Output is probabilistic:** Results should be interpreted as an ensemble (member spread / probabilities), not a single deterministic forecast. Open-Meteo currently ingests the control and perturbed members and processes ICON-CH1-EPS to 33 h and ICON-CH2-EPS to 120 h.
- **24 h retention** is the most operationally significant constraint for downstream archiving.

---

## Official documentation
- MeteoSwiss Open Data — ICON-CH1/2-EPS: https://opendatadocs.meteoswiss.ch/e-forecast-data/e2-e3-numerical-weather-forecasting-model
- ICON-CH1/2-EPS changelog: https://opendatadocs.meteoswiss.ch/e-forecast-data/e2-e3-numerical-weather-forecasting-model-changelog
- ICON forecasting systems overview: https://www.meteoswiss.admin.ch/weather/warning-and-forecasting-systems/icon-forecasting-systems.html
- Ensemble data assimilation (KENDA): https://www.meteoswiss.admin.ch/weather/warning-and-forecasting-systems/icon-forecasting-systems/ensemble-data-assimilation.html
- KENDA-CH1 analysis data: https://opendatadocs.meteoswiss.ch/e-forecast-data/e5-numerical-weather-analysis-data
- Terms of use (CC BY 4.0): https://opendatadocs.meteoswiss.ch/general/terms-of-use
- Example notebooks (data retrieval to visualization): https://github.com/MeteoSwiss/opendata-nwp-demos
