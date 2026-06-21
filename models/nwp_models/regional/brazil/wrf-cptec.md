# WRF (CPTEC/INPE Regional Model)

## What this model is
WRF is a regional numerical weather prediction (NWP) system operated by CPTEC/INPE, based on the Weather Research and Forecasting model and run operationally for South America since 2018. It provides short- to medium-range deterministic forecasts and is publicly distributed as a single configuration over a continental South America domain at ~7 km resolution (`ams_07km`).

---

## Who runs it
- **Organization:** CPTEC/INPE (Center for Weather Forecasting and Climate Studies / National Institute for Space Research)
- **Country / region:** Brazil

---

## What area it covers
- **Coverage:** South America
- **Domain details:** Continental South America domain (`ams_07km`); exact grid bounds TBD.

---

## Basic details
- **Model type:** Regional deterministic NWP
- **Model system / core:** WRF (Weather Research and Forecasting); ARW core unconfirmed
- **Dynamical formulation:** Non-hydrostatic (WRF)
- **Convection-allowing:** No — at ~7 km the model uses parameterized convection
- **Horizontal resolution:** ~7 km
- **Vertical levels:** TBD
- **Model top:** TBD
- **Forecast length:** +180 h (7.5 days)
- **Update frequency / cycles:** 1× daily (00 UTC)
- **Temporal output resolution:** Hourly

---

## Data assimilation (optional)
- **Data assimilation:** TBD

---

## Initial and boundary conditions
- **Initial / boundary conditions:** TBD — CPTEC's regional WRF is nested in a global driver (plausibly GFS or CPTEC's BAM), but the source is unconfirmed for the public feed.

---

## What it provides
Deterministic regional forecasts of core atmospheric variables, including:
- Near-surface temperature, wind, and humidity
- Atmospheric pressure and geopotential
- Precipitation
- Mid- and upper-level circulation fields

The WRF configuration is one of CPTEC's two operational regional NWP systems over South America, run alongside the [Eta](./eta-cptec.md) configurations.

---

## Data availability
- **Is the data free?** Yes — free of charge, no registration.
- **License:** **Transitional / not yet an open license.** Data is freely accessible and usable personally today, but CPTEC/INPE's operational-server notice restricts commercial use and redistribution in published/dissemination outlets without express CPTEC/INPE authorization, and requires attribution to "CPTEC/INPE." INPE has committed under its Open Data Plan (PDA 2025–2027, Decreto 8.777/2016) to republish the WRF data as open data on dados.gov.br, scheduled ~June 2026. As of late June 2026 it is not yet live on the dados.gov.br "Tempo e Clima" category; open reuse terms apply once it appears there.
- **Is the data downloadable?** Yes
- **Data formats:** GRIB2
- **Official download location:**
  https://dataserver.cptec.inpe.br/dataserver_modelos/wrf/ams_07km/
  - `ams_07km/brutos/<YYYY>/<MM>/<DD>/00/`
- **File structure note:** Each timestep ships as a separate GRIB2 file (`WRF_cpt_07KM_<init>_<valid>.grib2`, with `<init>`/`<valid>` as `YYYYMMDDHH`) plus a GrADS descriptor (`.ctl`), an index (`.grib2.idx`), and a wgrib-style inventory (`.inv`). Most users will want the `.grib2` files.

---

## Notes
- **Cycle:** Only the 00 UTC cycle is published (verified — no 12 UTC directory exists). This supersedes the earlier note of a 2× daily cycle and ~72 h length in the previous "CPTEC/INPE Regional Model (7 km)" entry; the public feed runs once daily to +180 h.
- **Relationship to siblings:** Run alongside the [Eta](./eta-cptec.md) regional configurations as CPTEC's second operational regional NWP system, and complements [BAM](../../global/brazil/bam-cptec.md) (global). WRF also contributes to CPTEC's SMEC multi-model ensemble (Eta + WRF + BRAMS).
- **Naming on gov.br:** The gov.br PNT service page lists this as "WRF 05km," which conflicts with the ~7 km resolution of the `ams_07km` operational feed; the dataserver path and filenames are treated as authoritative here.
- **Older data:** Only the current month is served on the operational FTP/dataserver; older data requires a request to CPTEC and is subject to availability.

---

## Official documentation
- CPTEC/INPE model data server — https://dataserver.cptec.inpe.br/dataserver_modelos/wrf/ams_07km/
- CPTEC/INPE — https://www.cptec.inpe.br/
- gov.br service page (PNT) — https://www.gov.br/pt-br/servicos/obter-dados-provenientes-de-modelos-numericos-de-previsao-de-tempo-inpe-pnt
