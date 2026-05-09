# ALARO Belgium (RMI)

## What this model is
ALARO Belgium is the **regional limited-area deterministic numerical weather prediction (NWP) system** operated by the Royal Meteorological Institute of Belgium.

It is built on the **ALADIN** spectral limited-area modelling system using the **ALARO physics package** (currently **ALARO-1**), a configuration designed for *gray-zone* resolutions between mesoscale and convection-permitting scales. Development takes place within the **ACCORD** consortium and the predecessor **ALADIN** consortium.

The dataset documented here — **`alaro_40l`** — is the publicly distributed product: ~4 km horizontal resolution with 40 vertical levels, packaged in GRIB. RMI also runs additional internal configurations (a 4 km / L87 ALARO operational run with longer forecast range and a 1.3 km / L87 non-hydrostatic ALARO/AROME run) that are not part of the public `alaro_40l` distribution.

---

## Who runs it
- **Organization:** Royal Meteorological Institute of Belgium (RMI / KMI / IRM)
- **Country / region:** Belgium

---

## What area it covers
- **Coverage:** Belgium and surrounding parts of Western Europe
- **Domain details:** Lambert conformal conic projection centred near Belgium (central meridian ≈ 4.55°E, latitude of origin ≈ 50.57°N), based on the operational ALARO-Belgium NWP domain
- **Grid dimensions (public dataset):** TBD (the internal 4 km operational ALARO runs on 432 × 432 grid points)

---

