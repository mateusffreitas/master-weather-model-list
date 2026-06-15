# CFSv2 (Climate Forecast System version 2)

## What this model is
CFSv2 is NCEP's operational global long-range forecasting system — a fully coupled atmosphere–ocean–land–sea-ice model that runs four coupled forecasts per day out to 9 months. Operational since March 2011, it underpins NWS seasonal guidance and contributes to multi-model seasonal ensembles. The distributed data is raw coupled model output (6-hourly fields, time series, and monthly means); anomaly and probability products are computed downstream rather than distributed here.

Note that CFSv2 is an *operational seasonal prediction* system, not a climate-projection model: it issues initialized, calendar-dated forecasts on a continuous operational cycle.

---

## Who runs it
- **Organization:** NOAA / NWS / NCEP — Environmental Modeling Center (EMC); disseminated via NOMADS and NOAA Open Data Dissemination (NODD), archived at NCEI
- **Country / region:** United States
- **Coordinating body / programme:** Standalone NCEP system; contributes as a member to the North American Multi-Model Ensemble (NMME) and provides input to NWS/CPC operational seasonal outlooks

---

## What area it covers
- **Coverage:** Global
- **Domain details:** Native spectral atmosphere at T126 (~0.937°, ~100 km); products distributed on regular lat-lon grids down to 0.5° (~56 km)

---

## Basic details
- **Model type:** Long-range coupled forecast system — time-lagged ensemble of deterministic coupled runs; single model
- **Model system / core:** CFSv2 — a GFS-based atmosphere coupled to the GFDL MOM4 ocean, with interactive sea ice and the Noah land-surface model
- **Range class:** Sub-seasonal to seasonal (out to 9 months)
- **Forecast length:** Control runs to 9 months; additional runs to one season and to 45 days
- **Initialization cadence:** Four cycles per day (00/06/12/18 UTC)
- **Ensemble generation:** Time-lagged — the four daily coupled runs are accumulated over successive cycles to build a seasonal ensemble (CPC commonly forms ~40 members from the most recent runs)
- **Ensemble size:** Per the NCEP operational configuration, 16 CFS runs per day — four control runs to 9 months (one per cycle), three perturbed runs to one season (00 UTC), and nine perturbed runs to 45 days (three each at 06/12/18 UTC)
- **Temporal output resolution:** Hourly, 6-hourly, and monthly means
- **Output aggregation levels:** 6-hourly forecast fields, time series of selected variables, and monthly means (raw output; anomalies and tercile probabilities are derived downstream against the reforecast climatology)

---

## Coupled configuration
- **Atmosphere:** GFS-based spectral model, T126 (~100 km), 64 hybrid sigma-pressure levels
- **Ocean:** GFDL MOM4, ~0.25° meridionally in the tropics relaxing to 0.5°, 40 vertical levels
- **Sea ice:** Interactive sea-ice model within the coupled framework
- **Land surface:** Noah land-surface model (4 soil layers)
- **Coupling notes:** Fully coupled with no flux correction; ocean–atmosphere coupled at the model time step

---

## Initialization
- **Atmosphere IC:** CFSv2 real-time coupled data assimilation system (the CDAS / GSI-based atmospheric analysis)
- **Ocean IC:** Global Ocean Data Assimilation System (GODAS)
- **Sea ice / land IC:** From the coupled CFSv2 analysis cycle
- **Perturbation method:** Lagged initialization across cycles, plus perturbed members in the season- and 45-day runs

---

## Hindcasts (reforecasts)
- **Hindcast period:** Retrospective forecasts spanning approximately 1982–2010, used to define the model climatology and to calibrate real-time anomaly and probability products. *(Flag: confirm the exact span and start-date cadence against Saha et al., 2014.)*
- **Hindcast cadence / ensemble size:** 9-month reforecasts initialized at a reduced cadence (every few days), with more frequent shorter reforecasts.
- **Reference climatology period:** Derived from the reforecast set (see flag above).
- **Distributed alongside forecasts?** Separately — the reforecast/retrospective dataset is archived at NCEI rather than in the NOMADS rolling production directory.

---

## Sources of predictability
Skill derives chiefly from the coupled ocean–atmosphere state (ENSO is the dominant seasonal driver), with the MJO contributing on sub-seasonal scales and land/soil-moisture memory adding regional signal.

---

## What it provides
- Raw coupled forecast fields across the standard CFS output streams — atmospheric pressure-level (`pgbf`), surface flux (`flxf`), and ocean (`ocnf`)
- 6-hourly forecast fields, time series of selected variables, and monthly means
- All in GRIB2

Pre-computed tercile/anomaly probability grids are **not** distributed through these channels — those exist only as CPC graphical outlook products (out of scope).

---

## Data availability
- **Is the data free?** Yes
- **License:** Public domain (U.S. government work; CC0-equivalent). NOAA/NODD requests attribution and prohibits implying NOAA endorsement.
- **Is the data downloadable?** Yes
- **Data formats:** GRIB2
- **Official download location:**  
  - NOMADS (rolling ~7-day production archive): https://nomads.ncep.noaa.gov/pub/data/nccf/com/cfs/prod/ — `cfs.YYYYMMDD/` holds the **forecast** (in scope); `cdas.YYYYMMDD/` holds the real-time **analysis** (out of scope); `monthly/` holds monthly means
  - AWS Open Data (CFSv2 operational forecasts): `s3://noaa-cfs-pds` (region `us-east-1`, no AWS account required: `aws s3 ls --no-sign-request s3://noaa-cfs-pds/`); browsable at https://noaa-cfs-pds.s3.amazonaws.com/index.html
  - Long-term archive: NOAA NCEI
- **Access route notes:** NOMADS retains only roughly the last week; the full archive lives at NCEI. The AWS bucket mirrors the operational forecasts (not the analysis) and offers SNS new-object notifications (`NewCFSObject`).

---

## Notes
- **Scope boundary.** Only the `cfs.*` forecast is in scope. The co-located `cdas.*` real-time analysis (CDAS) and the separate CFS Reanalysis (CFSR) are analyses — excluded under the catalog's no-analysis rule.
- **Single-model system.** Unlike CanSIPS or NMME, CFSv2 is one coupled model; its ensemble comes from time-lagging and perturbations. It is itself a contributing member of NMME.
- **Raw output only.** Anomalies and probabilities are user- or CPC-derived against the reforecast climatology; the CPC seasonal outlook maps are viewer-only and out of scope.
- **Operational since March 2011**, succeeding CFSv1.

---

## Recent version history
CFSv2 became operational at NCEP in March 2011, replacing CFSv1. No subsequent major operational version has superseded it.

---

## Official documentation
- CFS home page: https://cfs.ncep.noaa.gov/
- AWS Open Data registry: https://registry.opendata.aws/noaa-cfs/
- NCEI CFSv2 dataset page: https://www.ncdc.noaa.gov/data-access/model-data/model-datasets/climate-forecast-system-version2-cfsv2
- Saha et al. (2014), *The NCEP Climate Forecast System Version 2*, J. Climate: https://journals.ametsoc.org/view/journals/clim/27/6/jcli-d-12-00823.1.xml
- Saha et al. (2010), *The NCEP Climate Forecast System Reanalysis*, BAMS: https://journals.ametsoc.org/view/journals/bams/91/8/2010bams3001_1.xml
