# CHIMERE Hungary

## What this model is
CHIMERE Hungary is the operational **air quality / atmospheric composition forecast** run by HungaroMet (Hungarian Meteorological Service) using the CHIMERE Eulerian offline chemistry-transport model (CTM). It produces gridded hourly forecasts of six regulated pollutants — CO, NO₂, O₃, SO₂, PM10, and PM2.5 — over the Carpathian Basin and three high-resolution city domains (Budapest, Miskolc, Pécs).

The forecasts are driven by HungaroMet's [AROME](../../../nwp_models/regional/hungary/arome-hungary.md) meteorology and EMEP anthropogenic emissions, and are intended for public-health air quality forecasting and air quality assessment.

---

## Who runs it
- **Organization:** HungaroMet (Hungarian Meteorological Service, formerly OMSZ — Országos Meteorológiai Szolgálat)
- **Country / region:** Hungary

---

## What area it covers
- **Coverage:** Hungary and the wider Carpathian Basin, plus three nested high-resolution city windows
- **Domain details (four published domains):**

  | Domain | Code | Bounds | Resolution |
  |---|---|---|---|
  | Carpathian Basin | HUN | 45°N–50°N, 14°E–25°E | 0.1° × 0.1° (≈ 8–11 km) |
  | Budapest | BUD | 47.30°N–47.66°N, 18.85°E–19.37°E | 0.02° × 0.015° (≈ 1.5–2 km) |
  | Miskolc | MIS | 48.02°N–48.185°N, 20.51°E–20.89°E | 0.02° × 0.015° (≈ 1.5–2 km) |
  | Pécs | PEC | 46.01°N–46.19°N, 18.11°E–18.39°E | 0.02° × 0.015° (≈ 1.5–2 km) |
- **Projection:** Regular latitude–longitude (latlon)

---

## Basic details
- **Model type:** Air quality / atmospheric composition (regional offline chemistry-transport model)
- **Model system / core:** CHIMERE (multi-scale Eulerian offline CTM, developed by LMD/IPSL and partners)
- **Model version:** CHIMERE-2017
- **Horizontal resolution:** 0.1° × 0.1° (HUN); 0.02° × 0.015° (BUD, MIS, PEC)
- **Vertical levels:** TBD (not stated in the HungaroMet dataset description)
- **Model top:** TBD
- **Forecast length:** 48 hours (0–48 h)
- **Update frequency / cycles:** 1× daily (00 UTC), all domains
- **Temporal output resolution:** 1 hour

---

