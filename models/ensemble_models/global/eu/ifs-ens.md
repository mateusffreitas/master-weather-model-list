# IFS ENS (Integrated Forecasting System – Ensemble)

## What this model is
IFS ENS is ECMWF's flagship global ensemble numerical weather prediction system, providing probabilistic medium-range forecasts and quantifying forecast uncertainty for the 0–15 day range.

It is the ensemble counterpart of the deterministic [IFS](./ifs.md), built on the same model core, the same 9 km native horizontal resolution (since Cycle 48r1 in June 2023), and the same atmospheric initial analysis. The ensemble produces 51 members — 50 perturbed plus 1 control — designed to sample uncertainty in both initial conditions (via singular vectors and an Ensemble of Data Assimilations) and in the model itself (via the Stochastically Perturbed Parametrization scheme).

IFS ENS is widely regarded as one of the highest-skill global ensemble forecast systems in operational use. ECMWF distributes a curated subset of ENS output through the free Open Data programme at 0.25° resolution; the full native 9 km ensemble is also available under CC-BY-4.0 (since 1 October 2025) but typically requires service charges for high-volume delivery.

This entry describes the **medium-range ensemble**. The separate 101-member sub-seasonal (extended-range) ensemble at 36 km running daily out to 46 days is a different operational system — see Notes below.

---

## Who runs it
- **Organization:** European Centre for Medium-Range Weather Forecasts
- **Country / region:** International (European consortium of 23 Member States and 12 Co-operating States)

---

## What area it covers
- **Coverage:** Global

---

## Basic details
- **Model type:** Global ensemble NWP
- **Model system / core:** IFS (hybrid spectral / reduced Gaussian-grid)
- **Dynamical formulation:** Hydrostatic, semi-Lagrangian, semi-implicit time integration
- **Convection-allowing:** No (deep convection is parameterized at 9 km native resolution)
- **Ensemble size:** 51 (1 control + 50 perturbed)
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

## Initial conditions
- **Source:** ECMWF operational analyses produced by the deterministic IFS 4D-Var system (the same analysis used to initialize the deterministic IFS forecast)
- **Initial perturbations:** Two complementary methods are combined:
  - **Singular vectors (SVs):** Perturbations constructed from the leading singular vectors of the linearized model propagator, identifying the directions of fastest error growth over a finite optimization interval. Computed in the extratropics and in selected tropical regions where tropical cyclones are forecast.
  - **Ensemble of Data Assimilations (EDA):** A 50-member ensemble of perturbed 4D-Var analyses at TCo639 outer-loop resolution, providing flow-dependent initial-state perturbations. The EDA also supplies the deterministic IFS with flow-dependent background error covariances.

---

## Perturbations and design
- **Initial condition perturbations:** Combined SV + EDA approach (see above)
- **Model uncertainty:** **Stochastically Perturbed Parametrization (SPP)** scheme. SPP targets uncertain elements within the parametrizations of individual physical processes (currently 27 perturbed elements in IFS Cycle 49r1).
- **Stochastic schemes — historical note:** SPP replaced the long-standing **SPPT** (Stochastically Perturbed Parametrization Tendencies) scheme in IFS Cycle 49r1 (12 November 2024). SPPT had been the operational stochastic physics scheme since 1998, with several revisions over its 25+ years in operations. SPP improves physical consistency of the perturbations and has been shown to particularly help with boundary-layer underdispersion (relevant for 2 m temperature assimilation).
- **Recentering:** Soft re-centring of EDA perturbations is applied alongside the SPP changes from Cycle 49r1 onward.

---

## What it provides
Probabilistic global forecasts including:
- Individual ensemble member forecasts (50 perturbed + 1 control)
- Ensemble mean and ensemble spread (standard deviation)
- Probability products (e.g. probability of exceedance, probability of temperature standardized anomalies at 850 hPa)
- Tropical cyclone track probabilities and strike probabilities (when active TCs are observed or forecast)
- Extreme Forecast Index (EFI) and Shift of Tails (SOT) diagnostics
- Cluster means and cluster representative members
- Calibrated probabilistic guidance for medium-range temperature, wind, precipitation, and pressure

