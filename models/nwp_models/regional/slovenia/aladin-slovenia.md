# ALADIN Slovenia (ALADIN-SI)

## What this model is
ALADIN Slovenia (ALADIN-SI) is a **regional limited-area deterministic numerical weather prediction (NWP) model** operated by the Slovenian Environment Agency (ARSO).

It is part of the **ALADIN limited-area model system** (developed within the ALADIN consortium and the **RC LACE** regional cooperation, now under the **ACCORD** umbrella) and is used for short-range weather forecasting over Slovenia and surrounding regions, with particular relevance to forecasting in complex Alpine and sub-Mediterranean terrain. It also serves as the meteorological driver for ARSO's downstream **air-quality (CAM-X)** and **ocean (NEMO)** modeling systems.

---

## Who runs it
- **Organization:** Slovenian Environment Agency (ARSO)
- **Country / region:** Slovenia

---

## What area it covers
- **Coverage:** Slovenia and surrounding Central / South-East European regions
- **Domain details:** 432 × 432 horizontal grid points at 4.4 km spacing, centered over Slovenia and the eastern Alps / northern Adriatic

---

## Basic details
- **Model type:** Regional deterministic NWP (limited-area)
- **Model system / core:** ALADIN (ALARO-1vB physics package; code cycle cy43t2_bf10)
- **Dynamical formulation:** Hydrostatic (ALADIN spectral limited-area dynamical core)
- **Convection-allowing:** No (4.4 km grid; deep convection is parameterized via the ALARO physics)
- **Horizontal resolution:** ~4.4 km
- **Grid dimensions:** 432 × 432 horizontal grid points
- **Vertical levels:** 87
- **Time step:** 180 s
- **Forecast length:** Up to **72 hours**
- **Update frequency / cycles:** 4× daily
- **Cut-off time:** ~110 min after nominal (operational long cut-off)
- **Temporal output resolution:** Hourly (selected fields), with standard pressure-level fields archived at sub-daily intervals

---

## Data assimilation
- **Data assimilation:** Yes
- **Method / cadence:**
  - **Atmosphere:** 3-hourly **3D-Var** with a static, downscaled ensemble **B-matrix**
  - **Soil / surface:** **Optimal Interpolation (OI)**
- **Observations assimilated** (mostly via the **OPLACE** RC LACE preprocessing system):
  - **Surface:** SYNOP, OPLACE AWS (T2m, RH2m, U10m, P)
  - **Radiosondes:** OPLACE obsoul (T, U, q)
  - **Aircraft:** AMDAR / ACARS, Mode-S EHS / MRAR (T, U)
  - **Atmospheric motion vectors:** EUMETSAT, EUMETSAT HR (U)
  - **Radiances:** MSG SEVIRI, NOAA-19 (AMSU-A, MHS), Metop-A/B/C (AMSU-A, MHS, IASI)
  - **Scatterometer:** ASCAT (U10m over sea)
  - **GNSS:** EGVAP ZTD (passive monitoring only)
- **Note:** OPERA radar reflectivities are used in the ARSO 1.3 km nowcasting system (NWCRUC), **not** in ALADIN-SI itself.

---

## Initial and boundary conditions
- **Initial conditions:** From the ALADIN-SI 3D-Var / OI analysis (assimilation cycle every 3 h)
- **Lateral boundary conditions:** **ECMWF IFS** (with a 6 h lag); coupling at 1 h frequency in the assimilation cycle and 3 h frequency in the forecast
- **Initialization:** Space-consistent LBC at initial time; no separate initialization step

---

## What it provides
Deterministic forecasts of:
- precipitation (3-hourly accumulated rain + snow)
- total cloud cover
- surface and mean sea-level pressure
- near-surface temperature, humidity, and wind (2 m / 10 m)
- temperature, humidity, and wind at standard pressure levels  
  (925 hPa, 850 hPa, 700 hPa, 500 hPa)

Operationally also serves as the meteorological driver for ARSO's downstream **CAM-X** (air quality) and **NEMO** (ocean) modeling systems.

---

