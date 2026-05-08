# MEPS (MetCoOp Ensemble Prediction System)

## What this model is
MEPS (MetCoOp Ensemble Prediction System) is a **high-resolution, convection-permitting regional ensemble numerical weather prediction (NWP) system** based on HARMONIE-AROME.

It provides short-range probabilistic forecasts and quantifies forecast uncertainty over the Nordic region for high-impact weather such as heavy precipitation, strong winds, and convection.

---

## Who runs it
- **Organization:** MetCoOp (Meteorological Cooperation on Operational Numerical Weather Prediction)
  - Member institutes: MET Norway, SMHI (Sweden), FMI (Finland), Estonian Environment Agency (ESTEA), and LEGMC (Latvia, joined 2024)
- **Country / region:** Multi-national (Nordic and Baltic)

---

## What area it covers
- **Coverage:** Scandinavia and Finland (Nordic region, including surrounding marine areas)
- **Domain details:** ~2.5 km Lambert grid covering Norway, Sweden, Finland, and surrounding seas (North Sea, Norwegian Sea, Baltic Sea, parts of the Barents Sea). Domain expanded by ~20% in February 2020.

---

## Basic details
- **Model type:** Regional ensemble NWP (convection-permitting)
- **Model system / core:** HARMONIE-AROME (ALADIN-NH non-hydrostatic spectral dynamical core)
- **Dynamical formulation:** Non-hydrostatic, spectral, with two-time-level semi-implicit semi-Lagrangian discretization
- **Convection-allowing:** Yes (deep convection explicitly resolved at 2.5 km; shallow convection parameterized)
- **Ensemble size:** 30 perturbed members + 1 control (control is non-lagged; perturbed members are time-lagged up to 6 hours, with 5 new members generated every hour)
- **Horizontal resolution:** ~2.5 km
- **Vertical levels:** 65 (lowest level ~12 m above ground)
- **Model top:** 10 hPa (~30 km)
- **Forecast length:**
  - 61 hours for perturbed ensemble members
  - 66 hours for the control member
- **Update frequency / cycles:**
  - Ensemble: 5 new perturbed members initialized every hour (continuous cycling)
  - Control member: every 3 hours
- **Temporal output resolution:** 1 hour

---

## Data assimilation
- **Data assimilation:** Yes
- **Method / cadence:** Upper-air 3D-Var and surface analysis (CANARI-style optimal interpolation for surface fields), as used in the broader HARMONIE-AROME system. Assimilated observations include conventional surface, aircraft (including Mode-S EHS), radiosonde, satellite radiances, GPS-RO, and weather radar reflectivity.

---

## Initial and boundary conditions
- **Initial conditions:** HARMONIE-AROME analyses with ensemble perturbations (initial-condition perturbations and stochastic physics perturbations applied across members)
- **Boundary conditions:** ECMWF IFS (lateral boundary conditions)

---

## What it provides
Probabilistic and deterministic forecasts of:
- temperature (2 m, model levels, pressure levels)
- wind (10 m, model levels, pressure levels)
- precipitation (including precipitation type: drizzle, rain, sleet, snow, freezing drizzle, freezing rain, graupel, hail)
- mean sea level and surface pressure
- humidity
- cloud cover and hydrometeor fields
- surface fluxes and radiation
- vertical profiles and cross-sections (separate `vc` files)

Ensemble products derived from the 30+1 members include ensemble means and spread, percentiles, and probability-of-exceedance fields.

---

## Data availability
- **Is the data free?** Yes
- **License:** Norwegian Licence for Open Government Data (NLOD) and Creative Commons Attribution 4.0 International (CC BY 4.0). Attribution required: "MET Norway" or equivalent (e.g. "Data from The Norwegian Meteorological Institute" / "Based on data from MET Norway").
- **Is the data downloadable?** Yes
- **Data formats:** NetCDF and NCML (NCML files are aggregations pointing to underlying NetCDF files; accessible via OPeNDAP)
- **Official download location:**
  - MEPS latest catalogue: https://thredds.met.no/thredds/catalog/mepslatest/catalog.html
  - MEPS archive: https://thredds.met.no/thredds/catalog/meps25epsarchive/catalog.html

---