The IFS ENS is also a key reference baseline for evaluating ECMWF's machine-learning ensemble [AIFS ENS](./aifs-ens.md), which runs operationally alongside it.

---

## Data availability

### Free Open Data subset
- **Free?** Yes
- **License:** Creative Commons Attribution 4.0 (CC BY 4.0); attribution required
- **Resolution:** 0.25°
- **Format:** GRIB2 (with CCSDS compression since July 2023)
- **Retention:** Rolling archive of the most recent 12 forecast runs (~2–3 days)
- **Open Data streams:**
  - `stream=enfo, type=ef` — direct model output, all 51 members
  - `stream=enfo, type=cf` — control forecast only (will receive ex-HRES data after Cycle 50r1; see [Upcoming changes](./ifs.md#upcoming-changes) in the IFS entry)
  - `stream=enfo, type=pf` — individual perturbed members (selectable by number)
  - `stream=enfo, type=es` — ensemble spread
  - `stream=enfo, type=ep` — ensemble probability products
  - `stream=enfo, type=tf` — tropical cyclone tracks (BUFR)
- **Official download locations:**
  - https://data.ecmwf.int/forecasts/
  - https://registry.opendata.aws/ecmwf-forecasts/ (AWS mirror)
  - Microsoft Azure and Google Cloud mirrors also available

### Full ECMWF Real-time Catalogue
Since **1 October 2025**, the entire ECMWF Real-time Catalogue is licensed under CC-BY-4.0 — full-resolution 9 km IFS ENS output can be redistributed and used commercially with attribution. *Delivery* of high-volume data may incur service charges to cover infrastructure costs. A 9 km Open Data subset with ~2-hour latency is planned for later in 2026.

### Tooling
The `ecmwf-opendata` Python client provides programmatic access for ENS data including individual member selection:
- https://github.com/ecmwf/ecmwf-opendata

---

## Notes
- IFS ENS is the ensemble companion to the deterministic [IFS](./ifs.md). The two share the same model core, the same 9 km native resolution (since Cycle 48r1, June 2023), and the same atmospheric analysis.
- The deterministic IFS (historically "HRES") and the ENS Control member became scientifically and computationally bit-identical in IFS Cycle 49r1 (12 November 2024). With Cycle 50r1 (12 May 2026), the separate HRES is discontinued — the ex-HRES data stream becomes the ENS Control. See the [IFS entry](./ifs.md#upcoming-changes) for migration details.
- IFS ENS is run as a coupled system with ocean, sea ice, and wave components (the ENS-WAM ensemble runs on the same Tco1279 grid as the atmosphere since Cycle 49r1). The atmospheric component is the focus of this entry; the wave ensemble is a separate operational stream (`stream=waef`).
- The freely-distributed Open Data is a **curated subset** of the operational ENS at reduced resolution. The full native 9 km ensemble and the complete parameter list require ECMWF's commercial/operational data services or the planned 9 km Open Data subset coming later in 2026.
- The current IFS cycle is **49r1** (operational since 12 November 2024).

### Sub-seasonal (extended-range) ensemble — separate system
ECMWF also runs a separate **101-member sub-seasonal ensemble** at 36 km (TCo319) horizontal resolution out to 46 days, run daily at 00 UTC. This was restructured in IFS Cycle 48r1 (June 2023) from a 51-member system running twice weekly as an extension of the medium-range ensemble. It is now a fully independent forecast system rather than a continuation of the medium-range run, with its own re-forecast configuration. It is open-licensed (since July 2024 for data ≥0.4° resolution) but is not part of the medium-range ENS described above.

---

## Upcoming changes

### IFS Cycle 50r1 — operational 12 May 2026
ENS is part of the IFS Cycle 50r1 upgrade, deployed jointly with [AIFS Single v2](./aifs-single.md) and [AIFS ENS v2](./aifs-ens.md). See the [IFS entry](./ifs.md#upcoming-changes) for the full atmospheric model and data-assimilation upgrade list. Key changes specific to ENS users:

- **No change in horizontal resolution, vertical resolution, or forecast steps.**
- **Modified SPP configuration** to address excessive 10 m wind spread in the ensemble — the previous SPP scheme produced over-dispersion in near-surface wind extremes; the revised configuration yields more realistic ensemble spread.
- **Stream/archive change:** the separate HRES forecast is retired. The ex-HRES data stream becomes the ENS Control (`stream=oper, type=fc` → `stream=enfo, type=cf`; `stream=scda, type=fc` for 06/18 UTC). Users currently consuming `stream=enfo, type=cf` should migrate to `stream=oper, type=fc` ahead of the cycle implementation.
- **Tropical cyclone ensemble products** drop from 52 to 51 members (the redundant ex-HRES member is removed from the TC ensemble).
- Coupled atmosphere–ocean assimilation, weak-constraint 4D-Var extended to the boundary layer, and revised convection/microphysics improvements all apply to the analyses used to initialize ENS.

### IFS Cycle 50r2 — full GRIB2 transition (tentative Q4 2026)
Cycle 50r2 will complete ECMWF's migration to a GRIB2-only representation for all parameters, affecting Open Data distribution of ENS products. Key points:
- All Open Data parameters move from mixed GRIB1/GRIB2 to GRIB2 only.
- Legacy GRIB1-style parameter references move to GRIB2-native parameter identifiers.
- GRIB2 output uses CCSDS compression, requiring decoder support (for example, `libaec`).
- See the [IFS entry](./ifs.md#upcoming-changes) and ECMWF's migration documentation for details and test resources.

---

## Recent version history

### Cycle 49r1 — operational 12 November 2024
- ENS Control made bit-identical to the deterministic HRES (preparing for the HRES retirement in Cycle 50r1)
- ENS Control extended to 15 days at 00/12 UTC, matching the perturbed members
- **SPPT replaced by SPP** for representing model uncertainty — first major change to ECMWF's stochastic physics scheme since SPPT was introduced in 1998
- SPP also introduced into the EDA (further improving the consistency of analysis and forecast perturbations)
- Singular vector amplitude reduced (the combination of higher-resolution EDA, soft re-centring, and SPP allowed the SV contribution to be scaled back, partly addressing winter-storm-track over-dispersion)

### Cycle 48r1 — operational 27 June 2023
- ENS horizontal resolution increased from TCo639 (~18 km) to **TCo1279 (~9 km)**, matching the deterministic HRES
- Substantial improvements in surface variables (2 m temperature, 10 m wind) and tropical cyclone forecasting
- Extended-range ensemble restructured into a separate 101-member system at 36 km running daily out to 46 days (previously a 51-member extension of the medium-range run)
- GRIB2 output began using CCSDS compression

### Earlier history
ECMWF's ensemble started operational forecast production on **23 November 1992** as one of the first operational ensemble prediction systems in the world. The original design used singular vectors alone; perturbations from the EDA were added in 2010, and SPPT (the predecessor of today's SPP) was introduced in 1998.

---

## Official documentation
- Open Data dataset page: https://www.ecmwf.int/en/forecasts/datasets/open-data
- ECMWF ENS overview (Forecast User Guide, Section 5): https://confluence.ecmwf.int/display/FUG/Section+5+Forecast+Ensemble+(ENS)+-+Rationale+and+Construction
- IFS Documentation Cy49r1, Part V (Ensemble Prediction System): https://www.ecmwf.int/sites/default/files/elibrary/112024/81627-ifs-documentation-cy49r1-part-v-ensemble-prediction-system.pdf
- Changes to the forecasting system: https://confluence.ecmwf.int/display/FCST/Changes+to+the+forecasting+system
- IFS Cycle 50r1 implementation page: https://confluence.ecmwf.int/display/FCST/Implementation+of+IFS+Cycle+50r1
- ECMWF Newsletter 181 (SPP introduction in Cycle 49r1): https://www.ecmwf.int/en/newsletter/181/earth-system-science/improving-physical-consistency-ensemble-forecasts-using-spp
- ECMWF Newsletter 176 (Cycle 48r1 ENS resolution upgrade): https://www.ecmwf.int/en/newsletter/176/earth-system-science/ifs-upgrade-brings-many-improvements-and-unifies-medium
- ECMWF Open Data Python client: https://github.com/ecmwf/ecmwf-opendata
- ECMWF Open Data community forum: https://forum.ecmwf.int/c/open-data
