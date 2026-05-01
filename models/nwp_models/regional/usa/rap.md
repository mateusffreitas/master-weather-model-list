# RAP (Rapid Refresh)

## What this model is
The Rapid Refresh (RAP) is a high-frequency regional numerical weather prediction model designed for short-range forecasting of rapidly evolving weather, including fronts, wind shifts, temperature changes, and aviation-relevant conditions.

It serves as both a standalone forecast system and as the parent model providing initial and boundary conditions for the HRRR.

Since RAPv5 (December 2020), RAP also produces operational wildfire smoke forecasts as integrated output fields, with smoke aerosols feeding back on shortwave radiation and the atmospheric forecast. RAP-Smoke output is also the operational smoke component of NOAA's National Air Quality Forecast Capability (NAQFC) since June 2022.

---

## Who runs it
- **Organization:** NOAA / National Weather Service (NCEP)
- **Country / region:** United States

---

## What area it covers
- **Coverage:** North America

---

## Basic details
- **Model type:** Deterministic NWP
- **Model system / core:** WRF-ARW
- **Horizontal resolution:** ~13 km
- **Vertical levels:** 50
- **Model top:** 10 mb (hybrid sigma–isobaric)
- **Forecast length:**  
  - Up to 51 h for 03/09/15/21 UTC cycles  
  - Up to 21 h for other cycles
- **Update frequency / cycles:** Hourly (24× daily)
- **Temporal output resolution:** Hourly

---

## Data assimilation
- **Data assimilation:** Yes
- **Method / cadence:**  
  - Hourly cycling Gridpoint Statistical Interpolation (GSI)  
  - Assimilation of conventional, aircraft, satellite, cloud, and hydrometeor observations to improve short-range cloud and precipitation forecasts

---

## Initial and boundary conditions (for limited-area models)
- **Initial conditions:** RAP cycling analysis
- **Boundary conditions:** GFS (most recent available GFS forecast cycle)

---

## What it provides
Deterministic short-range forecasts of:
- Temperature, wind, humidity, and pressure
- Cloud cover, ceilings, and visibility
- Precipitation and precipitation type
- Aviation-relevant weather fields
- Wildfire smoke fields (near-surface smoke concentration and vertically integrated smoke), with smoke aerosols feeding back on radiation and the atmospheric forecast

RAP is commonly used for aviation forecasting and mesoscale analysis and complements HRRR by providing broader-scale context at slightly lower resolution.

---

## Data availability
- **Is the data free?** Yes
- **Is the data downloadable?** Yes
- **Data formats:** GRIB2
- **Official download locations:**  
  https://nomads.ncep.noaa.gov/pub/data/nccf/com/rap/prod/  
  https://registry.opendata.aws/noaa-rap/

The smoke fields are distributed alongside the standard meteorological fields in the operational RAP output. NOAA's RAP graphics portal also presents smoke fields under a separate "RAP-Smoke" menu page, but these are sourced from the same operational model run.

---

## Notes
- RAP and HRRR share much of the same modeling and assimilation infrastructure.
- RAP provides the initial and lateral boundary conditions for HRRR.
- Unlike HRRR, RAP uses a cumulus parameterization and is intended to retain synoptic-scale structure.
- RAPv5 (December 2, 2020) is the final version of RAP. No further upgrades are planned; the system will eventually be retired with the RRFSv2 transition.
- RAP-Smoke is no longer a separate experimental run — wildfire smoke prediction has been integrated into operational RAPv5 since December 2020, with smoke aerosols affecting radiation and feeding back into the meteorological forecast. This is the same RAPv5/HRRRv4 upgrade that integrated smoke into HRRR; the two systems share the smoke modelling architecture.
- Within NAQFC, RAP-Smoke replaced HYSPLIT as the operational smoke forecast guidance source on 28 June 2022; HYSPLIT is now used only for dust forecasts.

### Future outlook
RAP is expected to eventually be retired alongside HRRR as part of the RRFSv2 transition (MPAS-based). Unlike the NAM, HiresW, and HREF — which are covered by NWS Public Information Statement 25-41 — no formal retirement notification for RAP has been issued as of April 2026.

---

## Official documentation
- https://www.ncei.noaa.gov/products/weather-climate-models/rapid-refresh-update
- https://rapidrefresh.noaa.gov/
- NWS SCN 20-46 (RAPv5/HRRRv4 implementation): https://www.weather.gov/media/notification/pdf2/scn20-46rap_v5_hrrr_v4_aab.pdf
- Benjamin et al. (2016), *A North American Hourly Assimilation and Model Forecast Cycle: The Rapid Refresh*. Mon. Wea. Rev., 144, 1669-1694.
