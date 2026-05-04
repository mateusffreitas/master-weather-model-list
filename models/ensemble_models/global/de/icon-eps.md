# ICON-EPS (ICON Global Ensemble Prediction System)

## What this model is
ICON-EPS is DWD's operational global ensemble prediction system, designed to quantify forecast uncertainty in short- to medium-range weather prediction.

It is the ensemble counterpart of the deterministic [ICON](../../../nwp_models/global/germany/icon-global.md) global model, sharing the same dynamical core, the same triangular icosahedral grid, and the same tightly-coupled European nest architecture. ICON-EPS produces 40 ensemble members on a global ~26 km grid with a higher-resolution (~13 km) European nest embedded within the same forecast integration. The European-nest portion is distributed separately as [ICON-EU-EPS](../../regional/de/icon-eu-eps.md).

ICON-EPS has been operational since January 2018, and serves a dual purpose: producing probabilistic forecasts directly, and providing initial conditions and boundary forcing to several downstream DWD systems including the [ICON-D2-EPS](../../regional/de/icon-d2-eps.md) convection-permitting ensemble.

The current configuration reflects DWD's **23 November 2022 resolution upgrade**, which increased horizontal resolution from 40 km to 26 km globally and from 20 km to 13 km over the European nest, while raising the vertical level count from 90 to 120.

---

## Who runs it
- **Organization:** Deutscher Wetterdienst (DWD — German Weather Service)
- **Country / region:** Germany

---

## What area it covers
- **Coverage:** Global
- **Nested subdomain:** European nest at higher resolution. The nest portion is distributed separately as [ICON-EU-EPS](../../regional/de/icon-eu-eps.md) (full ICON-EU-EPS domain extends 23.5°W – 62.5°E, 29.5°N – 70.5°N).

---

## Basic details
- **Model type:** Global ensemble NWP
- **Model system / core:** ICON (Icosahedral Nonhydrostatic) — same model core as the deterministic ICON
- **Dynamical formulation:** Non-hydrostatic, on a triangular (icosahedral) horizontal grid
- **Convection-allowing:** No (deep convection is parameterized at ~26 km resolution)
- **Ensemble size:** 40 members
- **Native horizontal grid:**
  - Global: R3B06 triangular grid, ~26 km resolution
  - European nest: R3B07 triangular grid, ~13 km resolution
- **Public output grids:**
  - Native icosahedral (triangular) grid (GRIB2)
  - Regular latitude–longitude grids interpolated from native (selected element packages)
- **Vertical levels:** 120 (since 23 November 2022; 90 levels prior)
- **Vertical coordinate:** Smooth-Level Vertical (SLEVE) terrain-following hybrid coordinate; flat (height-based) above 16 km
- **Model top:** 75 km
- **Forecast length:**
  - 180 hours (7.5 days) for 00 and 12 UTC cycles
  - 120 hours (5 days) for 06 and 18 UTC cycles
  - 30 hours for the four intermediate cycles (03, 09, 15, 21 UTC), used primarily to provide boundary conditions to downstream systems
- **Update frequency:** 8× daily — 4 main cycles (00, 06, 12, 18 UTC) plus 4 intermediate cycles (03, 09, 15, 21 UTC)
- **Temporal output resolution:**
  - Hourly to +78 h
  - 3-hourly from +81 to +120 h
  - 6-hourly or 12-hourly for selected fields from +120 to +180 h

---

## Initial conditions
ICON-EPS is initialized by an **Ensemble Data Assimilation (EDA)** system based on a **Local Ensemble Transform Kalman Filter (LETKF)**, the ensemble component of DWD's hybrid LETKF + EnVar data assimilation system (see the [ICON entry](../../../nwp_models/global/germany/icon-global.md#data-assimilation) for details on the deterministic counterpart).

The EDA operates on a 3-hour assimilation cycle, producing 40 slightly different analysis states at the end of each cycle that become the initial conditions for the next ensemble forecast. Each ensemble member is integrated forward independently.

