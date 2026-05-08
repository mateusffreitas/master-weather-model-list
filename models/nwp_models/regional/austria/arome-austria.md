# AROME Austria

## What this model is
AROME Austria (operationally referred to as **AROME-Aut**) is the operational convection-permitting regional deterministic numerical weather prediction model run by GeoSphere Austria over Austria and the surrounding Alpine region.

It is designed for short-range forecasting of small-scale and rapidly evolving weather such as thunderstorms, heavy precipitation, fog, foehn and other local wind effects, with particular strength in mountainous terrain. AROME-Aut is the deterministic member of GeoSphere Austria's broader AROME-based suite, which also includes the regional ensemble system **C-LAEF** (Convection-permitting Limited-Area Ensemble Forecasting) and the higher-resolution rapid-update system **AROME-RUC**.

---

## Who runs it
- **Organization:** GeoSphere Austria (formerly ZAMG — Zentralanstalt für Meteorologie und Geodynamik; merged into GeoSphere Austria in 2023)
- **Country / region:** Austria

---

## What area it covers
- **Coverage:** Austria and surrounding Alpine regions
- **Domain details:** 600 × 432 grid points centred on the Alpine region; same domain as the operational C-LAEF ensemble

---

## Basic details
- **Model type:** Regional deterministic NWP (convection-permitting)
- **Model system / core:** AROME (ALADIN-NH non-hydrostatic spectral dynamical core), code cycle **cy43t2bf11**
- **Dynamical formulation:** Non-hydrostatic, spectral, with semi-Lagrangian advection and semi-implicit time integration
- **Convection-allowing:** Yes (deep convection explicitly resolved at 2.5 km; shallow convection parameterized)
- **Horizontal resolution:** ~2.5 km
- **Grid dimensions:** 600 × 432 grid points
- **Vertical levels:** 90 (lowest level ~5 m above ground; model top ~35 km)
- **Forecast length:** 60 hours
- **Update frequency:** 8× daily (00, 03, 06, 09, 12, 15, 18, 21 UTC)
- **Time step:** 60 s
- **Temporal output resolution:** 1 hour (both 2D and 3D fields)
- **Surface scheme:** SURFEX 8.0
- **Orography / physiography:** GMTED2010 / ECOCLIMAP 1

---

## Data assimilation
- **Data assimilation:** Yes
- **Method / cadence:** **3D-Var** for upper-air, **optimal interpolation (OI)** for surface; cycled every 3 hours with a ±90 minute assimilation window
- **Background-error covariance:** C-LAEF EDA climatological B-matrix (shared with the C-LAEF ensemble)
- **Observations assimilated:** Conventional surface (SYNOP, TAWES), aircraft (AMDAR, Mode-S MRAR), radiosonde (TEMP, including TEMP-radiosonde), wind profiler, SCADA wind-energy observations, atmospheric motion vectors (GeoWind from MSG-2/3), satellite radiances (AMSU-A, MHS, ATMS from NOAA-18/19/20, SNPP, MetOp-B/C; IASI from MetOp-B/C; SEVIRI water-vapour channels), ASCAT scatterometer winds, weather radar reflectivity and radial winds, INCA-RR precipitation analysis, GNSS ZTD, GPS radio occultation bending angles, ceilometer cloud cover (converted to RH; introduced operationally in the September 2023 C-LAEF upgrade and shared with AROME-Aut). The assimilation system is shared in setup with the C-LAEF control run.

---

## Initial and boundary conditions
- **Initial conditions:** 3D-Var (upper-air) + OI (surface)
- **Boundary conditions:** ECMWF IFS HRES, updated hourly

---

## What it provides
Deterministic forecasts of:
- 2 m temperature and humidity
- 10 m wind (including gusts)
- Mean sea-level and surface pressure
- Precipitation (rain, snow, graupel)
- Cloud cover (total, low, mid, high) and cloud microphysics
- Boundary-layer parameters and surface fluxes
- Radar reflectivity (simulated)
- Standard upper-air fields on isobaric and height-above-ground levels

---

## Data availability
- **Is the data free?** Yes
- **License:** Creative Commons Attribution 4.0 International (CC BY 4.0)
- **Is the data downloadable?** Yes
- **Data formats:** NetCDF
- **Official download location (direct file listing):**
  https://public.hub.geosphere.at/public/datahub.html?id=nwp-v1-1h-2500m/filelisting
- **API access:** GeoSphere Austria's Dataset API provides programmatic access to the same data with subsetting (by area, time, parameter):
  https://dataset.api.hub.geosphere.at/v1/docs/
- **Dataset landing page:**
  https://data.hub.geosphere.at/dataset/nwp-v1-1h-2500m

