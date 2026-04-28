# ICON-D2-RUC-EPS (ICON-D2 Rapid Update Cycle Ensemble)

## What this model is
ICON-D2-RUC-EPS is DWD's hourly-updating, convection-permitting regional ensemble prediction system for Germany and surrounding central European countries. It is the ensemble counterpart of the deterministic [ICON-D2-RUC](../../../nwp_models/regional/germany/icon-d2-ruc.md), sharing the same 2.2 km grid and short forecast range while explicitly representing forecast uncertainty across multiple ensemble members.

The system is purpose-built for probabilistic short-range forecasting of severe convective weather — thunderstorm probabilities, hail probabilities, flash-flood-inducing precipitation, and similar high-impact, fast-evolving phenomena where forecast uncertainty matters as much as the deterministic best guess. With hourly initialisation and dense radar data assimilation, ICON-D2-RUC-EPS sits at the boundary between nowcasting and short-range NWP, where probabilistic guidance is operationally most valuable.

ICON-D2-RUC-EPS is the operational descendant of work carried out within DWD's SINFONY (Seamless INtegrated FOrecastiNg sYstem) project, which targeted seamless ensemble prediction across the nowcasting-to-NWP transition. Earlier versions of the system appear in DWD literature under the name "SINFONY-RUC-EPS."

---

## Who runs it
- **Organization:** Deutscher Wetterdienst (DWD — German Weather Service)
- **Country:** Germany

---

## What area it covers
- **Coverage:** Germany, Benelux (Belgium, the Netherlands, Luxembourg), Switzerland, Austria, and parts of neighbouring countries (same domain as ICON-D2-RUC and ICON-D2)

---

## Basic details
- **Model type:** Regional convection-permitting ensemble prediction system, rapid update cycle
- **Underlying core:** ICON (Icosahedral Nonhydrostatic) limited-area configuration
- **Horizontal resolution:** 2.2 km (same as ICON-D2-RUC)
- **Forecast length:** Up to +14 hours (matching the deterministic ICON-D2-RUC)
- **Update frequency:** Hourly (24× daily)
- **Cloud microphysics:** Two-moment bulk scheme (Seifert and Beheng), inherited from the deterministic ICON-D2-RUC
- **Operational lineage:** Developed within the SINFONY project as SINFONY-RUC-EPS; deployed alongside ICON-D2-RUC's operational implementation in mid-2024

---

## How the ensemble is generated
ICON-D2-RUC-EPS member generation follows the broader ICON ensemble approach used in [ICON-D2-EPS](./icon-d2-eps.md), adapted to the rapid-update cycle:

- **Initial state perturbations:** From the hourly KENDA-LETKF ensemble data assimilation cycle
- **Lateral boundary conditions:** Derived from forecasts of larger-domain ensemble systems (typically ICON-EU-EPS members, with potential diversification across global ensembles)
- **Soil moisture:** Stochastically perturbed at initialisation
- **Model physics:** Randomized physics parameter perturbations across members

Because the system is convection-allowing at 2.2 km and runs hourly, error growth at small spatial and temporal scales is rapid. The hourly cadence partially compensates by forcing the ensemble back to observed reality each hour, while still preserving the spread needed for probabilistic guidance.

The SINFONY-RUC-EPS development line uses extensive remote-sensing data assimilation including 3D radar volume scans of radial winds and reflectivity, cell objects, Meteosat VIS channels, and lightning observations.

---

## What it provides
- Full set of surface and upper-air fields at 2.2 km resolution across all ensemble members
- Convection-allowing probabilistic products: thunderstorm probability, heavy precipitation probability, hail probability, wind gust exceedance probability
- Simulated radar reflectivity volume scans output at 5-minute intervals during forecast runs (a SINFONY-specific capability oriented toward radar-network comparisons)
- Lightning Potential Index (LPI) and related convective-weather parameters
- Cell-object-based ensembles supporting DWD's "warn-on-objects" warning paradigm for convective events
- Combined nowcasting and NWP ensemble products targeting hydrological warnings (precipitation and reflectivity ensembles)

---

## Data availability
- **Is the data free?** Yes
- **Is the data downloadable?** Yes
- **Data format:** GRIB2
- **Primary access:** DWD Open Data Server
  - ICON-D2-RUC-EPS: https://opendata.dwd.de/weather/nwp/v1/m/icon-d2-ruc-eps/
- **Distribution path note:** ICON-D2-RUC-EPS is distributed under `/weather/nwp/v1/m/` rather than alongside the main ICON-D2-EPS directory at `/weather/nwp/icon-d2-eps/`. The `v1/m/` path appears to be a separate distribution tier introduced in 2025 and also hosts ICON-D2-RUC and the ICON-ART family.
- **Licence:** DWD Open Data licence (CC-BY 4.0-compatible; attribution required)
- **Retention note:** DWD retains GRIB2 files on the open data server for a short period only (typically 24 hours).

---

## Notes
- ICON-D2-RUC-EPS is the ensemble counterpart of the deterministic [ICON-D2-RUC](../../../nwp_models/regional/germany/icon-d2-ruc.md) — see that entry for the shared model core, microphysics choices, and operational context.
- The system is operationally distinct from the standard [ICON-D2-EPS](./icon-d2-eps.md), which uses a 3-hourly cycle with 48-hour forecast length. ICON-D2-RUC-EPS extends DWD's convection-permitting ensemble guidance into the 0–14 hour rapid-update range. The two ensembles run in parallel and serve complementary forecast windows.
- At 2.2 km with hourly updates and dense radar DA, ICON-D2-RUC-EPS occupies an unusual operational niche internationally. Comparable peers include experimental rapid-update ensemble configurations at MeteoSwiss and ECCC; few national weather services currently operate a true convection-permitting rapid-update ensemble in production.
- The system's outputs feed into DWD's seamless nowcasting-NWP blending products and are particularly oriented toward severe convective warnings, where the combination of probabilistic guidance and rapid update cycling is operationally most valuable.

---

## Official documentation
- DWD ICON-D2 model description: https://www.dwd.de/EN/research/weatherforecasting/num_modelling/01_num_weather_prediction_modells/icon_d2/icon_d2_node.html
- DWD ensemble prediction overview: https://www.dwd.de/EN/research/weatherforecasting/num_modelling/04_ensemble_methods/ensemble_prediction/ensemble_prediction_node.html
- DWD ICON Database Reference: https://www.dwd.de/DWD/forschung/nwv/fepub/icon_database_main.pdf
- DWD Open Data Server v1/m subtree: https://opendata.dwd.de/weather/nwp/v1/m/
- SINFONY project context: documented in DWD SINFONY publications and EMS conference abstracts