The same 40-member LETKF ensemble that initializes ICON-EPS also provides the flow-dependent background-error covariances used by the deterministic ICON's hybrid EnVar analysis — meaning ICON-EPS and the deterministic ICON are tightly coupled through their shared analysis system.

---

## Perturbations and design
- **Initial condition perturbations:** Each member is initialized from a different LETKF analysis (see above).
- **Model uncertainty representation:** Stochastic **physics parameter perturbations**. Before each model run, a set of defined parameter perturbations is randomly assigned to the ensemble members. A constraint ensures that each forecast run perturbs the same total number of parameters, while the distribution across members varies run-to-run. Parameters are not varied spatially or temporally during a single forecast — they are fixed at the start of each member's integration.
- **Note on stochastic schemes:** Unlike ECMWF (SPP) or NCEP (SPPT + SKEB), ICON-EPS does not currently use stochastic perturbation of tendencies or stochastic kinetic energy backscatter. Model uncertainty is represented purely through static parameter perturbations applied at the start of each run.

### Use as boundary conditions for regional ensembles
ICON-EPS provides boundary conditions for DWD's regional ensembles:
- The European-nest portion is the [ICON-EU-EPS](../../regional/de/icon-eu-eps.md) regional ensemble
- Forecasts from members 1–20 of ICON-EPS provide lateral boundary conditions for [ICON-D2-EPS](../../regional/de/icon-d2-eps.md), DWD's 2.2 km convection-permitting ensemble

---

## What it provides
Probabilistic global forecasts including:
- Individual ensemble member forecasts (40 members)
- Ensemble mean and ensemble spread
- Surface and upper-air fields: temperature, wind components, geopotential height, specific and relative humidity, surface and mean sea level pressure
- Total precipitation, precipitation type, and convective precipitation
- Cloud cover (total, high, mid, low) and hydrometeor fields
- Surface fluxes, radiation components, and boundary-layer diagnostics
- Probability products derived from the 40-member ensemble (exceedance probabilities, percentile fields)
- Lateral boundary conditions for [ICON-EU-EPS](../../regional/de/icon-eu-eps.md) (via the European nest) and [ICON-D2-EPS](../../regional/de/icon-d2-eps.md) (via members 1–20)
- Flow-dependent background-error covariances for the deterministic [ICON](../../../nwp_models/global/germany/icon-global.md) hybrid LETKF + EnVar data assimilation

---

## Relationship to other models
- **[ICON Global](../../../nwp_models/global/germany/icon-global.md):** Deterministic counterpart, sharing the dynamical core, vertical structure, and coupled European nest architecture. The two systems share the LETKF ensemble at the heart of DWD's data assimilation.
- **[ICON-EU-EPS](../../regional/de/icon-eu-eps.md):** The European-nest portion of ICON-EPS, distributed as a separate regional product. Not an independent regional ensemble — it is the regional subset of this same forecast integration.
- **[ICON-D2-EPS](../../regional/de/icon-d2-eps.md):** Convection-permitting regional ensemble at 2.2 km, driven at the boundaries by ICON-EPS members 1–20.
- **[ICON-D2-RUC-EPS](../../regional/de/icon-d2-ruc-eps.md):** Hourly-updating convection-permitting ensemble; like ICON-D2-EPS, it depends on ICON-EPS for boundary forcing.

ICON-EPS is part of DWD's Modular Earth System Model (ICON) framework, jointly developed with the Max Planck Institute for Meteorology, and has been open source under a permissive licence since January 2024 (see the [ICON entry](../../../nwp_models/global/germany/icon-global.md#open-source) for details).

---

## Data availability
- **Is the data free?** Yes
- **License:** CC BY 4.0 (DWD Open Data licence; attribution required)
- **Data formats:** GRIB2
- **Grids available:** Native icosahedral grid (triangular); regular lat-lon grids also distributed for many element packages
- **Official download location:**
  - https://opendata.dwd.de/weather/nwp/icon-eps/grib/
- **Retention note:** DWD retains GRIB2 files on the open data server for a short period only (typically 24 hours). Users needing longer-term archives must set up their own retention.

