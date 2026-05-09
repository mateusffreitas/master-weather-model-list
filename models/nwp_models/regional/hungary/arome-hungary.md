# AROME Hungary

## What this model is
AROME Hungary is the operational **convection-permitting regional deterministic numerical weather prediction (NWP) model** run by HungaroMet (Hungarian Meteorological Service / OMSZ) over the Carpathian Basin.

It is designed for **short-range forecasting and nowcasting** of high-impact convective weather — thunderstorms, squall lines, mesoscale convective systems, heavy precipitation, and rapidly evolving local-scale events. AROME Hungary is one member of the wider AROME / HARMONIE-AROME family operated by national services in the **ACCORD** consortium (formerly the ALADIN and HIRLAM consortia, merged in 2021), with development also coordinated through the **RC LACE** Central European cooperation.

HungaroMet also operates a regional **ALADIN** deterministic configuration (8 km, hydrostatic) for synoptic-scale forecasting; that model is run on the same supercomputer alongside AROME but is not currently distributed on the open data portal.

---

## Who runs it
- **Organization:** HungaroMet (Hungarian Meteorological Service, formerly OMSZ — Országos Meteorológiai Szolgálat)
- **Country / region:** Hungary

---

## What area it covers
- **Coverage:** Hungary and the wider Carpathian Basin (Central Europe)
- **Domain bounds (publicly distributed grid):** **14.3°E – 24.3°E**, **44.9°N – 49.5°N**

---

## Basic details
- **Model type:** Regional deterministic NWP (convection-permitting)
- **Model system / core:** AROME (ALADIN-NH dynamical core, with Meso-NH–derived physics)
- **Code version:** cy43t3 (operational since 2021; previously cy40t1 from 2019)
- **Dynamical formulation:** Non-hydrostatic, spectral, with semi-Lagrangian advection and semi-implicit time integration
- **Convection-allowing:** Yes (deep convection explicitly resolved at 2.5 km; only shallow convection parameterized)
- **Horizontal resolution:** ~2.5 km, distributed on a 0.025° × 0.025° regular latitude–longitude grid
- **Vertical levels:** 59
- **Model top:** ~2.7 hPa (~23 km)
- **Forecast length:** Up to 48 hours (publicly distributed product)
- **Update frequency:** 4× daily on the open data portal (HungaroMet's internal operational AROME runs more frequently — see *Notes*)
- **Temporal output resolution:** 1 hour

---

## Data assimilation
- **Data assimilation:** Yes
- **Method / cadence:** 3D-Var on atmospheric variables, with surface fields obtained by interpolation from the ALADIN analysis; 3-hourly assimilation cycle. AROME's own high-resolution 3D-Var has been operational at HungaroMet since March 2013, initially using conventional observations (surface, radiosonde, aircraft); ongoing development targets denser use of satellite, GNSS, and radar data.

---

## Initial and boundary conditions
- **Initial conditions:** AROME 3D-Var analysis (atmospheric variables) + surface fields interpolated from the parent ALADIN analysis
- **Lateral boundary conditions:** ECMWF IFS (global model). Older HungaroMet documentation describes the parent HungaroMet ALADIN configuration as the AROME coupling source; the most recently published configuration table indicates ECMWF as the coupling source. The exact current chain is not confirmed in the public open-data PDF.

---

## What it provides
Deterministic forecasts of a wide range of atmospheric fields, distributed as one variable per file. The full parameter list is documented in the official dataset description (PDF linked below); a summary follows.

**2-D surface fields:**
- 2 m temperature, dewpoint, relative humidity, daily min/max
- 10 m U/V wind components and wind gusts
- Surface pressure, mean sea-level pressure
- Total cloud cover
- Total instantaneous precipitation, accumulated precipitation, total snow + graupel, total rainfall, snow mass, snow depth
- Surface temperature, accumulated short-wave surface radiation
- Planetary boundary layer height
- Convective available potential energy (CAPE), convective condensation level (CCL)
- Surface geopotential and land–sea mask (first time step only)

**3-D fields on standard pressure levels (1000, 850, 700, 500 hPa):**
- U/V wind, temperature, geopotential, relative humidity

**3-D fields on height levels (100 m above surface):**
- U/V wind, temperature, relative humidity (planetary boundary layer)

---

## Data availability
- **Is the data free?** Yes
- **License:** HungaroMet Open Data Portal terms (free use without modification; attribution required as *"Database: Meteorological Database, HungaroMet Nonprofit Zrt."*; modifications require written consent, with example attribution forms provided in the General Terms of Use). Warnings, alerts, and aviation forecasts may only be redistributed unmodified.
- **Is the data downloadable?** Yes
- **Data formats:** NetCDF (one variable per file, gzip-compressed inside ZIP archives)
- **Official download location:**
  https://odp.met.hu/weather/nwp/AROME/nc/
- **File naming conventions** (from the dataset description):
  - 2-D fields: `AROME-<variable>-<YYYYMMDD>_<HHmm>+<TTTtt>.nc.zip`
  - 3-D pressure-level fields: `AROME-<variable>_p<pressure_level>-<YYYYMMDD>_<HHmm>+<TTTtt>.nc.zip`
  - 3-D height-level fields: `AROME-<variable>_h<height_level>-<YYYYMMDD>_<HHmm>+<TTTtt>.nc.zip`

---

## Notes
- **Public subset vs internal operations:** The open data portal distributes AROME forecasts at 4 cycles per day out to +48 h. HungaroMet's internal operational AROME chain runs more frequently and produces longer forecast ranges (per HungaroMet's modeling activities page). Users requiring sub-3-hourly cycles or extended forecast ranges should contact HungaroMet directly.
- **Sibling system at HungaroMet — ALADIN (not publicly distributed):** HungaroMet also runs an **ALADIN** configuration at 8 km horizontal resolution with 49 vertical levels, hydrostatic dynamics, and parameterized convection, covering most of continental Europe. ALADIN supplies surface initial conditions and, historically, lateral boundary information to AROME, but it is not currently published on the ODP open data portal. Should it become publicly available in the future, it would fit the same `models/nwp_models/regional/hungary/` directory.
- **Ensemble counterpart:** HungaroMet operates an ALADIN-based regional ensemble for probabilistic forecasting (boundary-coupled to the Météo-France PEARP global ensemble). This is a separate product and would be documented under regional ensemble models if and when openly distributed.
- **Relationship to siblings in the AROME / HARMONIE-AROME family:** AROME Hungary shares the same dynamical core and physics packages as other ACCORD-consortium AROME / HARMONIE-AROME deployments. See [AROME France](../france/arome-france.md), [AROME Austria](../austria/arome-austria.md), [AROME-Arctic](../norway/arome-arctic.md), [HARMONIE-AROME Ireland](../ireland/harmonie-arome-ireland.md), and [MEPS](../norway/meps.md) for closely related systems. Other ALADIN/ALARO Central European deployments include [ALADIN Slovakia](../slovakia/aladin-slovakia.md), [ALADIN-HR (Croatia)](../croatia/aladin-hr.md), and [ALARO Belgium](../belgium/alaro-belgium.md).
- **Computing platform:** AROME and ALADIN are run on HungaroMet's in-house supercomputer. (Specific hardware details have evolved since the most recent published HungaroMet documentation.)
- **RC LACE / OPLACE:** HungaroMet leads the **OPLACE** observation pre-processing service for the RC LACE cooperation, providing pre-processed observation streams for data assimilation across partner services.

