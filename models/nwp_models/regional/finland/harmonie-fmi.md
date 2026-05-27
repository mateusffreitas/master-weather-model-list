# HARMONIE (MEPS) — FMI distribution

## What this model is
This entry documents the **Finnish Meteorological Institute (FMI) distribution of HARMONIE (MEPS)** — the MetCoOp Ensemble Prediction System, a high-resolution, convection-permitting regional NWP system based on HARMONIE-AROME.

The underlying model is the **same MetCoOp production** documented in the [MEPS](../norway/meps.md) entry: FMI is a co-producing member of MetCoOp (one of the three HPC platforms that runs the MEPS ensemble is FMI's "Vinha") and redistributes the MEPS output through its own Open Data service. What differs is the **access mechanism** (an FMI WFS stored query plus a binary download service) and the **distributed subset** (FMI currently serves surface-level data only). This is not a separate forecast — it is the MetCoOp MEPS run repackaged and delivered through FMI's infrastructure.

---

## Who runs it
- **Organization:** MetCoOp (Meteorological Cooperation on Operational Numerical Weather Prediction). Distributed by the **Finnish Meteorological Institute (FMI)** as one of the MetCoOp member institutes; FMI also hosts one of the three HPC platforms (Vinha) on which the MEPS ensemble is produced.
  - MetCoOp member institutes: MET Norway, SMHI (Sweden), FMI (Finland, since 2017), Estonian Environment Agency (ESTEA, since 2020), and LEGMC (Latvia, since 2024)
- **Country / region:** Finland (distributor); multi-national (Nordic and Baltic) production

---

## What area it covers
- **Coverage:** Scandinavia and Finland (Nordic region, including surrounding marine areas) — the MetCoOp MEPS domain (960 × 1080 grid points at 2.5 km uniform spacing)
- **Domain details:** The FMI download service exposes the data via the `harmonie_scandinavia_surface` producer; the default bounding box returned by the WFS query spans approximately 18.1°W–54.2°E and 49.8°N–75.2°N. Users are expected to subset to their area of interest with query parameters.

---

## Basic details
- **Model type:** Regional ensemble NWP (convection-permitting) — FMI distributes the surface-level subset
- **Model system / core:** HARMONIE-AROME (ALADIN-NH non-hydrostatic spectral dynamical core), currently cycle 43h2.2
- **Dynamical formulation:** Non-hydrostatic, spectral, with two-time-level semi-implicit semi-Lagrangian discretization
- **Convection-allowing:** Yes (deep convection explicitly resolved at 2.5 km; shallow convection parameterized)
- **Ensemble size:** 30 perturbed members + 1 control (see [MEPS](../norway/meps.md) for the ensemble/time-lagging design)
- **Horizontal resolution:** ~2.5 km
- **Vertical levels:** 65 (production); **FMI distributes surface-level fields only** (`levels=0`)
- **Model top:** 10 hPa (~30 km) in production
- **Forecast length:** Up to 66 hours (control member); 61 hours for the perturbed ensemble members. See [MEPS](../norway/meps.md) for details.
- **Update frequency / cycles:** Continuous cycling in production (MetCoOp); FMI exposes the latest available runs through `origintime`/`analysisTime`
- **Temporal output resolution:** 1 hour (selectable via the `timestep` parameter, e.g. 60 minutes)

---

## Data assimilation
- **Data assimilation:** Yes (MetCoOp production)
- **Method / cadence:** Upper-air 3D-Var with large-scale mixing, and surface analysis via CANARI optimal interpolation. Assimilated observations include conventional in-situ (SYNOP, SHIP, buoy, radiosonde), aircraft (AMDAR and Mode-S EHS), GNSS ZTD, weather radar reflectivity and Doppler radial wind, scatterometer, and microwave/IR satellite radiances. See the [MEPS](../norway/meps.md) entry for the full assimilation description; data assimilation is a property of the MetCoOp production, not the FMI distribution layer.

---

## Initial and boundary conditions
- **Initial conditions:** HARMONIE-AROME analyses with ensemble perturbations
- **Boundary conditions:** ECMWF IFS (lateral boundary conditions; Météo-France ARPEGE as automatic backup)

(These are production properties shared with [MEPS](../norway/meps.md).)

---

## What it provides
FMI distributes **surface-level** forecast fields. The `harmonie_scandinavia_surface` producer exposes parameters including:
- temperature (2 m), dew point, relative humidity
- wind (10 m): U/V components, wind speed, wind direction, wind gust
- precipitation amount
- mean sea level / surface pressure, geopotential height
- total, low, medium, and high cloud cover
- CAPE
- visibility
- global / net surface shortwave and longwave radiation (instantaneous and accumulated)

Note: the binary download service's documented `param` list is narrower than the full WFS-advertised property set; available parameters can be confirmed from the `observedProperties` field in the WFS response. Upper-air, model-level, and pressure-level fields are **not** part of the FMI surface distribution — for those, use the MET Norway THREDDS distribution documented in [MEPS](../norway/meps.md).

---

## Data availability
- **Is the data free?** Yes — no registration or account required (the user must accept the Creative Commons licence before using the open data interfaces)
- **License:** Creative Commons Attribution 4.0 International (CC BY 4.0). Attribution to the Finnish Meteorological Institute required. The CC BY 4.0 licence applies to FMI's open data sets (as well as Finnish Transport Agency and STUK data sets distributed through the same service).
- **Is the data downloadable?** Yes
- **Data formats:** GRIB2, GRIB1, and NetCDF (selectable via the `format` parameter; GRIB2 files decode with ECMWF's ecCodes / GRIB API)
- **Official download location:**
  - WFS stored query (lists available runs): `https://opendata.fmi.fi/wfs?service=WFS&version=2.0.0&request=getFeature&storedquery_id=fmi::forecast::harmonie::surface::grid&`
  - Binary download service (direct file download): `https://opendata.fmi.fi/download` (and `data.fmi.fi`) with the `harmonie_scandinavia_surface` producer

### How to access — FMI Open Data

FMI delivers gridded forecast model data in two steps, or via direct download:

1. **WFS stored query** — `fmi::forecast::harmonie::surface::grid` returns a collection of `GridSeriesObservation` features, one per available model run, including forecast period (`phenomenonTime`), available properties (`observedProperties`), forecast area geometry (`featureOfInterest`), and nominal run time (`analysisTime`). The `om:result` element does **not** embed the grid; instead it contains a `gml:fileReference` URL pointing to the binary download service.
2. **Binary Download Service** — a separate HTTP request to the referenced URL (under `opendata.fmi.fi/download` / `data.fmi.fi`) returns the actual GRIB2/GRIB1/NetCDF file.

The binary files can also be downloaded **directly** without making the WFS request first, by constructing a download URL against the `harmonie_scandinavia_surface` producer.

**Key query parameters for the download service:** `starttime`, `endtime`, `origintime` (nominal model run time), `timestep` (minutes) or `timesteps` (count), `param` (comma-separated parameters), `format` (`grib1` | `grib2` | `netcdf`), `projection` (e.g. `EPSG:4326`), and spatial subsetting via `bbox`, `gridcenter`, `gridsize`, `gridstep`, or `gridresolution`.

**Supported projections** include `EPSG:4326`, `EPSG:3995`, and polar stereographic (latitude of origin 60, central meridian 0). If no projection is specified, the data's native projection is used.

**Example (GRIB2, vicinity of Tampere, temperature and wind):**
`https://opendata.fmi.fi/download?producer=harmonie_scandinavia_surface&param=temperature,windums,windvms&format=grib2&gridcenter=23.7,61.5,100,100&projection=EPSG:4326&`

**Important:** forecast model files can be very large. FMI strongly recommends constraining each query with spatial and parameter filters to the exact area and variables of interest.

### Request limits
- Download Service: 20,000 requests per day
- View Service: 10,000 requests per day
- Combined Download + View: 600 requests per 5 minutes

### Client libraries
FMI and the community provide helper libraries for the open data interfaces:
- Python: https://github.com/pnuu/fmiopendata
- R (rOpenGov): https://github.com/rOpenGov/fmi2
- JavaScript (metolib): https://github.com/fmidev/metolib

---

## Notes
- **Same model as [MEPS](../norway/meps.md), different distribution.** FMI co-produces MetCoOp MEPS (its Vinha HPC runs members 9, 10, 11 of the ensemble, alongside SMHI/MET Norway's Cirrus and Stratus systems) and redistributes the output. The forecast state is identical to the MET Norway MEPS distribution; the differences are the access mechanism (FMI WFS + binary download service vs MET Norway THREDDS/OPeNDAP), the data format (FMI offers GRIB2/GRIB1/NetCDF; MET Norway offers NetCDF/NCML), and the distributed subset (FMI: surface level only; MET Norway: full vertical structure, ensemble members, cross-sections). This is analogous to how the UWC-West DINI run is repackaged independently by DMI, KNMI, Met Éireann, and the Icelandic Met Office (see [HARMONIE-AROME Ireland](../ireland/harmonie-arome-ireland.md) and [HARMONIE (DMI)](../denmark/harmonie-dmi.md)).
- **Surface-level subset only.** FMI's documentation states that currently only data near the ground or sea surface (surface level) is provided in gridded form. The full ensemble, model-level, and pressure-level fields are available from the MET Norway distribution.
- **Point time series.** In addition to the gridded product, FMI also serves HARMONIE (MEPS) as point time series via separate stored queries (see FMI's time series data documentation).
- **No FMI HARMONIE on AWS S3.** FMI mirrors only its SILAM atmospheric composition model and radar data to AWS Open Data; HARMONIE (MEPS) is **not** on S3 and must be accessed via the WFS / binary download routes above.
- **No account required.** Unlike the Met Éireann distribution of the sibling UWC-West HARMONIE production (which requires free registration), FMI's open data needs no account — only acceptance of the CC BY 4.0 licence and adherence to the request limits.
- **Companion suites.** MetCoOp also operates two related products built on the MEPS modelling grid: **MNWC** (a deterministic 12-hour nowcasting suite refreshed hourly, partly produced on FMI's Vinha HPC) and **MECaS** (a calibrated ensemble forecast of near-surface temperature, wind, and gusts). These are documented in the [MEPS](../norway/meps.md) entry's notes. FMI does not currently appear to redistribute MNWC or MECaS through this Open Data service.
- **Relationship to siblings:**
  - [MEPS](../norway/meps.md) — the MET Norway distribution of the same MetCoOp production (full distribution; primary reference for model internals).
  - [AROME-Arctic](../norway/arome-arctic.md) — MET Norway's deterministic Arctic-domain HARMONIE-AROME model.
  - [HARMONIE-AROME Ireland](../ireland/harmonie-arome-ireland.md), [HARMONIE (DMI)](../denmark/harmonie-dmi.md), and other ACCORD-consortium HARMONIE-AROME / AROME deployments share the same core model.
- **Repository location:** Like the [MEPS](../norway/meps.md) entry, this documents an ensemble system but is filed under `nwp_models/regional/` for consistency with the existing MEPS entry. The same relocation consideration noted there applies here.

---

## Official documentation
- FMI Open Data — Forecast models manual: https://en.ilmatieteenlaitos.fi/open-data-manual-forecast-models
- FMI Open Data — overview: https://en.ilmatieteenlaitos.fi/open-data
- FMI Open Data — licence (CC BY 4.0): https://en.ilmatieteenlaitos.fi/open-data-licence
- FMI Open Data — accessing data: https://en.ilmatieteenlaitos.fi/open-data-manual-accessing-data
- FMI Open Data — data models: https://en.ilmatieteenlaitos.fi/open-data-manual-data-models
- FMI WFS GetCapabilities: https://opendata.fmi.fi/wfs?request=GetCapabilities
- CC BY 4.0 licence text: https://creativecommons.org/licenses/by/4.0/
- Eresmaa et al. (2026), *The Operational Forecast Process at MetCoOp*, Bull. Amer. Meteor. Soc., 107, E946–E963. https://doi.org/10.1175/BAMS-D-25-0225.1
- See the [MEPS](../norway/meps.md) entry for further HARMONIE-AROME and MetCoOp references.
