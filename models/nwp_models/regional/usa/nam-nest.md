# NAM Nest (North American Mesoscale – High Resolution Nests)

> ⚠️ **Scheduled for retirement.** The NAM Nest is proposed for discontinuation and replacement by the [RRFS](./rrfs.md) per [NWS Public Information Statement 25-41](https://www.weather.gov/media/notification/pdf_2025/pns25-41_RRFS_legacy_model_cessation.pdf) (June 26, 2025). All five nest domains (CONUS, Alaska, Hawaii, Puerto Rico, and the moveable fire weather nest) are included in the retirement.

## What this model is
The NAM Nest is the **collective name for five high-resolution convection-allowing nested domains** run inside each cycle of the [NAM](./nam.md) (North American Mesoscale) production system. Despite its plural nature, "NAM Nest" is commonly used as a singular name in operational practice.

The four **fixed nests** (CONUS, Alaska, Hawaii, Puerto Rico) run at 3 km horizontal resolution. The fifth **moveable nest** runs at 1.5 km horizontal resolution and is repositioned each cycle — typically to support fire-weather operations during summer months at locations selected by the National Interagency Fire Center, and to support specific NWS Service Center, Regional, or Forecast Office needs at other times of year. All nests are convection-allowing (no parameterized convection) and all are one-way nested inside the parent 12 km [NAM](./nam.md) grid.

The NAM Nest has been operationally **frozen since the v4.0 upgrade in March 2017**. NCEP redirected its mesoscale modeling resources at that time toward developing the FV3-based regional system that ultimately became the [RRFS](./rrfs.md). With NAM Nest retirement, convection-allowing guidance for these regions is being transitioned to RRFS (CONUS, AK), and convection-allowing guidance for Hawaii and Puerto Rico is being transitioned to RRFS at 2.5 km resolution.

---

## Who runs it
- **Organization:** NOAA / National Weather Service (NCEP)
- **Country / region:** United States

---

## What area it covers
- **Coverage:** Five nested domains at high resolution, all run within a single NAM production cycle
  - **CONUS nest** — 3 km, contiguous United States
  - **Alaska nest** — 3 km
  - **Hawaii nest** — 3 km
  - **Puerto Rico nest** — 3 km
  - **Fire weather nest** — 1.5 km, moveable; placed at different locations each cycle over CONUS or Alaska based on operational needs

The four fixed nests cover the same domains as the equivalent NWS forecast areas of responsibility. The fire weather nest is specifically targeted to provide high-resolution guidance to **Incident Meteorologists (IMETs)** dispatched to wildfire locations.

---

## Basic details
- **Model type:** Regional deterministic NWP (convection-allowing nested high-resolution domains)
- **Model system / core:** NEMS NMMB — NOAA Environmental Modeling System version of the **Non-Hydrostatic Multi-scale Model on B-grid**
- **Dynamical formulation:** Non-hydrostatic, on the Arakawa B-grid horizontal staggering
- **Convection-allowing:** Yes — deep convection is explicitly resolved at 3 km (fixed nests) and 1.5 km (fire weather nest); no cumulus parameterization is used in any nest
- **Native horizontal resolution:**
  - 3 km — CONUS, Alaska, Hawaii, Puerto Rico fixed nests
  - 1.5 km — fire weather moveable nest
- **Vertical levels:** 60 (matching the NAM 12 km parent)
- **Forecast length:**
  - 60 hours for the four fixed 3 km nests
  - 36 hours for the 1.5 km fire weather nest
- **Update frequency:** 4× daily (00, 06, 12, 18 UTC) — all nests run once per parent NAM cycle
- **Temporal output resolution:** Hourly

---

## Data assimilation
The NAM Nest's DA configuration differs by domain:

### CONUS and Alaska nests — fully cycled with NDAS hybrid 3DEnVar
The 3 km CONUS and Alaska nests are initialized using the **NCEP hybrid variational ensemble analysis (NDAS, the NAM Data Assimilation System)** with a 6-hour cycle and hourly analysis updates. This is the same hybrid 3DEnVar GSI-based system used to initialize the parent 12 km NAM, with flow-dependent background-error covariances drawn from the GDAS-EnKF ensemble (the same one feeding [GFS](../../global/usa/gfs.md) and [RAP](./rap.md) hybrid DA).

Assimilated observations include conventional surface, aircraft, and radiosonde data; satellite radiances; GPS-RO; and additional mesoscale observations.

### Hawaii, Puerto Rico, and fire weather nests — non-cycled
These three nests **do not run their own DA cycle**. They are initialized using a **first guess interpolated from the parent 12 km NAM domain**, inheriting whatever analysis state the 12 km NAM produced for that cycle. This makes them more responsive to the parent model's behavior and limits their ability to assimilate region-specific observations directly.

---

## Initial and boundary conditions
- **Initial conditions:**
  - CONUS, Alaska: NDAS hybrid 3DEnVar analysis (cycled)
  - Hawaii, Puerto Rico, Fire Weather: First guess from the 12 km parent NAM analysis (non-cycled)
- **Lateral boundary conditions:** All five nests receive boundaries from the parent 12 km NAM, run as one-way nests inside the parent integration

---

## Physics suite
The NAM Nest uses essentially the same physics suite as the parent 12 km NAM, with several nest-specific modifications:

- **Microphysics:** Ferrier-Aligo bulk scheme (NCEP-developed, operational in NAM since August 2014). Nest-specific tuning includes:
  - 100% RH threshold for condensation onset (vs 98% in the 12 km parent)
  - Each microphysics species advected separately (vs total condensate in parent)
- **Convection:** No parameterized deep convection in any nest (resolved explicitly at 3 km and 1.5 km)
- **Gravity wave drag and mountain blocking:** Not used in nests (active in the 12 km parent)
- **Divergence damping:** Higher coefficient than the 12 km parent
- **Land surface:** Noah land surface model
- **Boundary layer:** MYJ (Mellor-Yamada-Janjic) scheme

---

## What it provides
Convection-allowing forecasts of:
- Near-surface temperature, humidity, wind, and pressure
- Convection and severe storm evolution (explicitly resolved)
- Precipitation, precipitation type, and accumulation
- Cloud cover, ceilings, visibility
- Composite radar reflectivity and simulated radar fields
- Convective indices (CAPE, CIN, helicity, updraft helicity)
- Aviation-relevant fields including ceilings, visibility, wind gusts
- Fire-weather-relevant fields (specifically targeted by the 1.5 km fire weather nest)

Unlike [HRRR](./hrrr.md), the NAM Nest covers Hawaii and Puerto Rico operationally (HRRR's Hawaii and Caribbean domains are experimental, not operational). This makes the NAM Nest particularly important for those regions until the [RRFS](./rrfs.md) transition completes.

---

## Relationship to other models
- **[NAM](./nam.md):** Parent model. The NAM Nest is one part of a single integrated NAM production execution that produces both the 12 km parent and all five nests in one run.
- **[HRRR](./hrrr.md):** Operational counterpart for CONUS and Alaska convection-allowing guidance. HRRR is hourly-updating and uses different DA (HRRRDAS), while the NAM Nest is 4× daily and shares NDAS DA with the parent NAM. The two systems are complementary: HRRR's hourly updates are useful for short-range forecasting while the NAM Nest provides convection-allowing guidance for Hawaii and Puerto Rico that HRRR doesn't operationally cover.
- **[HiresW Guam](./hiresw-guam.md):** Operational convection-allowing guidance for Guam (after the broader HiresW retirement); not produced by the NAM Nest.
- **[HREF](./href.md):** Multi-model regional ensemble that uses some NAM Nest output as members.
- **[NBM](./nbm.md):** Uses NAM Nest fields as one of many deterministic inputs.
- **[RRFS](./rrfs.md):** Future replacement. RRFSv1 (proposed for early 2026) covers CONUS at 3 km, Alaska at 3 km, Hawaii at 2.5 km, and Puerto Rico at 2.5 km, providing functionally equivalent convection-allowing guidance to all NAM Nest domains except the moveable fire weather nest. Many NAM Nest output grids will continue to be produced from RRFS output via post-processing.

---

## Data availability
- **Is the data free?** Yes
- **Is the data downloadable?** Yes
- **License:** Public domain (U.S. government work; CC0-equivalent)
- **Data formats:** GRIB2
- **Official download locations:**
  - **NOMADS:** https://nomads.ncep.noaa.gov/pub/data/nccf/com/nam/prod/ (real-time, ~10-day rolling)
  - **NCEI archive:** https://www.ncei.noaa.gov/products/weather-climate-models/north-american-mesoscale (long-term archive)
  - **AWS Open Data:** https://registry.opendata.aws/noaa-nam/

NAM Nest output is distributed as separate GRIB2 files per nest within the parent NAM directory structure.

---

## Status and retirement timeline
- **Proposed for full retirement** in NWS Public Information Statement 25-41 (June 26, 2025), as part of the broader NAM system retirement.
- All five nest domains (CONUS, AK, HI, PR, fire weather) are included in the proposal.
- **Replacement deterministic guidance** at convection-allowing scales is provided by [RRFS](./rrfs.md) (3 km CONUS and Alaska, 2.5 km Hawaii and Puerto Rico).
- The **moveable fire weather nest at 1.5 km** does **not** have a direct equivalent in RRFSv1 — its functionality may be addressed differently in future RRFS versions or through other post-processing.
- Some NAM Nest output grids will continue to be produced from RRFS output via post-processing to ease the transition; others will be fully discontinued. EMC has published lists of retained vs. fully discontinued grids:
  - Retained NAM Nest output grids: https://www.emc.ncep.noaa.gov/mmb/mpyle/rrfs_info/namnest_grids.txt
- Originally targeted for retirement in early 2026 alongside RRFSv1 operational implementation; timeline has slipped along with RRFSv1's implementation date.

---

## Notes
- The NAM Nest replaced the older ~5 km NAM CONUS nest and was the first 3 km convection-allowing operational guidance NCEP produced for Hawaii and Puerto Rico — both of which lacked operational convection-allowing coverage prior to the NAM Nest era.
- The NAM Nest is convection-allowing at 3 km and 1.5 km, similar in resolution to [HRRR](./hrrr.md), but uses entirely different dynamical core (NMMB B-grid vs WRF-ARW), microphysics (Ferrier-Aligo vs Thompson), and data assimilation (NDAS vs HRRRDAS) approaches. The two systems' forecast differences for any given storm reflect both initial-condition and model-formulation differences, not just resolution.
- The NAM Nest has been **frozen since the v4.0 implementation in March 2017**, when NCEP redirected mesoscale development resources toward the FV3-based RRFS effort. No scientific changes have been made since.
- The 1.5 km fire weather nest is one of the relatively few operational moveable-domain NWP forecasts globally — most operational systems use fixed domains. This makes it operationally distinctive but also means its retirement will leave a temporary gap in U.S. operational moveable-nest fire weather guidance until equivalent capability is established under RRFS.
- The NMMB dynamical core was developed at NCEP/EMC and has been used in operational NAM since 2011. The B-grid Arakawa staggering and non-hydrostatic formulation differ structurally from the WRF-ARW core used by HRRR and the FV3 core used by RRFS.

---

## Recent version history

### NAM v4.0 — operational 21 March 2017 (current; final version)
The last NAM upgrade. Headline NAM Nest changes:
- **CONUS nest resolution increased from 4 km to 3 km**
- **Alaska nest resolution increased from 6 km to 3 km**
- Updated Ferrier-Aligo microphysics (improved stratiform precipitation, more realistic radar reflectivity, reduced CONUS warm-season high precipitation bias)
- Model stability improvements (more frequent moist physics and radiation calls, mix-out of superadiabatic layers — partly motivated by NAM CONUS nest failures during Hurricane Joaquin in 2015)
- Land-surface and radiation changes targeting cool-season surface temperature/dewpoint biases
- The 1.5 km fire weather nest was retained and its forecast length kept at 36 hours

### Earlier history
- The NAM transitioned from WRF-NMM to the NMMB dynamical core in October 2011
- The first 4 km CONUS nest was added in 2012
- Hawaii, Puerto Rico, and Alaska nests were added in subsequent NAM versions
- The fire weather nest was introduced as a moveable domain to provide IMET-targeted high-resolution guidance during fire incidents

---

## Official documentation
- NAM at EMC: https://emc.ncep.noaa.gov/emc/pages/numerical_forecast_systems/nam.php
- NAM Vlab page: https://vlab.noaa.gov/web/emc/nam
- NAM v4.0 release notes: https://www.weather.gov/media/nws/Public_release_notes_NAM.v4.0.pdf
- NWS PNS 25-41 (NAM retirement notice): https://www.weather.gov/media/notification/pdf_2025/pns25-41_RRFS_legacy_model_cessation.pdf
- Retained NAM Nest output grids: https://www.emc.ncep.noaa.gov/mmb/mpyle/rrfs_info/namnest_grids.txt
- AWS Open Data: https://registry.opendata.aws/noaa-nam/

### Key references
- Aligo, E. A., B. Ferrier, and J. R. Carley (2018). *Modified NAM Microphysics for Forecasts of Deep Convective Storms.* Mon. Wea. Rev., 146, 4115–4153. https://doi.org/10.1175/MWR-D-17-0277.1
- Janjic, Z., and R. Gall (2012). *Scientific documentation of the NCEP Nonhydrostatic Multiscale Model on the B grid (NMMB), Part 1: Dynamics.* NCAR/TN-489+STR.
