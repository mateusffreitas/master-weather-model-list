# AICON-Global

## What this model is
AICON-Global is DWD's first global, machine-learning-based deterministic numerical weather prediction model. It is designed to complement and extend — not replace — the operational physics-based [ICON](./icon-global.md) system.

Unlike ICON, AICON does not integrate explicit equations of motion or physical parameterizations. Instead, it learns the temporal evolution of the atmospheric state directly from DWD's ICON reanalysis (ICON-DREAM), using a GraphCast-like encoder–processor–decoder neural network built on the [Anemoi](https://github.com/ecmwf/anemoi-core) framework. It is the DWD counterpart to ECMWF's [AIFS Single](../eu/aifs-single.md), with which it shares the Anemoi codebase; DWD discontinued its own in-house ML development in June 2024 in favour of the shared Anemoi effort.

AICON-Global was introduced on 3 September 2025 for evaluation, research, and training, and its forecasts were made available as Open Data in early 2026. Forecast skill is still evolving and the model is not part of the operational ICON suite.

---

## Who runs it
- **Organization:** Deutscher Wetterdienst (DWD — German Weather Service)
- **Country / region:** Germany
- **Framework:** Anemoi (collaborative European initiative led by ECMWF; also underpins ECMWF's AIFS and MetNorway's Bris)

---

## What area it covers
- **Coverage:** Global
- **Native grid:** ICON R3B07 triangular icosahedral grid, 2,949,120 cells, ~13 km mesh (same horizontal grid as [ICON Global](./icon-global.md))
- **Additional output:** Regular latitude–longitude grid also provided

---

## Basic details
- **Model type:** Global deterministic NWP (machine-learning / data-driven)
- **Model system / core:** AICON, built on the Anemoi framework (`anemoi-models`); GraphCast-like encoder–processor–decoder with a Graph-Transformer GNN (multi-head attention message passing) constructed directly on ICON's triangular mesh
- **Dynamical formulation:** Not applicable — data-driven; no explicit dynamical core. Learns atmospheric evolution from the ICON-DREAM reanalysis.
- **Convection-allowing:** No (~13 km grid)
- **Horizontal resolution:** ~13 km (native ICON R3B07 icosahedral grid; also output on a regular lat–lon grid)
- **Vertical levels:** 13 reduced ICON model levels (top-down: 49, 57, 64, 70, 75, 79, 86, 91, 96, 101, 108, 112, 119), representing the atmosphere from the surface to about 50 hPa
- **Model top:** ~50 hPa
- **Forecast length:**
  - 180 hours for the 00 and 12 UTC cycles
  - 120 hours for the 06 and 18 UTC cycles
  - 48 hours for the intermediate 03, 09, 15, and 21 UTC cycles (not published on Open Data — see Notes)
- **Update frequency / cycles:** The model is initialized every 3 hours (8 cycles/day) per DWD's introduction notice. The public Open Data server publishes only the four main cycles: 00, 06, 12, 18 UTC.
- **Temporal output resolution:** 3-hourly

---

## Data assimilation and initialization
AICON-Global does **not** perform its own data assimilation. Inference is initialized from a GRIB2 analysis produced by DWD's operational [ICON](./icon-global.md#data-assimilation) system (hybrid LETKF + EnVar), and the model rolls the forecast forward autoregressively in 3-hour steps, producing GRIB2 forecast output.

**Training:** The model was trained on the ICON-DREAM reanalysis (Dual-resolution Reanalysis for Emulators, Applications and Monitoring) at 13 km, over 2010-01-01 to 2023-12-31 at 3-hourly frequency (without rollout). A transfer-learning schedule across mesh resolutions (53 km → 26 km → 13 km) was used to keep training cost-efficient. Training ran on the HoreKa supercomputer (A100-40G) at KIT; inference is containerized for multi-GPU deployment and slots into DWD's existing 24/7 NWP process chain.

---

## What it provides
Deterministic global forecasts, output every 3 hours. Parameters available on the Open Data server:
- **Near-surface:** 2 m temperature (`T_2M`), 2 m relative humidity (`RELHUM_2M`), 10 m wind components (`U_10M`, `V_10M`)
- **Surface:** surface pressure (`PS`), mean sea level pressure (`PMSL`), total precipitation accumulated since forecast start (`TOT_PREC`)
- **Upper air (13 model levels):** temperature (`T`), specific humidity (`QV`), wind components (`U`, `V`), pressure (`P`)