## Notes
- **Repository location:** MEPS is an ensemble system, but is currently filed under `nwp_models/regional/norway/` rather than `ensemble_models/regional/norway/`. Consider relocating to align with the repository's convention of separating deterministic and ensemble systems (and using `ensemble-model.template.md` for ensemble-specific fields like perturbation strategy).
- **File naming convention:** Files follow `<model>_<form>_<resolution>_<DTG>.<type>`, where `<form>` is `det` for the control member, or `lagged_Xh_subset`/`lagged_Xh_latest` for time-lagged ensemble output. Most files in the latest catalogue are NCML-only and must be accessed via OPeNDAP rather than downloaded directly. Some files (`*_latest.nc`, `meps_lagged_6_h_latest_*`, `meps_det_vc_*`) are also available as NetCDF.
- **Time-lagging:** Since the February 2020 reorganization, MEPS runs 5 ensemble members every hour rather than larger batches at fixed cycle times. Ensemble files therefore contain 30 members at varying lag (up to 6 hours), while the control member is non-lagged. Only a subset of parameters from all members is archived (in 6-hourly subset files); model-level information is archived only for the control member.
- **MET Nordic post-processed product:** A separate higher-resolution (1 km) post-processed product, [MET Nordic](https://github.com/metno/NWPdocs/wiki/MET-Nordic-dataset), is built on top of MEPS and distributed via the same THREDDS server. It is not the MEPS model itself but a downstream nowcasting / blending product.
- **Relationship to siblings:**
  - [AROME-Arctic](../norway/arome-arctic.md) — MET Norway's deterministic Arctic-domain HARMONIE-AROME model. AROME-Arctic generally tracks MEPS upgrades after some delay, with several configuration differences (no radar assimilation, different surface scheme settings, no MWHS-2/FengYun-3C assimilation).
  - [HARMONIE-AROME Ireland](../ireland/harmonie-arome-ireland.md), [AROME Hungary](../hungary/arome-hungary.md), and other ACCORD-consortium HARMONIE-AROME and AROME deployments share the same core model.
- **United Weather Centres (UWC):** MetCoOp is one of two regional UWC clusters; the other is **UWC-West** (KNMI, DMI, Met Éireann, Veðurstofa Íslands), which runs a separate joint operational suite. The two clusters target a common UWC NWP production by ~2027. **DMI is in UWC-West, not MetCoOp.**

---

## Recent version history

### 2024-12-10 — MEPS archive cleaning
Introduction of a cleaning policy for the MEPS archive. See [MET Norway's announcement](https://github.com/metno/NWPdocs/wiki/Updates-to-MEPS-and-AROME%E2%80%90Arctic-archive-and-cleaning-2024).

### 2023-10-01 — New MEPS archive regime
Archive structure changed. `meps_det_*.nc` and `meps_subset_*.nc` NetCDF files are no longer archived as monolithic files. Instead, one file is produced per member, level type, and time step, with NCML files aggregating these. See [New MEPS Archive 2023](https://github.com/metno/NWPdocs/wiki/New-MEPS-Archive-2023).

### 2021-03-23 — Upgrade to HARMONIE cycle 43 (cy43)
Minor changes to forecast quality. See the [cy43 upgrade note](https://github.com/metno/NWPdocs/blob/master/documents/MEPS_update_cy43.pdf).

### 2020-02-04 — Continuous cycling and domain expansion
Major reorganization of the production schedule. Previously MEPS ran 10 members at fixed 00/06/12/18 UTC cycles; from this date it runs 5 new members every hour with 66-hour forecast length, producing 30 time-lagged ensemble members + 1 non-lagged control. The domain was expanded by ~20%. NetCDF outputs in the latest catalogue were largely replaced by NCML aggregations. SURFEX-derived parameters were prefixed with `SFX_` in variable names. Post-processed (`pp`) files were discontinued in favour of the separate MET post-processed (yr) product chain.

---

## Official documentation
- MEPS dataset documentation: https://github.com/metno/NWPdocs/wiki/MEPS-dataset
- MetCoOp project overview: https://www.met.no/en/projects/metcoop
- Licensing and crediting: https://www.met.no/en/free-meteorological-data/Licensing-and-crediting
- Müller et al. (2017), *AROME-MetCoOp: A Nordic Convective-Scale Operational Weather Prediction Model*, Wea. Forecasting, 32, 609–627. https://doi.org/10.1175/WAF-D-16-0099.1
- Frogner et al. (2019), *Convection-permitting ensembles: Challenges related to their design and use*, QJRMS. https://doi.org/10.1002/qj.3525
- Bengtsson et al. (2017), *The HARMONIE-AROME model configuration in the ALADIN-HIRLAM NWP system*, Mon. Wea. Rev., 145, 1919–1935.
