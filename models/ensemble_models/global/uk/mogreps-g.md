# MOGREPS-G (Met Office Global and Regional Ensemble Prediction System – Global)

## What this model is
MOGREPS-G is the global ensemble numerical weather prediction system operated by the UK Met Office.

It is the global ensemble counterpart to the deterministic [UKMO Global](../../../nwp_models/global/uk/ukmo-global.md), built on the same Unified Model code base in its Global Coupled 5 (GC5) science configuration. MOGREPS-G provides probabilistic medium-range forecasts to 10 days, supplies lateral boundary conditions for the regional [MOGREPS-UK](../../regional/uk/mogreps-uk.md) ensemble, and contributes flow-dependent background-error covariances back to UKMO Global's hybrid 4D-Var data assimilation system.

The ensemble distribution is structurally distinctive: 17 perturbed members + 1 control are produced **per cycle** for forecast distribution, but the underlying data assimilation runs an **ensemble of 44 perturbed members** under a hybrid four-dimensional ensemble-variational (En-4DEnVar) scheme — the first operational atmospheric ensemble in the world to apply hybrid 4DEnVar to each of its analysis members (since December 2019).

The current operational version is part of **Operational Suite 47 (OS47)**, implemented on 21 January 2026 — the Met Office's first major science upgrade in over three years and the first run on its new Microsoft Azure-based supercomputer. OS47 extended MOGREPS-G's forecast range from 7d 6h to 10 days.

---

## Who runs it
- **Organization:** Met Office
- **Country / region:** United Kingdom

---

## What area it covers
- **Coverage:** Global

---

## Basic details
- **Model type:** Global ensemble NWP
- **Model system / core:** Unified Model (UM), in its Global Coupled 5 (GC5) science configuration as of OS47
- **Dynamical formulation:** Non-hydrostatic, fully compressible deep-atmosphere equations on a regular latitude–longitude grid (ENDGame dynamical core); semi-Lagrangian, semi-implicit time integration
- **Convection-allowing:** No (deep convection is parameterized at ~20 km resolution)
- **Ensemble size (forecast distribution):** 18 per cycle (1 control + 17 perturbed). Two adjacent cycles can be combined to form a 36-member time-lagged ensemble for downstream products.
- **Underlying DA ensemble:** 44 perturbed members (En-4DEnVar)
- **Native horizontal grid:** N640 (1280 × 960 grid points; ~0.28° E–W × ~0.19° N–S; ~20 km in mid-latitudes)
- **Vertical levels:** 70
- **Forecast length:** 246 hours (10 days) for all 4 cycles, since OS47 (21 January 2026); previously 174 hours (7d 6h) under OS46
- **Update frequency:** 4× daily (00, 06, 12, 18 UTC)
- **Temporal output resolution (Open Data):**
  - Hourly to +54 h, then 3-hourly to +198 h or +246 h (parameter-dependent)
  - Some parameters are available hourly to +132 h before transitioning to 3-hourly
- **Data latency:** Approximately 10–11 hours after model run time

---

## Initial conditions
MOGREPS-G is initialized from a **hybrid four-dimensional ensemble-variational (En-4DEnVar)** ensemble data assimilation system. Since December 2019, this system applies En-4DEnVar to each of **44 perturbed analysis members**, making MOGREPS-G the first operational atmospheric ensemble in the world to do so.

The MOGREPS-G ensemble analysis serves a dual purpose:
- It produces the perturbed initial conditions for the MOGREPS-G ensemble forecast itself
- It provides **flow-dependent background-error covariances** for the deterministic [UKMO Global](../../../nwp_models/global/uk/ukmo-global.md) hybrid 4D-Var data assimilation system

A subsequent OS46-era upgrade (December 2020) added **shifting and lagging** to the ensemble use in deterministic DA — including ensemble members from the previous cycle and from adjacent forecast lead times — to augment the effective ensemble size without running additional forecasts. The deterministic system was further weighted toward ensemble covariances in May 2022.

The 44-member DA ensemble size is specifically chosen for analysis quality. The reduction to 17 + 1 perturbed members for forecast distribution reflects computational and bandwidth tradeoffs; the full 44 are used in the analysis cycle but only a subset are propagated forward.