---

## Recent version history

### 2021 — Upgrade to AROME cycle 43t3 (cy43t3)
The operational AROME chain was upgraded to **cy43t3**, the same code base used in several ACCORD-consortium services in the same period (cf. [MEPS](../norway/meps.md) which moved to cy43 in March 2021). Specific impact details for AROME Hungary are not documented in the public open-data PDF beyond the version label.

### 2019 — Upgrade to AROME cycle 40t1 (cy40t1)
First version-tagged release documented in the open-data dataset description.

---

## Official documentation
- HungaroMet AROME page (English): https://www.met.hu/en/idojaras/elorejelzes/modellek/AROME/
- HungaroMet open data portal (root): https://odp.met.hu/
- AROME public dataset description (PDF): https://odp.met.hu/weather/nwp/AROME/Description_shortrange_forecast-AROME-en.pdf
- HungaroMet Open Data Portal General Terms of Use (PDF): https://odp.met.hu/ (linked from the ODP root)
- HungaroMet regional modeling activities (Hungarian): https://www.met.hu/rolunk/tevekenysegek/idojarasmodellezes/modellek/
- HungaroMet data assimilation page (Hungarian): https://www.met.hu/rolunk/tevekenysegek/idojarasmodellezes/adatasszimilacio/
- HungaroMet physical parameterizations page (Hungarian): https://www.met.hu/rolunk/tevekenysegek/idojarasmodellezes/parametrizacio/
- ACCORD consortium: https://www.umr-cnrm.fr/accord/
- RC LACE Central European cooperation: http://www.rclace.eu/

### Key reference
- Szintai, B., Szűcs, M., Randriamampianina, R., Kullmann, L. (2015). *Application of the AROME non-hydrostatic model at the Hungarian Meteorological Service: physical parametrizations and ensemble forecasting.* Időjárás, **119**, 241–265.

### Contact
- Open data technical contact: odp@met.hu
- Licensing / consent for modifications: service@met.hu
