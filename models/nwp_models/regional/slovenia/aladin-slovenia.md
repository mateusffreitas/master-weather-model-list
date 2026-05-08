# ALADIN Slovenia (ALADIN-SI)

## What this model is
ALADIN Slovenia (ALADIN-SI) is a **regional limited-area deterministic numerical weather prediction (NWP) model** operated by the Slovenian Environment Agency (ARSO).

It is part of the **ALADIN limited-area model system** (developed within the ALADIN consortium and the **RC LACE** regional cooperation) and is used for short-range weather forecasting over Slovenia and surrounding regions, with particular relevance to forecasting in complex Alpine and sub-Mediterranean terrain.

---

## Who runs it
- **Organization:** Slovenian Environment Agency (ARSO)
- **Country / region:** Slovenia

---

## What area it covers
- **Coverage:** Slovenia and surrounding Central / South-East European regions
- **Domain details:** ~432 × 432 horizontal grid points at 4.4 km spacing, centered over Slovenia and the eastern Alps / northern Adriatic

---

## Basic details
- **Model type:** Regional deterministic NWP (limited-area)
- **Model system / core:** ALADIN (ALARO-1vB physics package; code cycle cy43t2_bf10 as of 2021)
- **Dynamical formulation:** Hydrostatic (ALADIN spectral limited-area dynamical core)
- **Convection-allowing:** No (4.4 km grid; deep convection is parameterized via the ALARO physics)
- **Horizontal resolution:** ~4.4 km
- **Grid dimensions:** 432 × 432 horizontal grid points
- **Vertical levels:** 87
- **Time step:** 180 s
- **Forecast length:** Up to **72 hours**
- **Update frequency / cycles:** 4× daily — forecasts to +72 h every 6 h
- **Temporal output resolution:** Hourly (selected fields), with standard pressure-level fields archived at sub-daily intervals

---

## Data assimilation
- **Data assimilation:** Yes
- **Method / cadence:**
  - **Atmosphere:** 3-hourly **3D-Var** with a static, downscaled ensemble **B-matrix**
  - **Soil / surface:** **Optimal Interpolation (OI)**
- **Observations assimilated** (mostly via the **OPLACE** RC LACE preprocessing system):
  SYNOP, AMV, HR-AMV, TEMP, AMSU-A & MHS, SEVIRI, IASI, ASCAT, OSCAT, Mode-S MRAR (SI/CZ), MUAC EHS, and ZTD (passive monitoring)

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

---

## Data availability
- **Is the data free?** Yes
- **License:** TBD (ARSO publishes the data publicly but no explicit open-data licence is stated on the download or documentation pages)
- **Is the data downloadable?** Yes
- **Data formats:** GRIB (distributed as compressed ZIP archives)
- **Official download location:**  
  https://meteo.arso.gov.si/uploads/probase/www/model/data/

---

## Notes
- This entry describes the **operational ALADIN-SI regional forecast model** (4.4 km, up to +72 h).
- ARSO additionally runs a number of related but distinct ALADIN-based systems that are **not** the same product as ALADIN-SI and are not described here:
  - **ALADIN for SEE-MHEWS-A** — a non-hydrostatic 2.5 km configuration on 1429 × 1141 points run on ECMWF infrastructure as part of the South-East European Multi-Hazard Early Warning Advisory System.
  - **ALADIN-NWCRUC** — a non-hydrostatic ~1.3 km hourly nowcasting system over the North Adriatic, with hourly 3D-Var + OI assimilation including radar reflectivity (OPERA).
- Technical details above (cycle, grid dimensions, vertical levels, time step, assimilation cadence, observation list) reflect the configuration documented in the **ARSO NWP poster presented at the ALADIN/HIRLAM ASW, April 2021**, and may have evolved since.

---

## Official documentation
- ARSO operational NWP — GRIB output documentation:  
  https://meteo.arso.gov.si/uploads/meteo/help/sl/NumericniRezultatiGRIB.html
- Pristov et al. (2021): *NWP activities at ARSO (Slovenia)* — poster, ALADIN/HIRLAM All-Staff Workshop / ACCORD, April 2021
