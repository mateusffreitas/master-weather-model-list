# UKMO Global (Unified Model — Global Deterministic)

## What this model is
The UKMO Global model is the global deterministic numerical weather prediction system operated by the UK Met Office.

It is a global configuration of the **Unified Model (UM)** — the Met Office's flagship NWP and climate modelling suite, named because the same model code is used across timescales from nowcasting to climate projections, and across spatial scales from kilometre-scale convective forecasting to centennial Earth system runs.

UKMO Global provides medium-range deterministic forecasts of atmospheric conditions worldwide and is the parent model that supplies lateral boundary conditions for the Met Office's regional UK Variable-resolution model ([UKV](../../regional/uk/ukv.md)), as well as for several Unified Model deployments operated by partner agencies (notably the Australian Bureau of Meteorology and the Korea Meteorological Administration).

The current operational version is part of **Operational Suite 47 (OS47)**, implemented on 21 January 2026 — the Met Office's first major science upgrade in over three years and the first run on its new Microsoft Azure-based supercomputer.

---

## Who runs it
- **Organization:** Met Office
- **Country / region:** United Kingdom

---

## What area it covers
- **Coverage:** Global

---

## Basic details
- **Model type:** Global deterministic NWP
- **Model system / core:** Unified Model (UM), in its Global Coupled 5 (GC5) science configuration as of OS47
- **Dynamical formulation:** Non-hydrostatic, fully compressible deep-atmosphere equations on a regular latitude–longitude grid (ENDGame dynamical core); semi-Lagrangian, semi-implicit time integration
- **Convection-allowing:** No (deep convection is parameterized at ~10 km resolution)
- **Native horizontal grid:** N1280 (2560 × 1920 grid points; ~0.09° resolution; ~10 km in mid-latitudes)
- **Vertical levels:** 70
- **Forecast length:**
  - 168 hours (7 days) for 00 and 12 UTC cycles
  - 67 hours for 06 and 18 UTC cycles
- **Update frequency:** 4× daily (00, 06, 12, 18 UTC)
- **Temporal output resolution (Open Data):**
  - Hourly to +54 h
  - 3-hourly from +57 to +144 h
  - 6-hourly from +150 to +168 h
- **Data latency:** Typically 3–6 hours after model run time

---

## Data assimilation
UKMO Global uses **hybrid 4D-Var** data assimilation, combining a static background error covariance with flow-dependent covariances derived from the Met Office's global ensemble (MOGREPS-G).

OS47 introduced the **JOPA** (Joint Observation Processing Approach) code, the new JEDI-based observation processing system from the Met Office's Next Generation Modelling System (NGMS), replacing the legacy OPS code. OS47 also added assimilation of new **Mode-S aircraft observations** for improved upper-air wind coverage.

---

## What it provides
Deterministic global forecasts of:
- Temperature (surface, screen-level, and upper-air)
- Wind components (10 m and upper-air on pressure and height levels)
- Specific and relative humidity
- Surface and mean sea level pressure
- Total precipitation, precipitation rate, and precipitation type
- Cloud cover (total, high, mid, low) and cloud base / ceiling
- Surface fluxes, radiation components, and boundary-layer diagnostics
- Visibility, wind gusts, and aviation-relevant fields

UKMO Global output also serves as boundary forcing for the Met Office's regional UKV deterministic and MOGREPS-UK ensemble systems.

---

## Data availability