GeoSphere Austria publishes the dataset as `nwp-v1-1h-2500m` (1-hour output, 2500 m resolution). Both the direct download portal and the API serve identical data; users can choose whichever access pattern suits their workflow.

---

## Notes
- AROME-Aut and the **C-LAEF control run** are configured identically but run on different HPC platforms, so the C-LAEF control can act as an operational backup for AROME-Aut. C-LAEF is the 16+1-member regional ensemble companion (same 2.5 km / L90 setup), running on the ECMWF ATOS HPC system. **AROME-RUC** is a separate higher-resolution (1.2 km) rapid-update configuration covering Austria with hourly cycling.
- Hardware: AROME-Aut and AROME-RUC run on GeoSphere Austria's **HPE CRAY-XD2000** cluster (100 nodes, AMD Genoa, Slingshot 11 interconnect, Lustre 200 TB), operational for all NWP systems since **15 January 2025**. The previous HPE Apollo 8600 system was retired at that point. The C-LAEF control run continues to operate on the ECMWF ATOS XH2000.
- Organizational note: GeoSphere Austria was formed by the 2023 merger of ZAMG with the Geological Survey of Austria; older publications and documentation refer to ZAMG.
- A high-resolution Austrian regional reanalysis ensemble (**ARA**), built on the C-LAEF system at 2.5 km / L90 with 10+1 members and 3-hourly cycling, has been developed at GeoSphere Austria targeting the period 2013–2023, with ERA5 providing coupling fields. ARA is a research/reanalysis product distinct from this operational forecast entry.
- **Relationship to the wider AROME family:** AROME-Aut shares the same dynamical core and physics packages as the broader AROME / HARMONIE-AROME family operated by ACCORD-consortium services. See [AROME France](../france/arome-france.md), [AROME-Arctic](../norway/arome-arctic.md), [HARMONIE-AROME Ireland](../ireland/harmonie-arome-ireland.md), and [MEPS](../norway/meps.md) for closely related deployments.

---

## Recent version history

### 2025-01-15 — Migration to new HPC
All GeoSphere Austria operational NWP systems (AROME-Aut, AROME-RUC, and other on-site components) migrated to the new **HPE CRAY-XD2000** cluster. AROME-Aut continues to run with the cy43t2bf11 setup; the migration was a hardware change rather than a model upgrade.

### 2023 (September) — Shared assimilation upgrades with C-LAEF
The September 2023 C-LAEF upgrade introduced changes that AROME-Aut shares through its common assimilation framework, most notably the assimilation of **ceilometer-derived cloud cover** (converted to relative humidity), which improves low-stratus forecasts in autumn and winter. The same upgrade replaced the hybrid model-physics perturbation scheme in C-LAEF with a pure stochastic parameter perturbation (SPP) scheme — this change applies to the ensemble and not to the deterministic AROME-Aut.

### Forward look — C-LAEF 1k
Although outside the scope of this deterministic entry, GeoSphere Austria is developing **C-LAEF 1k**, a 1 km / 16+1 member ensemble in cooperation with Slovenia and Croatia, running in lagged mode since early 2025 with a southward domain extension. It is expected to influence the future direction of AROME-Aut's configuration as well.

---

## Official documentation
- GeoSphere Austria data hub (NWP 2.5 km dataset): https://data.hub.geosphere.at/dataset/nwp-v1-1h-2500m
- Direct file listing: https://public.hub.geosphere.at/public/datahub.html?id=nwp-v1-1h-2500m/filelisting
- Dataset API documentation: https://dataset.api.hub.geosphere.at/v1/docs/
- GeoSphere Austria homepage: https://www.geosphere.at

### Key references
- Weidle, F., Awan, N., Neduncheran, A., Meier, F., Scheffknecht, P., Wastl, C., Wittmann, C. (2024). *NWP related activities in AUSTRIA.* 4th ACCORD All Staff Workshop, Norrköping, Sweden, 15–19 April 2024.
- Weidle, F., Awan, N., Deacu, D., Neduncheran, A., Meier, F., Schneider, S., Wastl, C., Wittmann, C. (2025). *NWP related activities in AUSTRIA.* 5th ACCORD All Staff Workshop, Zalakaros, Hungary, 31 March – 4 April 2025.
- Montmerle, T., Michel, Y., Arbogast, E., Ménétrier, B., Brousseau, P. (2018). *A 3D ensemble variational data assimilation scheme for the limited-area AROME model: Formulation and preliminary results.* Quarterly Journal of the Royal Meteorological Society, 144(716): 2196–2215. https://doi.org/10.1002/qj.3334