Initial perturbations were generated using the **Ensemble Transform Kalman Filter (ETKF)** from MOGREPS-G's first operational implementation in September 2008 until December 2019, when it was replaced by the En-4DEnVar approach.

---

## Perturbations and design
- **Initial condition perturbations:** En-4DEnVar ensemble analyses (see above)
- **Model uncertainty representation:** Two complementary stochastic schemes:
  - **Random Parameters (RP):** Selected uncertain physics parameters (e.g., entrainment rate, fallspeed coefficients, gravity-wave drag coefficients) vary stochastically during the forecast to sample model uncertainty in physical parameterizations
  - **Stochastic Kinetic Energy Backscatter (SKEB):** Injects wind increments proportional to the square root of diagnosed kinetic energy dissipation from semi-Lagrangian advection and other unresolved sources
- **Time-lagging:** Adjacent cycles can be combined to form 36-member time-lagged ensembles for downstream applications, exploiting the 6-hour cycle frequency to expand effective ensemble size at the cost of staggered initialization times.

---

## What it provides
Probabilistic global forecasts including:
- Individual ensemble member forecasts (17 perturbed + 1 control)
- Ensemble mean and ensemble spread
- Surface and upper-air fields: temperature, wind components, geopotential height, specific humidity, mean sea level pressure
- Total precipitation and precipitation type
- Cloud cover, ceilings, visibility, and wind gusts
- Boundary-layer and aviation-relevant diagnostics
- Lateral boundary conditions for the [MOGREPS-UK](../../regional/uk/mogreps-uk.md) regional ensemble
- Flow-dependent background-error covariances for the deterministic [UKMO Global](../../../nwp_models/global/uk/ukmo-global.md) data assimilation

---

## Relationship to other models
- **[UKMO Global](../../../nwp_models/global/uk/ukmo-global.md):** Deterministic counterpart, sharing the Unified Model codebase and GC5 science configuration. The two systems are operationally interdependent — MOGREPS-G provides ensemble covariances to UKMO Global's hybrid 4D-Var, and both share the OS47 upgrade cycle.
- **[MOGREPS-UK](../../regional/uk/mogreps-uk.md):** Regional convection-permitting ensemble counterpart for the UK; receives lateral boundary conditions from MOGREPS-G.

The Unified Model is also operated by partner agencies for ensemble forecasting: the Australian Bureau of Meteorology runs ACCESS-GE (33 km global ensemble), and the Korea Meteorological Administration runs its own Unified Model ensemble.

---

