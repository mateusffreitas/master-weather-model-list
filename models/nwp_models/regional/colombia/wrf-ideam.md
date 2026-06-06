# WRF (IDEAM Regional Forecast System)

## What this model is
IDEAM operates a regional numerical weather prediction system based on the Weather Research and Forecasting (WRF) model to provide high-resolution short-range deterministic forecasts for Colombia and surrounding regions.

The system dynamically downscales global GFS guidance over a set of nested domains to better represent regional topography, convection, and local-scale weather. WRF output forms the basis for IDEAM's national and subnational forecast products.

---

## Who runs it
- **Organization:** Instituto de Hidrología, Meteorología y Estudios Ambientales (IDEAM), Grupo de Modelamiento Numérico de Tiempo y Clima
- **Country / region:** Colombia

---

## What area it covers
- **Coverage:** Colombia and surrounding regions (tropical northern South America, adjacent Caribbean and Pacific)
- **Publicly distributed NetCDF domains:**
  - **Colombia (00Z):** ~10 km
  - **Colombia (12Z):** ~20 km — see *Notes* on the domain-naming discrepancy
  - **Cundinamarca (00Z):** ~5 km
  - **Bogotá (00Z):** ~1.6 km
- **Domain details (Colombia 00Z, 10 km — from sample output):**
  - **Projection:** Mercator (`MAP_PROJ = 3`)
  - **Grid center:** ~4.705°N, 74.15°W; `TRUELAT1 = 4.705°`, `STAND_LON = −74.15°`
  - **Grid dimensions:** 249 × 249 mass points (250 × 250 staggered), `DX = DY = 10 km`
  - **Approximate extent (derived from grid geometry, not from official bounds):** ~6.5°S–15.7°N, ~85.3°W–63.0°W
- **Not publicly distributed:** a ~15 km "Centro del país" parent domain that serves as the mother domain for the Cundinamarca/Bogotá downscaling is explicitly noted in the guide as not available to the public.

---

## Basic details
- **Model type:** Deterministic regional NWP
- **Model system / core:** WRF (ARW core), version 4.0 (output `TITLE` reads "OUTPUT FROM WRF V4.0 MODEL")
- **Dynamical formulation:** Non-hydrostatic
- **Convection-allowing:** No (Colombia 10 km domain uses parameterized cumulus — see *Physics configuration*; the 5 km and 1.6 km nests may differ — TBD)
- **Horizontal resolution:**
  - Colombia (00Z): ~10 km
  - Colombia (12Z): ~20 km
  - Cundinamarca: ~5 km
  - Bogotá: ~1.6 km
- **Vertical levels:** 32 eta (mass) levels (`BOTTOM-TOP_GRID_DIMENSION = 33`), Colombia 10 km domain
- **Forecast length:** ~7 days per the user guide; the sample 00Z Colombia file contains hourly output to +192 h (8 days) — see *Notes*
- **Update frequency / cycles:** 00 UTC (Colombia, Cundinamarca, Bogotá) and 12 UTC (Colombia/Caribbean domain). Approximate availability: ~03:00 local (HLC, UTC−5) for 00Z runs, ~14:00 local for the 12Z run
- **Temporal output resolution:** 1 h (Colombia 10 km domain, confirmed from sample output: 193 hourly timesteps). Per-domain figures in the user guide's Table 1 appear unreliable — see *Notes*

---

## Data assimilation
- **Data assimilation:** No. The sample is a `REAL-DATA CASE` with FDDA, observational nudging, and DFI all disabled (`GRID_FDDA = 0`, `OBS_NUDGE_OPT = 0`, `DFI_OPT = 0`); the regional run inherits the driving global analysis.

---

## Initial and boundary conditions (for limited-area models)
- **Initial conditions:** NCEP GFS (FV3 core), as distributed/used by IDEAM
- **Boundary conditions:** NCEP GFS (FV3); update cadence TBD

---

## Physics configuration
Parameterizations decoded from the 00Z Colombia 10 km sample output (other domains/cycles may differ — TBD):
- **Microphysics:** WSM6 — WRF Single-Moment 6-class (`MP_PHYSICS = 6`)
- **Longwave radiation:** RRTMG (`RA_LW_PHYSICS = 4`)
- **Shortwave radiation:** RRTMG (`RA_SW_PHYSICS = 4`)
- **Surface layer:** Revised MM5 Monin–Obukhov (`SF_SFCLAY_PHYSICS = 91`)
- **Land surface model:** Noah LSM, 4 soil layers (`SF_SURFACE_PHYSICS = 2`)
- **Planetary boundary layer:** YSU — Yonsei University (`BL_PBL_PHYSICS = 1`)
- **Cumulus:** New Tiedtke (`CU_PHYSICS = 16`) — i.e., deep convection is parameterized at 10 km, not explicitly resolved
- **Gravity-wave drag:** On (`GWD_OPT = 1`)
- **Model time step:** 60 s (`DT = 60`)

