# BRAMS (CPTEC/INPE Regional Atmosphere–Chemistry Model)

## What this model is
BRAMS (Brazilian developments on the Regional Atmospheric Modeling System) is a regional model operated by CPTEC/INPE that serves a dual role: numerical weather prediction and air-quality / atmospheric-composition forecasting over South America. Its composition capability comes from the coupled aerosol-and-tracer-transport configuration (CATT-BRAMS), which forecasts biomass-burning smoke and trace gases (notably CO and particulate matter) online within the meteorological model. It is publicly distributed in two domains (~8 km and ~15 km) and three output streams.

---

## Who runs it
- **Organization:** CPTEC/INPE (Center for Weather Forecasting and Climate Studies / National Institute for Space Research)
- **Country / region:** Brazil

---

## What area it covers
- **Coverage:** South America
- **Domain details:** Two continental South America domains — `ams_08km` (~8 km) and `ams_15km` (~15 km). Exact grid bounds TBD.

---

## Public configurations and output streams

| Stream | Domain | Res. | Content | Forecast length | Output step | Format | Filename pattern |
|---|---|---|---|---|---|---|---|
| `ams_08km` | South America | ~8 km | Meteorology | −3 h to +180 h (7.5 days) | Hourly | **GRIB2** | `BRAMS_ams_08km_<init>_<valid>.grib2` |
| `ams_15km/previsao` | South America | ~15 km | Meteorology | +0 h to +72 h (3 days) | 3-hourly | GrADS (`.gra`+`.ctl`) | `profile_<init>-A-<valid>-g1.gra` |
| `ams_15km/gases` | South America | ~15 km | Chemistry / composition | +0 h to +24 h (1 day) | 3-hourly | GrADS (`.gra`+`.ctl`) | `profile_<init>G-A-<valid>-g1.gra` |

(`<init>` = `YYYYMMDDHH`; `<valid>` in the GrADS streams is `YYYY-MM-DD-HHMMSS`. The `G` in the gases filenames marks the chemistry stream; `-g1` denotes BRAMS grid 1. All three streams run once daily at 00 UTC.)

---

## Basic details
- **Model type:** Regional coupled meteorology–chemistry (NWP + air quality)
- **Model system / core:** BRAMS (RAMS-derived regional model); chemistry via CATT-BRAMS (Coupled Aerosol and Tracer Transport)
- **Horizontal resolution:** ~8 km (`ams_08km`) and ~15 km (`ams_15km`)
- **Vertical levels:** TBD
- **Model top:** TBD
- **Forecast length:** +180 h (`ams_08km`); +72 h (`ams_15km/previsao`); +24 h (`ams_15km/gases`)
- **Update frequency / cycles:** 1× daily (00 UTC) — all streams
- **Temporal output resolution:** Hourly (`ams_08km`); 3-hourly (`ams_15km` previsao and gases)

---

