# ALADIN Slovakia (ALADIN/SHMÚ)

## What this model is
ALADIN/SHMÚ is the **regional limited-area deterministic numerical weather prediction (NWP) system** operated by the Slovak Hydrometeorological Institute.

It is based on the **ALADIN** modeling system, in the **ALARO Canonical Model Configuration**, and forms the primary source of short-range weather forecasts at SHMÚ. Development is carried out within the **ACCORD** consortium and the **RC LACE** regional cooperation.

---

## Who runs it
- **Organization:** Slovak Hydrometeorological Institute (SHMÚ)
- **Country / region:** Slovakia

---

## What area it covers
- **Coverage:** Slovakia and a large part of Central Europe
- **Domain details:** 625 × 576 grid points at 4.5 km spacing, centred on Slovakia and covering most of Central Europe and the surrounding Mediterranean / Baltic margins

---

## Basic details
- **Model type:** Regional deterministic NWP (limited-area)
- **Model system / core:** ALADIN (ALARO Canonical Model Configuration)
- **Dynamical formulation:** Hydrostatic (ALADIN spectral limited-area dynamical core)
- **Convection-allowing:** No (4.5 km grid; deep convection is parameterized by the ALARO physics)
- **Code version:** CY46T1_bf07 (export, with bug-fixes)
- **Physics package:** ALARO-1vB
- **Horizontal resolution:** 4.5 km
- **Grid dimensions:** 625 × 576 horizontal grid points
- **Vertical levels:** 63 (an L63 → L87 upgrade is in development; see *Notes*)
- **Time step:** 180 s
- **Forecast length:** Up to **72 hours** (for the publicly distributed product, all four daily cycles)
- **Update frequency:** 4× daily (00, 06, 12, 18 UTC)
- **Temporal output resolution:** 1 hour
- **Output availability:** Approximately 3–4 hours after the cycle reference time

---

## Data assimilation
- **Data assimilation:** Yes
- **Upper-air analysis:** Spectral blending with digital filter initialization (**DFI**) combined with **3D-Var**
- **Surface analysis:** **CANARI** (optimal interpolation)
- **SST treatment:** Relaxation to SST from the LBC0 file (replacing the previous *blendsur* approach, following the configuration used at CHMI)
- **Initialization:** None applied to the analysis itself (DFI is used inside the blending step)

---

## Initial and boundary conditions
- **Initial conditions:** From the ALADIN/SHMÚ blending + 3D-Var / CANARI analysis
- **Lateral boundary conditions:** **ARPEGE** (Météo-France global model), with both long and short cut-off LBCs available; coupling frequency 3 hours

---

## What it provides
Deterministic forecasts of standard meteorological variables on the model grid, including:
- temperature, humidity, and wind (3D atmosphere; surface and selected vertical levels)
- surface and mean sea-level pressure
- hourly precipitation
- cloud cover and radiation variables
- derived parameters used for warnings, hydrology, and aviation

Outputs are used operationally for **weather forecasting, warnings, aviation, hydrology, and emergency management** in Slovakia.

---

## Data availability
- **Is the data free?** Yes (publicly accessible at the SHMÚ open-data portal)
- **License:** TBD (no explicit open-data licence is stated on the SHMÚ open-data directory or the SHMÚ ALADIN documentation page; the SHMÚ website footer asserts general copyright over website content, but the open-data portal itself does not publish specific licence terms)
- **Is the data downloadable?** Yes
- **Data formats:** GRIB2
- **Official download location:**  
  https://opendata.shmu.sk/meteorology/weather/nwp/aladin/sk/4.5km/  
  (one file per forecast hour; each file contains all distributed meteorological elements)

---

## Notes
- The current operational configuration runs on the SHMÚ **NEC HPC** system (240 nodes total; Intel Xeon Gold Scalable Processors, Cascade Lake, Omni-Path, Linux), with ALADIN/SHMÚ using 40 nodes per cycle.
- An upgrade from **L63 to L87** vertical levels is in progress at SHMÚ, motivated in part by improved suitability for GNSS ZTD assimilation; impact has been assessed as approximately neutral as of mid-2024 and the upgrade had not yet been declared operational.
- Other ALADIN-based systems are also run at or by SHMÚ but are **not the same product** as the ALADIN/SHMÚ deterministic forecast described here, and are not openly downloadable in the same way:
  - **A-LAEF** — the ACCORD Limited Area Ensemble Forecasting system (ALARO-1vB, multi-physics + surface SPPT, 4.8 km, L60), run operationally at ECMWF under SHMÚ supervision and shared as the common RC LACE ensemble.
  - **ALA2e** — an experimental non-hydrostatic 2 km configuration on CY48T3, with ALARO-1vB physics, L87 and a 90 s time step, downscaled from A-LAEF control.
  - **RUC1 / ALA1** — a 1 km test-mode rapid-update-cycle configuration with hourly cycling, L87, 30 s time step, and CANARI + 3D-Var.
- Future development plans documented at the September 2024 RC LACE DA working days include SEVIRI radiance assimilation, GNSS (ZTD/STD) assimilation, and continued upgrades to the RUC1 system.

---

## Recent version history

### 2024 — Cycle upgrade and SST handling change
ALADIN/SHMÚ (and the experimental RUC1 system) were upgraded to **CY46T1_bf07** (export branch with bug-fixes). The treatment of sea-surface temperature was changed from *blendsur* to **relaxation to SST from LBC0**, following the approach used at CHMI. No principal changes were made to the upper-air 3D-Var + blending observation set or algorithmics in this cycle.

### 2017 — 4.5 km / L63 ALARO-1vB became operational
The current 4.5 km, L63 configuration with **ALARO-1vB** physics replaced the previous 9 km / L37 system, with substantially improved performance in high-impact weather situations. This is the configuration described in Derková et al. (2017).

---

## Official documentation
- SHMÚ — *Model ALADIN — popis* (model description page):  
  https://www.shmu.sk/sk/?page=1016
- Derková, M. et al. (2017): *Recent improvements in the ALADIN/SHMÚ operational system*, Meteorologický časopis, 20, 45–52
- Derková, M., Imrišek, M., Neštiak, M., Simon, A. (2024): *Data assimilation activities @ SHMU*, RC LACE DA Working Days online, 5–6 September 2024
