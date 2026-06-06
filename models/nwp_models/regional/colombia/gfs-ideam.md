# GFS (IDEAM Redistribution – Colombia)

## What this model is
IDEAM redistributes NOAA's Global Forecast System (GFS, FV3 core) through its national modeling portal for users in Colombia and the wider Andean region. The data is produced by NOAA/NCEP; IDEAM mirrors selected cycles, resolutions, and forecast lengths locally and uses this guidance as the backbone of several national forecast products (precipitation, wind circulation, synoptic-scale analysis) and as initial/boundary conditions for its regional [WRF system](./wrf-ideam.md).

This entry documents the redistribution. For model configuration, physics, dynamical core, and authoritative documentation, see the primary [GFS entry](../../global/usa/gfs.md).

---

## Who runs it
- **Source model operator:** NOAA / National Centers for Environmental Prediction (NCEP)
- **Redistributor:** Instituto de Hidrología, Meteorología y Estudios Ambientales (IDEAM)
- **Country / region:** Colombia (redistribution); United States (source)

---

## What area it covers
- **Coverage:** Global (source model)
- **Primary area of use:** Colombia and surrounding regions of northern South America

---

## Basic details
- **Model type:** Deterministic global NWP (redistributed)
- **Dynamical core:** FV3 (Finite-Volume Cubed-Sphere)
- **Publicly distributed cycles (IDEAM subset):**
  | Cycle | Resolution | Forecast length | Approx. update (HLC, UTC−5) |
  |-------|-----------|-----------------|------------------------------|
  | 00Z   | 0.5°      | ~7 days         | ~01:00 |
  | 06Z   | 0.25°     | ~15 days        | ~07:00 |
  | 12Z   | 0.5°      | ~7 days         | ~12:00 |
  | 18Z   | 0.25°     | ~15 days        | ~21:00 |
- **Update frequency / cycles:** 4× daily (00, 06, 12, 18 UTC)
- **Temporal output resolution:** 6-hourly (stated in the guide for the 06Z module; the per-cycle figures in the guide's Table 1 are unreliable — see *Notes*)

For the full native technical specification, see the [GFS entry](../../global/usa/gfs.md).

---

## What it provides
Selected GFS forecast fields in GRIB2. The guide explicitly documents the **06Z** download as containing precipitation, winds, wind gusts, and ozone at 6-hourly steps — i.e., a **variable subset**, not the full GFS field set. The variable content of the 00Z, 12Z, and 18Z directories is not documented in the guide and is assumed similar but unverified (see *Notes*).

IDEAM uses these GFS fields for:
- Precipitation forecasting
- Wind circulation at multiple pressure levels (surface, 850, 700, 500, 250 hPa)
- Synoptic-scale atmospheric analysis
- Initial and boundary conditions for the regional [WRF system](./wrf-ideam.md)

---

## Data availability
- **Is the data free?** Yes
- **License:** TBD. No explicit license is stated on the modeling portal. IDEAM's general open-data section describes its data as published under an open license without legal restrictions, but that statement is attached to its geographic open-data portal rather than this modeling portal — treat as unverified for these products until confirmed.
- **Is the data downloadable?** Yes
- **Data formats:** GRIB2
- **Official download location:**
  - Portal root: https://bart.ideam.gov.co/wrfideam/
  - GFS 00Z: https://bart.ideam.gov.co/wrfideam/new_modelo/GFS00COLOMBIA/grib2/
  - GFS 06Z: https://bart.ideam.gov.co/wrfideam/new_modelo/GFS06COLOMBIA/grib2/
  - GFS 12Z: https://bart.ideam.gov.co/wrfideam/new_modelo/GFS12COLOMBIA/grib2/
  - GFS 18Z: https://bart.ideam.gov.co/wrfideam/new_modelo/GFS18COLOMBIA/grib2/

---

## Notes
- **This is a redistribution of NOAA GFS, not an independently run global model.** IDEAM does not operate its own global NWP system. For users outside Colombia, it is generally preferable to access GFS directly from NOAA NOMADS or the NOAA AWS bucket (`s3://noaa-gfs-bdp-pds/`), which carry the full field set at native resolution.
- **Redistribution subset:** IDEAM distributes specific cycle/resolution/forecast-length combinations (0.5° to 7 days for 00Z/12Z; 0.25° to 15 days for 06Z/18Z) that differ from NOAA's native GFS (0.25° to 16 days for all cycles). These are IDEAM's selections.
- **Guide documents only the 06Z download:** the guide's Descargas section describes a "GFS 06Z" module explicitly; the 00Z/12Z/18Z GRIB2 directories listed above exist on the portal but are not described in the guide, so their exact variable subset and cadence are inferred (TBD).
- **Guide Table 1 "Resolución temporal" appears corrupted:** the column reads 6 h / 7 h / 8 h / 9 h for the four GFS rows (and 1–5 h for the WRF rows) in lockstep with row order, which is not plausible. The 6-hourly cadence used above comes from the guide's prose description of the 06Z download.
- **Update times** depend on the availability and download speed of upstream NOAA GFS data; the HLC times above are approximate (the guide presents them as the most frequent update time, not a guarantee).
- **Companion / sibling entries:**
  - Drives IDEAM's regional [WRF system](./wrf-ideam.md).
  - Another GFS redistribution in this repository: [GFS (CWA, Taiwan)](../taiwan/gfs-cwa.md).
  - Primary source model: [GFS](../../global/usa/gfs.md).

---

## Official documentation
- IDEAM modeling portal: https://bart.ideam.gov.co/wrfideam/
- Ruiz, J.F. & Melo, J.Y. (2025), *Guía rápida de usuario para consultar el portal de modelación numérica de tiempo y clima del IDEAM*, Subdirección de Meteorología, IDEAM
- Primary GFS documentation: see the [GFS entry](../../global/usa/gfs.md)