## Data availability
- **Is the data free?** Yes
- **License:** Open with mandatory attribution. ARSO publishes information from meteo.arso.gov.si as freely reusable on the condition that the source is cited as "Vir: Agencija Republike Slovenije za okolje" or "Vir: ARSO" (in English: "Source: Slovenian Environment Agency" or "Source: ARSO"). Mandatory attribution is stipulated by **Article 14 of the Slovenian Act on the State Meteorological, Hydrological, Oceanographic and Seismological Service** (Official Gazette of the Republic of Slovenia, No. 60/17). No specific named open-data licence (e.g., CC BY) is asserted; reuse terms are governed by the ARSO site policy and Slovenian law.
- **Is the data downloadable?** Yes
- **Data formats:** GRIB (distributed as compressed ZIP archives)
- **Official download location:**  
  https://meteo.arso.gov.si/uploads/probase/www/model/data/

---

## Notes
- This entry describes the **operational ALADIN-SI regional forecast model** (4.4 km, up to +72 h). The configuration described above is documented unchanged across ARSO operational status posters from September 2021 through September 2024; details after that date may have evolved.
- ARSO additionally runs a number of related but distinct ALADIN-based systems that are **not** the same product as ALADIN-SI and are not described here:
  - **ALARO-RUC (NWCRUC)** — a non-hydrostatic 1.3 km hourly nowcasting system over the North Adriatic (cy43t2_bf10, ALARO-1vB, 589 × 589 points, 87 vertical levels, 60 s time step). It has hourly 3D-Var + OI assimilation including OPERA radar reflectivity (ingested via the NIMBUS production lines as of 2024). The system was pre-operational from July 2021 and is operational as of ARSO's September 2023 EWGLAM poster, running 24 forecast cycles per day to +36 h. Cutoff times are 70 min after nominal time for assimilation and 35 min for production; output is provided every 5 min.
  - **ALADIN for SEE-MHEWS-A (SEEMHEWS)** — a non-hydrostatic 2.5 km configuration on 1429 × 1141 points, 87 vertical levels, run on ECMWF Atos infrastructure as part of the South-East European Multi-Hazard Early Warning Advisory System. Uses the same model version and assimilation setup as operational ALADIN-SI; runs twice daily to +72 h with a ~10 h cut-off.
- ARSO also contributes to the EU **Destination Earth (DE_330)** initiative — providing IFS-driven LBCs over a large European domain (DEOL) and observation monitoring with 30 min and 12 h cut-offs to support local DestinE digital twins. This is a separate activity from ALADIN-SI.
- **Near-term DA development** documented in the 2024 RC LACE DA status report includes migration from cy43t2 to **cy48t3 (export)** with 3D-Var implemented via **OOPS**, cooperation with GeoSphere Austria on the **C-LAEF1k** convection-permitting ensemble (EnVar testing), and cooperation with DHMZ on **all-sky IR radiances** (IASI, IRS).

---

## Official documentation
- ARSO operational NWP — GRIB output documentation:  
  https://meteo.arso.gov.si/uploads/meteo/help/sl/NumericniRezultatiGRIB.html
- ARSO website — Conditions of re-use:  
  https://meteo.arso.gov.si/met/sl/about/
- Pristov et al. (2021): *NWP activities at ARSO (Slovenia)* — poster, ALADIN/HIRLAM All-Staff Workshop / ACCORD, April 2021
- ARSO NWP team (2021): *NWP activities at ARSO (Slovenia)* — poster, EWGLAM, September 2021
- Strajnar et al. (2022): *NWP activities at ARSO (Slovenia)* — poster, 2nd ACCORD All-Staff Workshop, March 2022
- Pristov et al. (2022): *NWP activities at ARSO (Slovenia)* — poster, EWGLAM, September 2022
- Cedilnik et al. (2023): *ACCORD activities at ARSO (Slovenia)* — poster, 3rd ACCORD All-Staff Workshop, March 2023
- Cedilnik et al. (2023): *NWP activities at ARSO (Slovenia)* — poster, EWGLAM, September 2023
- Strajnar (2024): *Data assimilation status and activities in Slovenia – 2024* — RC LACE