DWD provides CDO grid description files and weight files for users wanting to interpolate native triangular output to custom regular grids:
- https://opendata.dwd.de/weather/lib/cdo/

---

## Notes
- ICON-EPS and the deterministic [ICON](../../../nwp_models/global/germany/icon-global.md) are not separate models — they share the dynamical core, physics, vertical grid, and the European nest. The two systems are also tightly coupled through their shared LETKF + EnVar data assimilation.
- The 40-member ensemble size is materially smaller than ECMWF's 51-member ENS or NCEP's 31-member GEFS, but ICON-EPS makes up some ground through 8× daily cycling (4 main + 4 intermediate cycles), providing more frequent ensemble updates than most other operational global ensembles.
- The combined main-and-intermediate cycle structure means ICON-EPS produces a new ensemble forecast every 3 hours, primarily to provide fresh boundary conditions to DWD's regional convection-permitting systems. Only the 4 main cycles produce extended-range forecasts to 120 or 180 hours.
- Output should be interpreted probabilistically rather than as a deterministic forecast.
- ICON-EPS runs as a coupled system with the deterministic ICON; the atmospheric component is the focus of this entry. See DWD documentation for the broader ICON modelling framework.

---

## Recent version history

### 23 November 2022 — major resolution upgrade (current configuration)
The most significant ICON-EPS upgrade since operational launch:
- **Global horizontal resolution increased** from 40 km to 26 km (R2B06 → R3B06 grid)
- **European nest horizontal resolution increased** from 20 km to 13 km (R2B07 → R3B07 grid; ICON-EU-EPS portion)
- **Vertical levels increased** from 90 to 120 globally; from 60 to 74 in the European nest
- New high-resolution global orography data (combining MERIT, REMA, GLOBE)
- Adaptive surface friction adjustment
- Vertical resolution increase primarily benefits upper troposphere and stratosphere
- The horizontal resolution upgrade was applied to the EPS only (the deterministic ICON remained at 13 km), bringing ICON-EPS into a resolution range where its skill characteristics align more closely with the deterministic system

### January 2018 — operational launch
ICON-EPS became operational alongside DWD's broader transition to the ICON modelling framework. The original configuration used 40 km global / 20 km nest resolution with 90 vertical levels and the same 40-member design retained today. The system replaced the previous COSMO-based global ensemble.

### Earlier history
ICON-EPS was developed as part of DWD's transition from the legacy GME global model and COSMO-EU regional model (replaced by ICON in January 2015 and ICON-EU in June 2015). The ensemble system followed three years later in January 2018.

---

## Official documentation
- DWD ICON model description: https://www.dwd.de/EN/research/weatherforecasting/num_modelling/01_num_weather_prediction_modells/icon_description.html
- DWD ensemble prediction overview: https://www.dwd.de/EN/research/weatherforecasting/num_modelling/04_ensemble_methods/ensemble_prediction/ensemble_prediction_node.html
- DWD ICON Database Reference Manual: https://www.dwd.de/DWD/forschung/nwv/fepub/icon_database_main.pdf
- DWD Open Data Server root: https://opendata.dwd.de/weather/nwp/
- DWD ICON-EPS resolution upgrade documentation (November 2022): https://www.dwd.de/DE/fachnutzer/forschung_lehre/numerische_wettervorhersage/nwv_aenderungen/_functions/DownloadBox_modellaenderungen/icon_eps/pdf_2022/pdf_icon_eps_23_11_2022.pdf

### Key references
- Reinert, D., Prill, F., Frank, H., Denhard, M., Baldauf, M., Schraff, C., Gebhardt, C., Marsigli, C., and Zängl, G. *DWD Database Reference for the Global and Regional ICON and ICON-EPS Forecasting System.*
- Zängl, G., Reinert, D., Rípodas, P., and Baldauf, M. (2015). *The ICON (ICOsahedral Non-hydrostatic) modelling framework of DWD and MPI-M: Description of the non-hydrostatic dynamical core.* Quarterly Journal of the Royal Meteorological Society, 141(687), 563–579. https://doi.org/10.1002/qj.2378