## Meteorological driver
- **Driving NWP model:** BRAMS is itself the meteorological model (composition is computed inside it, not driven by a separate met model).
- **Coupling:** Online — CATT-BRAMS transports tracers/aerosols using the same dynamical core that produces the meteorology.
- **Initial / boundary conditions:** TBD — the regional BRAMS is nested in a global driver (plausibly CPTEC's BAM or GFS), unconfirmed for the public feed.

---

## Chemistry and aerosols
- **Configuration:** CATT-BRAMS (Coupled Aerosol and Tracer Transport coupled to BRAMS), per the published model design (Freitas et al., 2009; Longo et al., 2013).
- **Primary focus:** Biomass-burning emissions and transport over South America — carbon monoxide (CO) and particulate matter / smoke tracers.
- **Gas-phase chemical mechanism:** TBD (current operational configuration not documented in the public feed).
- **Number of chemical species:** TBD.
- **Aerosol treatment / components:** TBD; particulate matter from biomass burning is among the outputs.

---

## Emissions
- **Wildfire / biomass-burning emissions:** Per the CATT-BRAMS design, derived from satellite fire detection via the Brazilian Biomass Burning Emission Model (3BEM) / PREP-CHEM-SRC. Current operational inventory details are not documented in the public feed (TBD).
- **Anthropogenic / biogenic inventories:** TBD.

---

## Data assimilation (optional)
- **Assimilates AQ observations:** TBD.

---

## What it provides
- **Meteorology** (`ams_08km` GRIB2; `ams_15km/previsao` GrADS): temperature, wind, humidity, pressure, precipitation, and 3D atmospheric fields.
- **Composition** (`ams_15km/gases` GrADS): trace gases and aerosol/smoke tracers from biomass burning (CO and particulate matter among them). Exact distributed species list is encoded in the GrADS `.ctl` descriptors (TBD to enumerate).

---

## Data availability
- **Is the data free?** Yes — free of charge, no registration.
- **License:** **Transitional / not yet an open license.** Data is freely accessible and usable personally today, but CPTEC/INPE's operational-server notice restricts commercial use and redistribution in published/dissemination outlets without express CPTEC/INPE authorization, and requires attribution to "CPTEC/INPE." INPE has committed under its Open Data Plan (PDA 2025–2027, Decreto 8.777/2016) to republish BRAMS as open data on dados.gov.br, scheduled ~June 2026. As of late June 2026 it is not yet live on the dados.gov.br "Tempo e Clima" category; open reuse terms apply once it appears there. Note the PDA entry references the ~8 km BRAMS specifically; the open-data status of the `ams_15km` GrADS streams is less clear.
- **Is the data downloadable?** Yes
- **Data formats:** GRIB2 (`ams_08km`) and GrADS binary (`.gra` data + `.ctl` descriptor) for `ams_15km`. The GrADS streams require GrADS-compatible tooling (e.g., GrADS/OpenGrADS, or the `.ctl` read via xgrads/pygrads) rather than a standard GRIB reader; individual `.gra` timesteps are large (~0.8 GB).
- **Official download location:**
  https://dataserver.cptec.inpe.br/dataserver_modelos/brams/
  - `ams_08km/brutos/<YYYY>/<MM>/<DD>/00/`
  - `ams_15km/brutos/previsao/<YYYY>/<MM>/<DD>/00/`
  - `ams_15km/brutos/gases/<YYYY>/<MM>/<DD>/00/`

---

## Notes
- **Format split & tooling:** Only `ams_08km` is GRIB2 and broadly accessible; `ams_15km` is GrADS binary. The `.gra`/`.ctl` pair is still structured gridded data but is less universal — flag for users expecting GRIB/NetCDF.
- **`ams_08km` output offset:** Hourly output is provided from −3 h (3 h before the 00 UTC init) out to +180 h.
- **Stream split within `ams_15km`:** `previsao` is meteorology (+72 h); `gases` is the chemical/composition stream (+24 h), marked by the `G` in the filename.
- **Resolution conflict:** The gov.br PNT service page lists "BRAMS Ambiental 20 km," which conflicts with the dataserver's ~8 km and ~15 km domains; the dataserver paths/filenames are treated as authoritative here.
- **Dual role:** BRAMS is also one of CPTEC's regional NWP systems; the `ams_08km` GRIB2 stream is essentially meteorological. This entry is filed under air quality on the basis of the CATT-BRAMS composition capability — cross-reference from the NWP side if useful.
- **Relationship to siblings:** Contributes to CPTEC's SMEC multi-model ensemble (Eta + WRF + BRAMS); run alongside the [Eta](../../../nwp_models/regional/brazil/eta-cptec.md) and [WRF](../../../nwp_models/regional/brazil/wrf-cptec.md) regional systems and the global [BAM](../../../nwp_models/global/brazil/bam-cptec.md).
- **Stale server inventories:** Plain-text directory dumps (e.g., `previsao.txt`, `gases.txt`, `lista_*.txt`) left in the `ams_15km` tree reflect a 2021–2022 archive snapshot, not current availability; current runs are present and daily (with occasional missing days).
- **Older data:** Only recent data is served operationally; older data requires a request to CPTEC and is subject to availability.

---

## Official documentation
- CPTEC/INPE model data server — https://dataserver.cptec.inpe.br/dataserver_modelos/brams/
- CPTEC/INPE — https://www.cptec.inpe.br/
- gov.br service page (PNT) — https://www.gov.br/pt-br/servicos/obter-dados-provenientes-de-modelos-numericos-de-previsao-de-tempo-inpe-pnt