## Data availability
- **Is the data free?** Yes
- **License:** Creative Commons Attribution-ShareAlike 4.0 (CC BY-SA 4.0); attribution and ShareAlike required (note: this differs from ECMWF's CC-BY-4.0 by adding the ShareAlike obligation)
- **Resolution:** ~20 km native (no downsampling)
- **Format:** NetCDF (CF-compliant)
- **Retention:** **30-day rolling archive** — a curated subset of the operational ensemble parameters
- **Caveat:** The dataset is offered on a free, **unsupported**, non-operational basis. The Met Office does not recommend it for critical business purposes.
- **Official location:**
  - https://registry.opendata.aws/met-office-global-ensemble/
- **Data catalog change (late January 2026):** From the OS47 upgrade onward, the dataset includes additional parameters, extended forecast range to 246 hours, vertical level additions, precision changes, and a parameter rename (`height_asl_on_pressure_levels` → `geopotential_height_on_pressure_levels`).

The publicly distributed subset is significantly smaller than the full operational ensemble. The Met Office distributes the full operational MOGREPS-G output through its commercial Weather DataHub and direct customer feeds, not through the AWS Open Data programme.

---

## Notes
- The forecast-distribution ensemble (17 perturbed + 1 control = 18 per cycle) is materially smaller than the 51-member ECMWF ENS or 31-member NOAA GEFS. The Met Office partly compensates through time-lagging (combining adjacent cycles to form 36-member ensembles) and through the much larger 44-member DA ensemble that informs both MOGREPS-G initial conditions and UKMO Global background errors.
- MOGREPS-G is run **coupled to a 1/4° interactive ocean model** (since OS45 / 5 May 2022), with hourly atmosphere–ocean exchange. The atmospheric component is the focus of this entry; consult Met Office documentation for ocean coupling details.
- Output should be interpreted probabilistically rather than as a deterministic forecast. The system's value is in calibrated probabilities and ensemble spread, not in any single-member view.
- Like UKMO Global, MOGREPS-G runs on the Met Office's new Microsoft Azure-based supercomputer, which became operational in 2024 and on which OS47 was the first major modelling suite to run.

---

## Recent version history

### OS47 / PS47 / GC5 — operational 21 January 2026 (current)
First major science upgrade in over three years, deployed on the Met Office's new Microsoft Azure supercomputer. Highlights for MOGREPS-G:
- **Forecast range extended from 174 hours (7d 6h) to 246 hours (10 days)**, enabling earlier alerts for high-impact weather
- New **GC5** science configuration (replacing GC4 which had been operational since May 2022)
- Improved tropical cyclone structure and intensity guidance carried over from the parent UKMO Global upgrade
- New stochastic perturbations consistent with the Regional Atmosphere and Land 3 (RAL3) configuration adopted for the UK ensemble
- Open Data product changes: additional parameters, vertical levels, and timesteps; precision changes; `height_asl_on_pressure_levels` renamed to `geopotential_height_on_pressure_levels`
- TIGGE data volumes increased from 3.2 GB per member to 4.4 GB per member to accommodate the longer forecast range

### OS45 / GC4 — operational 5 May 2022
Major upgrade introducing **two-way atmosphere–ocean coupling** to MOGREPS-G at 20 km / N640L70 resolution, with the Global Atmosphere–Land Model fully coupled to a 1/4° interactive ocean model. Both atmospheric and ocean models exchange information hourly. GC4 science configuration adopted.

### December 2019 — En-4DEnVar implementation
MOGREPS-G transitioned from the **Ensemble Transform Kalman Filter (ETKF)** to **hybrid four-dimensional ensemble-variational (En-4DEnVar)** data assimilation. This made MOGREPS-G the **first operational atmospheric ensemble in the world** to apply hybrid 4DEnVar to each of its perturbed analysis members.

### November 2016 (PS38) — resolution upgrade
Horizontal resolution increased from 33 km (33 km) to 20 km (N640L70, 1280 × 960 lat-lon grid). Members per cycle increased from 12 to 18 (later refined to 17 + 1 control). The two-cycle time-lagged ensemble of 36 members was formalized.

### Earlier history
MOGREPS was the Met Office's primary short-range ensemble system from its operational launch in September 2008. The original system was 24-member at 24 km resolution, focused on the North Atlantic and European domain, and used initial perturbations from an ETKF scheme (Bowler et al. 2007a).

---

## Official documentation
- Met Office MOGREPS overview: https://www.metoffice.gov.uk/research/weather/ensemble-forecasting/mogreps
- Met Office numerical weather prediction overview: https://www.metoffice.gov.uk/research/approach/modelling-systems/unified-model/weather-forecasting
- Parallel Suite 47 (PS47) overview: https://www.metoffice.gov.uk/services/data/parallel-suite-47-ps47-overview
- Met Office Weather DataHub upcoming changes: https://datahub.metoffice.gov.uk/support/changes-and-updates
- AWS Open Data Registry: https://registry.opendata.aws/met-office-global-ensemble/
- TIGGE MOGREPS-G model description: https://confluence.ecmwf.int/display/TIGGE/Met+Office

### Key references
- Inverarity, G. W., et al. (2023). *Met Office MOGREPS-G initialisation using an ensemble of hybrid four-dimensional ensemble variational (En-4DEnVar) data assimilations.* Quarterly Journal of the Royal Meteorological Society, 149, 1138–1163. https://doi.org/10.1002/qj.4431
- Tennant, W. J., G. J. Shutts, A. Arribas, and S. A. Thompson (2011). *Using a stochastic kinetic energy backscatter scheme to improve MOGREPS probabilistic forecast skill.* Mon. Wea. Rev., 139, 1190–1206. https://doi.org/10.1175/2010MWR3430.1
- Bowler, N. E., A. Arribas, K. R. Mylne, K. B. Robertson, and S. E. Beare (2008). *The MOGREPS short-range ensemble prediction system.* Quart. J. Roy. Meteor. Soc., 134, 703–722.