---

## What it provides
High-resolution deterministic forecasts. The public NetCDF output is the full raw WRF model state — the user guide cites "more than 80 meteorological variables" (the sample 00Z Colombia file contains 204 variables), intended for processing with tools such as wrf-python, NCL, CDO, NCO, GrADS/ARWPost, or Panoply. Core fields include:
- Precipitation (resolved + convective)
- Near-surface temperature, humidity, and 10 m wind
- 3D wind, temperature, geopotential, and moisture on model levels
- Cloud fields, radiation fluxes, and land-surface/soil state

IDEAM also derives GeoTIFF products (24 h accumulated precipitation, ET0, min/mean/max temperature, mean/max wind) and hourly GeoTIFF fields (humidity, precipitation, temperature, wind, cloud cover) from the same WRF output; those are distributed separately from the NetCDF directories listed here.

---

## Data availability
- **Is the data free?** Yes
- **License:** TBD. No explicit license is stated on the modeling portal. IDEAM's general open-data section describes its data as published under an open license without legal restrictions, but that statement is attached to its geographic open-data portal rather than this modeling portal — treat as unverified for the WRF products until confirmed.
- **Is the data downloadable?** Yes
- **Data formats:** NetCDF (raw WRF output). GeoTIFF derived products are also available elsewhere on the portal.
- **Official download location:**
  - Portal root: https://bart.ideam.gov.co/wrfideam/
  - Colombia (00Z): https://bart.ideam.gov.co/wrfideam/new_modelo/WRF00COLOMBIA/netcdf/
  - Colombia (12Z): https://bart.ideam.gov.co/wrfideam/new_modelo/WRF12COLOMBIA/netcdf/
  - Cundinamarca (00Z): https://bart.ideam.gov.co/wrfideam/new_modelo/WRF00CUNDINAMARCA/netcdf/
  - Bogotá (00Z): https://bart.ideam.gov.co/wrfideam/new_modelo/WRF00BOGOTA/netcdf/

---

## Notes
- **Domain-naming discrepancy (verify):** the user guide's Table 1 describes the 12Z run as covering the **Caribbean at 20 km**, but the public download directory is named `WRF12COLOMBIA`. Whether this domain is Caribbean-centered, Colombia-centered, or a relabeled superset is unresolved from the available documentation.
- **Guide Table 1 "Resolución temporal" appears corrupted:** the column reads 1 h / 2 h / 3 h / 4 h / 5 h for the five WRF rows (and 6/7/8/9 h for the four GFS rows) in lockstep with row order, which is not plausible as genuine output cadence. The Colombia 10 km hourly cadence used above is taken from the sample file, not this table. Per-domain temporal resolution for the other nests is TBD.
- **Forecast length discrepancy:** the guide states 7 days; the sample 00Z Colombia file contains 193 hourly timesteps (to +192 h / 8 days). Minor, but unresolved.
- **Physics settings are domain/cycle-specific:** all parameterization values above come from one 00Z Colombia 10 km file (sim. start 2020-05-18). The 5 km and 1.6 km nests in particular may use different cumulus settings; do not assume these carry across domains.
- **Driving model:** the same IDEAM portal redistributes the [GFS (IDEAM)](./gfs-ideam.md) global guidance that initializes and bounds this WRF system.
- **Sibling system:** for another operational South American regional WRF in this repository, see [WRF-SMN Argentina](../argentina/wrf-smn.md).

---

## Official documentation
- IDEAM modeling portal: https://bart.ideam.gov.co/wrfideam/
- Ruiz, J.F. & Melo, J.Y. (2025), *Guía rápida de usuario para consultar el portal de modelación numérica de tiempo y clima del IDEAM*, Subdirección de Meteorología, IDEAM
- Ruiz, J.F., Arango, C. & Kilpinen, J., *Verificación del modelo WRF que opera IDEAM*, IDEAM: http://www.ideam.gov.co/documents/21021/21132/VerificacionWRF.pdf