## Meteorological driver
- **Driving NWP model:** [AROME Hungary](../../../nwp_models/regional/hungary/arome-hungary.md) (HungaroMet's 2.5 km convection-permitting AROME)
- **Coupling:** Offline (one-way) — CHIMERE ingests AROME forecast fields; the dataset description explicitly describes CHIMERE as an "off-line" CTM
- **Update source frequency (if offline):** TBD (not documented)

---

## Chemistry and aerosols
> The HungaroMet dataset description does not document the chemistry/aerosol configuration. The fields below are marked TBD for this deployment; see *Notes* for general CHIMERE-2017 model characteristics.
- **Gas-phase chemical mechanism:** TBD (CHIMERE-2017 default is the MELCHIOR2 reduced mechanism; not confirmed for this deployment)
- **Number of chemical species:** TBD
- **Aerosol treatment:** TBD (CHIMERE uses a sectional aerosol scheme)
- **Aerosol components represented:** TBD (CHIMERE typically represents sulfate, nitrate, ammonium, organic and black carbon, dust, sea salt, and primary PM)
- **Heterogeneous/aqueous chemistry:** TBD

---

## Emissions
- **Anthropogenic inventory:** EMEP gridded emissions (via the CEIP WebDab emission database)
- **Biogenic emissions:** TBD (not documented; CHIMERE commonly uses MEGAN)
- **Wildfire emissions:** TBD
- **Dust scheme:** TBD
- **Sea salt scheme:** TBD
- **Other sources:** TBD

---

## Data assimilation (optional)
- **Assimilates AQ observations:** No air quality data assimilation is described in the HungaroMet dataset description; the system runs as a forecast CTM driven by AROME meteorology and EMEP emissions.

---

## What it provides
Hourly gridded surface concentration forecasts of six pollutants (one parameter per file):

| Parameter | Description | Unit |
|---|---|---|
| CO | Carbon monoxide concentration | ppb |
| NO2 | Nitrogen dioxide concentration | µg/m³ |
| O3 | Tropospheric ozone concentration | µg/m³ |
| SO2 | Sulphur dioxide concentration | µg/m³ |
| PM10 | PM10 (particulate matter) concentration | µg/m³ |
| PM25 | PM2.5 (fine particulate matter) concentration | µg/m³ |

(Note: CO is reported in ppb; all other species in µg/m³.)

---

## Data availability
- **Is the data free?** Yes
- **License:** HungaroMet Open Data Portal terms (free use without modification; attribution required as *"Database: Meteorological Database, HungaroMet Nonprofit Zrt."*; modifications require written consent, with example attribution forms provided in the General Terms of Use). Warnings, alerts, and aviation forecasts may only be redistributed unmodified.
- **Is the data downloadable?** Yes
- **Data formats:** NetCDF (one parameter per file, compressed inside ZIP archives)
- **Official download location:**
  https://odp.met.hu/weather/nwp/CHIMERE/
  - Domain subfolders: `HUN/`, `BUD/`, `MIS/`, `PEC/`
- **File naming convention** (from the dataset description):
  - `CHIMERE_<domain>-<parameter>-<YYYYMMDD>_<HHmm>+<TTTtt>.nc.zip`
  - `<domain>`: domain identifier (HUN / BUD / MIS / PEC)
  - `<parameter>`: pollutant name (CO, NO2, O3, SO2, PM10, PM25)
  - `<YYYYMMDD>`: forecast date
  - `<HHmm>`: forecast initial time (UTC)
  - `<TTTtt>`: forecast lead time, hours (TTT) and minutes (tt)

---

## Notes
- **Offline multi-scale CTM:** CHIMERE is designed to run from the hemispheric scale down to the urban scale, with resolutions from ~1–2 km to hundreds of km. The HungaroMet deployment uses this multi-scale capability to nest three ~1.5 km city domains (Budapest, Miskolc, Pécs) inside the ~10 km Carpathian Basin domain.
- **Meteorological coupling:** Driven offline by HungaroMet's [AROME](../../../nwp_models/regional/hungary/arome-hungary.md) forecast — both systems are operated by HungaroMet and distributed through the same open data portal.
- **Emissions source:** Anthropogenic emissions come from the EMEP/CEIP gridded inventory rather than a national HungaroMet inventory.
- **Verification:** HungaroMet points to the Copernicus Atmosphere Monitoring Service (CAMS) for regularly updated CHIMERE verification results. CHIMERE is also one of the constituent models in the [CAMS Regional](../eu/cams-regional.md) European air quality ensemble; the HungaroMet deployment documented here is an independent national configuration.
- **Uncertainties:** Per the dataset description, forecast uncertainty arises from the input emission inventory, the driving meteorological forecast, and the nonlinear chemical/physical process descriptions in the CTM.
- **Chemistry configuration not documented:** The HungaroMet dataset description does not specify the gas-phase mechanism, aerosol scheme, or vertical structure. The general CHIMERE-2017 characteristics noted in the *Chemistry and aerosols* section are model-level defaults, not confirmed for this specific deployment.

---

## Official documentation
- HungaroMet ODP CHIMERE folder (root): https://odp.met.hu/weather/nwp/CHIMERE/
- CHIMERE public dataset description (PDF, English): https://odp.met.hu/weather/nwp/CHIMERE/Description_airquality_forecast-CHIMERE-en.pdf
- CHIMERE dataset description (PDF, Hungarian): https://odp.met.hu/weather/nwp/CHIMERE/Leiras_levegominoseg_elorejelzes-CHIMERE-hu.pdf
- HungaroMet open data portal (root): https://odp.met.hu/
- CHIMERE model home (LMD / IPSL): https://www.lmd.polytechnique.fr/chimere/
- Copernicus Atmosphere Monitoring Service (CAMS): https://atmosphere.copernicus.eu/
- EMEP / CEIP WebDab emission database: https://www.ceip.at/webdab-emission-database

### Contact
- Open data technical contact: odp@met.hu