## Basic details
- **Model type:** Regional deterministic NWP (limited-area)
- **Model system / core:** ALADIN (ALARO Canonical Model Configuration)
- **Dynamical formulation:** Hydrostatic (the public 4 km ALARO; RMI's internal 1.3 km ALARO is non-hydrostatic)
- **Convection-allowing:** No (4 km grid; deep convection is parameterized by the ALARO physics, designed specifically for the gray-zone)
- **Code version (internal operational reference):** CY43T2 with ALARO-1 (as documented for RMI's operational ALARO chain in 2021 and as used through CORDEX.be II climate runs in 2024)
- **Physics package:** ALARO-1 — including the modular Multiscale Microphysics and Transport (3MT) deep convection scheme with non-saturated downdraft, the TOUCANS turbulence scheme, and the ACRANEB2 broadband radiation scheme
- **Horizontal resolution:** ~4 km
- **Vertical levels:** 40 (in the public `alaro_40l` distribution; RMI's internal 4 km operational ALARO uses 87 levels)
- **Forecast length:** Up to ~60 hours (TBD for the public distribution specifically; +60 h is the documented range of the internal 4 km operational ALARO chain)
- **Update frequency:** 4× daily (00, 06, 12, 18 UTC) — the most recent 4 runs are kept on the FTP server
- **Temporal output resolution:** TBD

---

## Data assimilation
- **Data assimilation:** Limited (development ongoing)
- **Method / cadence:** As of the 2021 status report, RMI was developing 3D-Var assimilation cycles for ALARO and AROME under the DASKit project, targeting a 3-hourly cycle. AROME-Belgium had a CANARI-OImain 3-hourly **surface** assimilation cycle in operation; SYNOP, TEMP and AMDAR were being assimilated, with GNSS, Mode-S and radar (BDZH) being monitored or tested. The 2024 RMI Seamless Prediction Programme talk describes a 3 h DA cycle for AROME-Belgium. The exact assimilation status of the publicly distributed `alaro_40l` configuration is TBD.

---

## Initial and boundary conditions
- **Initial conditions:** TBD (likely produced from ARPEGE-coupled fields and any operational ALARO surface analysis)
- **Lateral boundary conditions:** **ARPEGE** (Météo-France global model), with 3-hourly coupling for the 4 km ALARO chain

---

## What it provides
Deterministic forecasts of standard meteorological variables, including:
- Temperature, maximum and minimum temperature, dew-point temperature, wet-bulb potential temperature
- 10 m U/V wind components and stationary boundary layer (SBL) gust
- Cloud cover (low / medium / high / total)
- Total precipitation; large-scale and convective rain; large-scale and convective snow
- Relative and specific humidity
- Geopotential and mean sea-level pressure
- Orography
- Freezing level / 0 °C isotherm
- Convective Available Potential Energy (CAPE)
- Vertical velocity

---

## Data availability
- **Is the data free?** Yes
- **License:** TBD — the RMI open-data portal does not publish explicit open-licence terms (e.g. CC BY) on the dataset metadata page; metadata states INSPIRE Directive 2007/2/EC conformity. Users requiring legally-binding terms should contact RMI directly.
- **Is the data downloadable?** Yes
- **Data formats:** GRIB (decoding requires the RMI local concept tables `params227-1.tab` and `params227-228.tab`, available from the documentation page below)
- **Public dataset name:** `alaro_40l`
- **Official download location:**
  https://opendata.meteo.be/ftp/forecasts/alaro_40l/
  (FTP also reachable at `ftp://opendata24-me.oma.be/forecasts/alaro_40l/`; only the latest 4 runs are retained)
- **Web services:** OGC WCS 1.1.0 and WMS 1.3.0 endpoints are also published from the same dataset:
  - WCS GetCapabilities: https://opendata.meteo.be/service/alaro/wcs?service=WCS&version=1.1.0&request=GetCapabilities
  - WMS GetCapabilities: https://opendata.meteo.be/service/alaro/wms?service=WMS&version=1.3.0&request=getCapabilities

---

## Notes
- The dataset name `alaro_40l` refers to the **40 vertical levels** in the publicly distributed product, **not** a horizontal resolution — the model itself runs at ~4 km horizontally. RMI's internal operational ALARO uses 87 vertical levels; the L40 distribution is a public-release subset.
- The publicly distributed `alaro_40l` is the only deterministic NWP product RMI currently distributes openly. Internally, RMI also operates:
  - A **4 km / L87 ALARO** operational chain (CY43T2 + ALARO-1; +60 h, 3-hourly ARPEGE coupling) — the parent of `alaro_40l`.
  - A **1.3 km / L87 non-hydrostatic ALARO** running operationally since 2019, coupled hourly to the 4 km ALARO run, with a tuned configuration introduced in 2021 (improved TTE solver, updated roughness-length climatology) targeted for full operational status.
  - A **1.3 km / L87 AROME-Belgium** with a 3 h surface assimilation cycle (CANARI-OImain) and ongoing 3D-Var development. AROME-Belgium is coupled to **IFS** (ECMWF), in contrast to ALARO's ARPEGE coupling.
  - The **INCA-BE** nowcasting system (1 km, two parallel versions backed by ALARO and AROME respectively).
  - The **pySTEPS-BE** ensemble nowcast (5-minute time step out to +6 h, 48 members) and the seamless probabilistic system being developed under RMI's *Seamless Prediction Programme* (project IMA).
  None of those internal systems is openly distributed under the `alaro_40l` dataset.
- ALARO is also used at RMI for **regional climate** runs over Belgium and Europe (CORDEX.be I, CORDEX.be II, EURO-CORDEX), at resolutions from 25 km down to 1.3 km, again using CY43T2 + ALARO-1. These climate runs are research products distinct from the operational NWP described here.
- For the parent global model, see [ARPEGE Global](../../global/france/arpege-global.md). For other ALARO-based operational deployments in the consortium, see [ALADIN Slovakia](../slovakia/aladin-slovakia.md). For convection-permitting NWP coverage of the same broader region, see [HARMONIE-AROME Netherlands](../netherlands/harmonie-arome-netherlands.md), whose Dutch domain extends well into Belgium.

---

## Recent version history

### 2021 — ALARO-1 / CY43T2 operational at RMI
The 4 km operational chain was running CY43T2 with ALARO-1, including non-saturated downdraft in 3MT, the TOUCANS turbulence scheme, and the ACRANEB2 radiation scheme, on a 432 × 432 / L87 grid coupled 3-hourly to ARPEGE.

### 2021 — Tuned 1.3 km ALARO ("MODC")
A retuned 1.3 km configuration with revised dynamics/physics tunings (provided via cooperation with CHMI), an improved Total Turbulent Energy (TTE) solver, and an updated roughness-length climatology was put into daily use in September 2021, with operational status targeted for April 2022. RMSE improvements were seen across mean sea-level pressure, 10 m wind, and 2 m temperature, together with reduced cold (winter) and warm (summer) biases.

### 2024 — Seamless Prediction Programme
Work on RMI's *Seamless Prediction Programme* (project IMA) was reported, integrating ALARO and AROME with INCA-BE and pySTEPS-based blending to provide a probabilistic, observation-driven, convection-permitting forecasting system from +5 min out to +24 h. This concerns the broader RMI forecasting suite rather than the public `alaro_40l` dataset specifically.

---

## Official documentation
- RMI open-data dataset documentation (parameter tables): https://opendata.meteo.be/documentation/?dataset=alaro
- RMI open-data portal: https://opendata.meteo.be/
- Dataset metadata (RMI catalogue): https://opendata.meteo.be/geonetwork/srv/eng/catalog.search#/metadata/RMI_DATASET_ALARO
- Dataset metadata (geo.be federal catalogue): https://www.geo.be/catalog/details/RMI_DATASET_ALARO
- ALARO model page (RMI): https://www.meteo.be/en/research-rmi/scope-of-research/numerical-model-alaro/numerieke-weersvoorspellingen
- Public ALARO charts: https://www.meteo.be/en/weather/forecasts/weather-model-alaro

### Key references
- Termonia, P. et al. (2018). *The ALADIN System and its canonical model configurations AROME CY41T1 and ALARO-0 CY40T1.* Geoscientific Model Development, 11, 257–281. DOI: 10.5194/gmd-11-257-2018
- De Troch, R. et al. (2013). *Multiscale performance of the ALARO-0 model for simulating extreme summer precipitation climatology in Belgium.* Journal of Climate, 26, 8895–8915.
- Caluwaerts, S. et al. (2021). *The operational ALADIN-Belgium model.* RMI poster, Belgium National contribution (April 2021).
- Reyniers, M., Van Ginderachter, M., De Cruz, L. et al. (2024). *The Seamless Prediction Programme at the Royal Meteorological Institute of Belgium.* EUMETSAT Integrated Nowcasting Workshop, 23–25 January 2024, Darmstadt, Germany.
- Dewettinck, W. et al. (2024). *State of regional climate modelling in Belgium with the ALARO model.* ACCORD All-Staff Workshop, 2024.