### Free Open Data — AWS Open Data Registry
- **Free?** Yes
- **License:** Creative Commons Attribution-ShareAlike 4.0 (CC BY-SA 4.0); attribution and ShareAlike required (note: this differs from ECMWF's CC-BY-4.0 by adding the ShareAlike obligation)
- **Resolution:** ~10 km native (no downsampling)
- **Format:** NetCDF (CF-compliant)
- **Retention:** **2-year rolling archive** — significantly longer than typical operational open-data services, which usually retain only the most recent few days. The archive contains data from December 2023 onward.
- **Official location:**
  - https://registry.opendata.aws/met-office-global-deterministic/

### Microsoft Planetary Computer
A second, parallel distribution of the same data was launched on the Microsoft Planetary Computer in February 2026:
- **Near-surface collection:** https://planetarycomputer.microsoft.com/dataset/met-office-global-deterministic-near-surface
- **Height levels collection:** https://planetarycomputer.microsoft.com/dataset/met-office-global-deterministic-height
- **Pressure levels collection:** Available as a separate Planetary Computer collection
- The Planetary Computer release includes a 2-year historical archive (>600 TB combined with the UKV dataset), pitched at researchers training and evaluating AI weather models against authoritative forecasts.

### Caveats for both channels
The data is offered on a free, **unsupported** basis — the Met Office does not recommend either distribution for critical business purposes. Service-desk support is available Mon-Fri 09:00-17:00 UTC with a 3-5 business day target response time, and only for non-operational queries.

---

## Notes
- The Unified Model is grid-point (rather than spectral) and is unusual in spanning weather and climate timescales with a single codebase. The same model is used by the Met Office for centennial climate predictions in the IPCC assessment reports as for tomorrow's short-range forecast.
- UKMO Global runs on the Met Office's **new Microsoft Azure-based supercomputer**, which became operational in 2024 and on which OS47 was the first major modelling suite to run.
- The Met Office uses a science configuration scheme for the UM: the global model uses **Global Coupled 5 (GC5)** as of OS47 — earlier configurations included GC2, GC3, and GC4 (operational from May 2022 to January 2026). GC5 is documented in Willett et al. (2025).
- The asymmetric forecast length (168 h at 00/12 UTC vs 67 h at 06/18 UTC) is structurally similar to the equivalent IFS, ICON, and GFS schedules — the longer runs at 00/12 anchor medium-range guidance, while the 06/18 runs target short-range applications.
- The Unified Model is also operated by partner agencies including the **Australian Bureau of Meteorology** (ACCESS-G global at 12.5 km) and the **Korea Meteorological Administration** (KMA Unified Model global at ~10 km).

---

## Recent version history

### OS47 / PS47 / GC5 — operational 21 January 2026 (current)
First major science upgrade in over three years, and the first running on the Met Office's new Microsoft Azure supercomputer. Highlights for the global deterministic model:
- New **GC5** science configuration (replacing GC4 which had been operational since May 2022)
- Stronger performance in temperature, winds, convection, and monsoon representation
- More realistic tropical cyclone structure (deeper, better-defined storms)
- Reduction in unrealistic precipitation "spikes"
- Improved near-surface temperature
- New **Mode-S aircraft observation** assimilation for better upper-air winds
- New **JOPA** (JEDI-based) observation processing replacing the legacy OPS code — the first operational deployment of components from the Met Office's Next Generation Modelling System (NGMS)
- Updated ocean data assimilation
- Companion change: MOGREPS-G global ensemble extended from 7 to 10 days (see [MOGREPS-G entry](../../../ensemble_models/global/uk/mogreps-g.md))
- Open-data product changes: the `height_asl_on_pressure_levels` parameter was replaced by `geopotential_height_on_pressure_levels`; some parameter ID changes for freezing rain accumulation
- Operational data availability is approximately 10–20 minutes later than under OS46

### OS46 / GC4 — operational May 2022 to January 2026
Previous operational configuration with GC4 science, succeeded by OS47. GC4 was the first global coupled configuration with the new ENDGame dynamical core matured for production use.

### Earlier history
The Unified Model entered Met Office operational service in 1991 as the world's first NWP model designed to span weather-and-climate timescales with a single codebase. The current ENDGame dynamical core replaced the older "New Dynamics" formulation in 2014. The horizontal resolution upgrade to N1280 (~10 km) was implemented as part of the 2018 supercomputer transition.

---

## Official documentation
- Met Office numerical weather prediction overview: https://www.metoffice.gov.uk/research/approach/modelling-systems/unified-model/weather-forecasting
- Parallel Suite 47 (PS47) overview: https://www.metoffice.gov.uk/services/data/parallel-suite-47-ps47-overview
- Met Office Weather DataHub upcoming changes: https://datahub.metoffice.gov.uk/support/changes-and-updates
- Met Office services data pages: https://www.metoffice.gov.uk/services/data
- AWS Open Data Registry: https://registry.opendata.aws/met-office-global-deterministic/

### Key references
- Willett, M. R., et al. (2025). *The Met Office Unified Model Global Atmosphere 8.0 and JULES Global Land 9.0 configurations.* EGUsphere preprint. https://doi.org/10.5194/egusphere-2025-1829
- Walters et al. (2019). *The Met Office Unified Model Global Atmosphere 7.0/7.1 and JULES Global Land 7.0 configurations.* GMD, 12, 1909–1963.
- Williams et al. (2018). *The Met Office Global Coupled model 3.0 and 3.1 (GC3.0 and GC3.1) configurations.* JAMES, 10, 357–380.
- Wood et al. (2014). *An inherently mass-conserving semi-implicit semi-Lagrangian discretization of the deep-atmosphere global non-hydrostatic equations.* QJRMS, 140, 1505–1520. (Documents the ENDGame dynamical core)
