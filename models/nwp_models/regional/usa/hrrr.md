# HRRR (High-Resolution Rapid Refresh)

## What this model is
The High-Resolution Rapid Refresh (HRRR) is a convection-allowing, high-resolution regional numerical weather prediction model designed for very short-range forecasting of rapidly evolving weather such as thunderstorms, fog, low clouds, wind shifts, and precipitation.

It is optimized for near-surface and storm-scale prediction and is widely used in severe weather, aviation, fire weather, and short-term decision support across the United States.

Since HRRRv4 (December 2020), HRRR also produces operational wildfire smoke forecasts as integrated output fields, replacing the earlier experimental HRRR-Smoke parallel run. Smoke aerosols feed back on radiation and the atmospheric forecast, making HRRR one of the first operational weather models in the U.S. to include direct smoke-meteorology coupling.

---

## Who runs it
- **Organization:** NOAA / National Weather Service (NCEP)
- **Country / region:** United States

---

## What area it covers
- **Coverage:** United States (CONUS)
- **Domain details:** Separate operational domains also exist for Alaska, Hawaii, and Puerto Rico.

---

## Basic details
- **Model type:** Deterministic NWP
- **Model system / core:** WRF-ARW
- **Horizontal resolution:** ~3 km
- **Vertical levels:** 50
- **Model top:** 20 mb (hybrid sigma–isobaric)
- **Forecast length:**  
  - Up to 48 h for 00/06/12/18 UTC cycles  
  - Shorter forecasts for intermediate cycles
- **Update frequency / cycles:** Hourly (24× daily)
- **Temporal output resolution:**  
  - 15-minute output during early forecast hours  
  - Hourly output at longer lead times

---

## Data assimilation
- **Data assimilation:** Yes
- **Method / cadence:**  
  - Hourly cycling Gridpoint Statistical Interpolation (GSI)  
  - Intensive radar reflectivity and radial velocity assimilation  
  - Cloud and hydrometeor data assimilation during a 1-hour pre-forecast spinup
  - Hybrid data assimilation using flow-dependent covariances from the 36-member HRRR Data Assimilation System (HRRRDAS) ensemble at 3 km

---

## Initial and boundary conditions (for limited-area models)
- **Initial conditions:** RAP analysis
- **Boundary conditions:** RAP, updated hourly

---

## What it provides
Deterministic short-range forecasts of:
- Near-surface temperature, humidity, wind, and pressure
- Convection and severe storm evolution
- Precipitation type and intensity
- Low clouds, ceilings, visibility, fog
- Fire-weather-relevant fields including near-surface wind and humidity
- Wildfire smoke fields (near-surface smoke concentration and vertically integrated smoke), with smoke aerosols feeding back on radiation and the atmospheric forecast

HRRR is primarily intended for surface, boundary-layer, and storm-scale analysis rather than synoptic-scale upper-air diagnostics.

---

## Data availability
- **Is the data free?** Yes
- **Is the data downloadable?** Yes
- **Data formats:** GRIB2
- **Official download locations:**  
  https://nomads.ncep.noaa.gov/pub/data/nccf/com/hrrr/prod/  
  https://registry.opendata.aws/noaa-hrrr-pds/

The smoke fields are distributed alongside the standard meteorological fields in the operational HRRR output. NOAA's HRRR graphics portal also presents smoke fields under separate "HRRR CONUS Smoke Fields" and "HRRR Alaska Smoke Fields" menu pages, but these are sourced from the same operational model run.

---

## Notes
- HRRR is a convection-allowing model and does not use a cumulus parameterization.
- It is tightly coupled with RAP, which supplies its initial and boundary conditions.
- HRRR-Smoke is no longer a separate experimental run — wildfire smoke prediction has been integrated into operational HRRRv4 since December 2020, with smoke aerosols affecting radiation and feeding back into the meteorological forecast. HRRR-DAS (the 36-member ensemble data assimilation system) is the operational DA component of HRRRv4, not a separate variant.
- HRRRv4 is the final version of HRRR. No further upgrades are planned; the system will eventually be retired with the RRFSv2 transition.

### HRRR-Cast — experimental AI sibling
NOAA OAR/GSL released **HRRR-Cast** in July 2025, an AI-based experimental sibling that emulates HRRR forecasts using a neural network trained on HRRR analysis data. It is not a replacement for the operational HRRR — it runs experimentally at NWS EMC for evaluation, with HRRR-Cast v2 (September 2025) adding ensemble capabilities, increased vertical resolution (12 → 20 levels), and additional surface variables. A future transition to NWS operations is being evaluated, with v3 in development for late 2025 / early 2026 and trained on a larger ~9-year HRRR archive.

### Future outlook
HRRR is expected to eventually be retired as part of NOAA's broader consolidation of regional convection-allowing guidance under the Unified Forecast System. Unlike the NAM, HiresW, and HREF — which are covered by NWS Public Information Statement 25-41 and scheduled for retirement alongside RRFSv1 — HRRR is expected to continue operating until the RRFSv2 transition (based on the MPAS dynamical core). No formal retirement notification has been issued as of April 2026.

---

## Official documentation
- https://rapidrefresh.noaa.gov/hrrr/
- Dowell et al. (2022), *The High-Resolution Rapid Refresh (HRRR): An Hourly Updating Convection-Allowing Forecast Model. Part I*: https://repository.library.noaa.gov/view/noaa/53029
- HRRR-Cast (NOAA GSL): https://gsl.noaa.gov/news/new-upgrades-to-hrrr-cast-noaas-experimental-ai-powered-regional-model