Verification against SYNOP observations shows good near-surface skill in the short range (roughly 0–72 h), with AICON outperforming ICON for near-surface fields at short lead times; ICON remains stronger for surface pressure at longer leads.

---

## Data availability
- **Is the data free?** Yes
- **License:** CC BY 4.0 (DWD Open Data licence; attribution required)
- **Is the data downloadable?** Yes
- **Data formats:** GRIB2
- **Official download location:**
  - https://opendata.dwd.de/weather/nwp/v1/m/aicon/ (parameters under the `/p/` subtree)
- **Retention note:** As with other DWD NWP Open Data, GRIB2 files are retained on the server for a short rolling window only; users needing archives must set up their own retention.
- **Product page / metadata:** https://www.dwd-geoportal.de/products/aicon/

---

## Notes
- **Public data is a subset:** although DWD's 3 September 2025 introduction notice describes initialization every 3 hours (eight cycles), the public Open Data server publishes only the four main cycles (00, 06, 12, 18 UTC). The intermediate 48 h cycles (03/09/15/21 UTC) are not distributed publicly.
- AICON-Global complements but does not replace ICON. As of early 2026 it is provided for evaluation, research, and training, and is technically operational for validation by the DWD forecast centre (reached Q3 2025); it is not part of the operational ICON suite.
- The vertical output is a deliberately reduced set of 13 ICON model levels (versus 120 for operational ICON). Note this uses ICON's native Smooth-Level Vertical (SLEVE) terrain-following levels, unlike GraphCast-family models that output on pressure levels.
- The parameter set has expanded slightly since the 3 September 2025 introduction notice: mean sea level pressure (`PMSL`) appears in the Open Data tree in addition to the originally listed surface pressure.
- **Energy footprint:** A single AICON inference run consumes ~0.13 kWh, versus ~60.24 kWh for a deterministic 180 h ICON global run — roughly a 460× reduction at inference time (DWD figures, German electricity mix 2023/2024).
- **Roadmap:** A limited-area extension is planned — AICON-LAM (~6.5 km EU) targeted for NWP operation in Q2 2026, and AICON-LAM (~2 km DE) in Q1 2027. DWD's LAM approach merges global and regional reanalysis input datasets rather than using the stretched-grid approach of Nipen et al. (2024).
- **AI approach:** Standalone data-driven forecast model (initialized from a physics analysis, rolled forward by a neural network). Add to [`AI_MODELS.md`](../../../../AI_MODELS.md) under the standalone-deterministic group, alongside AIFS Single and AIGFS.

---

## Relationship to other models
- **[ICON Global](./icon-global.md)** (DWD, operational) — the physics-based global model AICON complements. AICON is trained on ICON's reanalysis (ICON-DREAM), uses ICON's R3B07 grid and ICON analyses for initialization, and the publicly distributed cycles (00, 06, 12, 18 UTC) match ICON Global's four main cycles. The existing ICON Global entry already notes AICON as an experimental sibling.
- **[AIFS Single](../eu/aifs-single.md)** (ECMWF, operational) — architectural and framework peer. Both are built on the Anemoi encoder–processor–decoder stack; AICON applies it on ICON's icosahedral mesh and ICON-level vertical structure rather than ECMWF's lat–lon / pressure-level setup.
- **Bris** (MetNorway) — another Anemoi-based model (extends AIFS); part of the same collaborative framework.

---

## Official documentation
- DWD AICON research page: https://www.dwd.de/DWD/forschung/nwv/aicon.html
- DWD-Geoportal AICON product page: https://www.dwd-geoportal.de/products/aicon/
- DWD Open Data root: https://opendata.dwd.de/weather/nwp/
- Operationelles NWV-System Änderungsmitteilung — Introduction of AICON-Global (3 September 2025)

### Key references
- Prill, F., Jacob, M., & DWD AICON Team (2025). *AICON – Introducing ML-based weather forecasting at DWD.* ECMWF Workshop on HPC in Meteorology, September 2025.
- Lang, S., et al. (2024). *AIFS — ECMWF's data-driven forecasting system.* arXiv. https://doi.org/10.48550/arXiv.2406.01465
- Valmassoi, A., et al. (2024). *ICON-DREAM: A new dual resolution reanalysis from DWD.* 6th WCRP International Conference on Reanalysis.
