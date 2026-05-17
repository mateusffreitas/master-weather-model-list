# IFS (Integrated Forecasting System)

## What this model is
The Integrated Forecasting System (IFS) is ECMWF's flagship global numerical weather prediction system and the deterministic backbone of European medium-range forecasting.

IFS uses a hybrid spectral / reduced Gaussian-grid dynamical core and a 4D-Var data assimilation system, and is widely regarded as one of the most skillful operational global NWP systems in the world. It provides the analyses used to initialize ECMWF's other forecast products including IFS ENS (the physics ensemble), AIFS Single (the deterministic AI model), and AIFS ENS (the ensemble AI model).

ECMWF distributes IFS output through two channels: a free Open Data subset (0.25°, CC-BY-4.0) and the full Real-time Catalogue (native 9 km, also under CC-BY-4.0 since 1 October 2025 but subject to service charges for high-volume delivery). This entry covers the IFS as a whole, with the Open Data details called out explicitly.

---

## Who runs it
- **Organization:** European Centre for Medium-Range Weather Forecasts
- **Country / region:** International (European consortium of 23 Member States and 12 Co-operating States)

---

## What area it covers
- **Coverage:** Global

---

## Basic details
- **Model type:** Global deterministic NWP
- **Model system / core:** IFS (hybrid spectral / reduced Gaussian-grid)
- **Dynamical formulation:** Hydrostatic, semi-Lagrangian, semi-implicit time integration
- **Convection-allowing:** No (deep convection is parameterized at 9 km native resolution)
- **Native horizontal resolution:** ~9 km (Tco1279 octahedral reduced Gaussian grid)
- **Open Data resolution:** 0.25° (~28 km) regular latitude–longitude
- **Vertical levels:** 137
- **Model top:** 0.01 hPa (~80 km)
- **Forecast length:**
  - 15 days (360 h) for 00 and 12 UTC cycles
  - 6 days (144 h) for 06 and 18 UTC cycles
- **Update frequency:** 4× daily (00, 06, 12, 18 UTC)
- **Temporal output resolution (Open Data):**
  - 00 / 12 UTC: 3-hourly to +144 h, 6-hourly from +150 h to +360 h
  - 06 / 18 UTC: 3-hourly to +144 h

---

## Data assimilation
- **Method:** 12-hour incremental 4D-Var with weak-constraint formulation
- **Background error covariances:** Flow-dependent, derived from an Ensemble of Data Assimilations (EDA)
- **Assimilated observations:** Conventional and satellite observations including radiances, GPS-RO, atmospheric motion vectors, scatterometer winds, and a wide range of additional sources

---

## What it provides
Deterministic global forecasts of:
- Temperature, wind, humidity, geopotential, and pressure on standard pressure levels
- Surface and near-surface fields (2 m temperature, 10 m wind, mean sea level pressure, etc.)
- Total precipitation, precipitation type, and convective indices
- Cloud and hydrometeor fields
- Heat and cold indices, mean radiant temperature, and globe temperature (added in Cycle 49r1)

The IFS analysis is also used to initialize ECMWF's [AIFS Single](./aifs-single.md), [AIFS ENS](../../../ensemble_models/global/eu/aifs-ens.md), [IFS ENS](../../../ensemble_models/global/eu/ifs-ens.md), and several downstream limited-area systems run by ECMWF Member States (including AROME, ALADIN, and HARMONIE-AROME family configurations).

---

## Relationship to other models
- **[IFS ENS](../../../ensemble_models/global/eu/ifs-ens.md):** The 51-member ensemble counterpart to IFS, built on the same model core and analyses. As of Cycle 49r1, IFS ENS runs at the same 9 km resolution as the deterministic IFS.
- **[AIFS Single](./aifs-single.md):** ECMWF's machine-learning deterministic model. Shares IFS analyses for initialization.
- **[AIFS ENS](../../../ensemble_models/global/eu/aifs-ens.md):** ECMWF's machine-learning ensemble. Also initialized from IFS analyses.

The deterministic IFS forecast (historically known as "HRES") and the ENS Control member became scientifically and computationally bit-identical with IFS Cycle 49r1 (12 November 2024). With Cycle 50r1 (12 May 2026), the separate HRES stopped being produced — the data stream formerly known as HRES is now the ENS Control. See [Recent version history](#recent-version-history) below for migration details.

---

## Data availability

### Free Open Data subset
- **Free?** Yes
- **License:** Creative Commons Attribution 4.0 (CC BY 4.0); attribution required
- **Resolution:** 0.25°
- **Format:** GRIB2 (with CCSDS compression since July 2023)
- **Retention:** Rolling archive of the most recent 12 forecast runs (~2–3 days)
- **Official download locations:**
  - https://data.ecmwf.int/forecasts/
  - https://registry.opendata.aws/ecmwf-forecasts/ (AWS mirror)
  - Microsoft Azure and Google Cloud mirrors also available

The free Open Data subset is a curated parameter list rather than the full IFS output. It expanded from 0.4° to 0.25° in March 2024 and is planned to expand again to include a 9 km subset (with ~2-hour latency) later in 2026.

### Full ECMWF Real-time Catalogue
Since **1 October 2025**, the entire ECMWF Real-time Catalogue is licensed under CC-BY-4.0 — meaning data costs have been removed and full-resolution 9 km IFS output can be redistributed and used commercially with attribution. However, *delivery* of high-volume data may still incur service charges to cover infrastructure costs. Full-catalogue access typically requires a Real-time Dissemination Service Agreement.

### Tooling
The `ecmwf-opendata` Python client provides a consistent interface for retrieving Open Data from the portal, AWS, Azure, or GCP, and is the recommended programmatic access path:
- https://github.com/ecmwf/ecmwf-opendata

---

## Notes
- The operational IFS runs on a roughly annual major cycle upgrade schedule. Each cycle can shift skill characteristics, biases, and variable distributions — users running long backtests or climatologies should split evaluation windows around cycle transition dates.
- The freely-distributed Open Data is a **curated subset** of the operational IFS at reduced resolution. For the full-resolution native 9 km output and the complete parameter list, users should consult ECMWF's commercial/operational data services or wait for the 9 km Open Data subset planned for later in 2026.
- IFS is run as a coupled system with ocean, sea ice, and wave components. The atmospheric component is the focus of this entry; see ECMWF's documentation for details on the wave model (WAM) and ocean/sea-ice coupling, both of which were upgraded materially in Cycle 50r1.
- Heat and cold indices, mean radiant temperature, and globe temperature were added as standard IFS output parameters in Cycle 49r1 (November 2024).
- The current IFS cycle is **50r1** (operational since 12 May 2026).

---

## Upcoming changes

### IFS Cycle 50r2 — full GRIB2 transition (tentative Q4 2026)
Cycle 50r2 will complete ECMWF's migration to a GRIB2-only representation for all parameters. This affects Open Data users directly.

**Key points:**
- All Open Data parameters move from mixed GRIB1/GRIB2 to GRIB2 only.
- Legacy GRIB1-style parameter references (e.g., `165.128`) move to GRIB2-native parameter identifiers.
- Some level-type handling changes.
- GRIB2 output uses CCSDS compression, requiring decoder support (for example, `libaec`).
- Users with custom extraction, parameter-matching, or decoder logic should validate against ECMWF's test datasets prior to cutover.

ECMWF migration resources:
- Static test dataset: available since September 2025
- Migration documentation: https://confluence.ecmwf.int/display/MTG2US/Migration+to+GRIB+edition+2+Information+page
- GRIB2 encoding changes: https://confluence.ecmwf.int/display/MTG2US/Migration+to+GRIB2+-+changes+to+encoding+of+parameters

---

## Recent version history

### Cycle 50r1 — operational 12 May 2026 (current)
A major upgrade affecting the atmospheric, wave, and ocean/sea-ice components, deployed jointly with AIFS Single v2 and AIFS ENS v2 on the same day. The Release Candidate Phase ran from 19 February 2026 through the go-live; test data was available from MARS with experiment version (expver) 0080.

**Key atmospheric changes:**
- **No change in horizontal resolution, vertical resolution, or forecast steps.**
- Coupled data assimilation became more central: outer-loop coupling between atmosphere and ocean, with a 12-hour ocean analysis running alongside the atmospheric analysis. Microwave imagers and geostationary infrared data now contribute to coupled atmosphere/ocean increments.
- Weak-constraint 4D-Var formulation extended to the boundary layer, allowing assimilation of 2 m temperature observations across the full 12-hour window (vs the first 6 hours previously).
- Convection and cloud-microphysics changes addressing stationary precipitation and improving how precipitation propagates inland from the coast.
- New glacier parametrisation in the ecLand component (four-layer land-ice scheme replacing the previous binary glacier mask).
- Improved QBO, stratospheric winds, and stratospheric humidity (radiosonde humidity assimilation reintroduced up to 60 hPa).
- Solar eclipse effects now represented via accurate astronomical computations.
- Single-precision computation in the coupled atmosphere–ocean trajectory and reduced-resolution EDA inner loop yields ~40% computational savings.
- Underlying ocean/sea-ice core moved from NEMO 3.4 + LIM2 to NEMO4-SI3, with fully active coupling. (Mentioned for context — see ECMWF's documentation for details.)

**Stream/archive handling changes (primarily affects MARS and dissemination users):**
- The separate HRES forecast is no longer produced. The data stream formerly known as HRES is now the **ENS Control** (`stream=oper, type=fc` → `stream=enfo, type=cf`; `stream=scda, type=fc` for 06/18 UTC).
- Users who consumed `stream=enfo, type=cf` should have migrated to `stream=oper, type=fc` (and `stream=scda, type=fc` for 06/18 UTC) ahead of cycle implementation.
- Vegetation fraction difference (`vegdiff`) is discontinued and replaced by Urban cover.

Open Data users were not directly affected by the stream/archive changes but should expect to see modest forecast differences from the model upgrade itself.

### Cycle 49r1 — operational 12 November 2024
- HRES and ENS Control made scientifically and computationally bit-identical
- HRES extended from 10 to 15 days at 00/12 UTC, and runs to 6 days at 06/18 UTC
- New land surface model upgrades including urban tiles
- Heat indices, mean radiant temperature, and globe temperature added as output parameters
- Multi-layer snow scheme integrated into reforecasts via offline land DA reanalysis

### Cycle 48r1 — operational 27 June 2023
- ENS horizontal resolution increased from 18 km to 9 km, matching HRES
- Extended-range ensemble (now "sub-seasonal") restructured: separate system running daily out to 46 days at 36 km with 101 members
- GRIB2 output began using CCSDS compression

### Cycle 41r2 — operational 8 March 2016
- HRES grid resolution upgraded to 9 km on the new octahedral reduced Gaussian grid (Tco1279); ENS to 18 km

---

## Official documentation
- Open Data dataset page: https://www.ecmwf.int/en/forecasts/datasets/open-data
- Changes to the forecasting system: https://confluence.ecmwf.int/display/FCST/Changes+to+the+forecasting+system
- Planned model changes: https://www.ecmwf.int/en/forecasts/documentation-and-support/changes-ecmwf-model
- IFS Cycle 50r1 implementation page: https://confluence.ecmwf.int/display/FCST/Implementation+of+IFS+Cycle+50r1
- IFS Cycle 49r1 implementation page: https://confluence.ecmwf.int/display/FCST/Implementation+of+IFS+Cycle+49r1
- ECMWF Newsletter 185 (Cycle 50r1 overview): https://www.ecmwf.int/en/newsletter/185/earth-system-science/upgrade-ifs-cycle-50r1
- ECMWF Open Data Python client: https://github.com/ecmwf/ecmwf-opendata
- ECMWF Open Data community forum: https://forum.ecmwf.int/c/open-data
