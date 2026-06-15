# CanSIPS (Canadian Seasonal to Inter-annual Prediction System)

## What this model is
CanSIPS is Environment and Climate Change Canada's operational global long-range forecasting system, producing probabilistic seasonal-to-interannual predictions out to 12 months. It is a multi-model ensemble (MME) built from two fully coupled atmosphere–ocean–sea-ice–land models developed in-house — CanESM5 and GEM5.2-NEMO — whose members are pooled into a single 40-member ensemble. Output is both deterministic (ensemble mean) and probabilistic (tercile and threshold-exceedance categories). The current operational version is **CanSIPS v3.0.0**, in operations since 11 June 2024.

Note that CanSIPS is an *operational seasonal prediction* system, not a climate-projection model: it issues initialized, calendar-dated forecasts on a monthly cycle.

---

## Who runs it
- **Organization:** Environment and Climate Change Canada (ECCC) — models developed by the Canadian Centre for Climate Modelling and Analysis (CCCma) and the Meteorological Research Division (MRD); run operationally by the Canadian Centre for Meteorological and Environmental Prediction (CCMEP) at the Canadian Meteorological Centre (CMC)
- **Country / region:** Canada
- **Coordinating body / programme:** Standalone ECCC system; also contributes as a member to the North American Multi-Model Ensemble (NMME) and the Copernicus C3S seasonal multi-system

---

## What area it covers
- **Coverage:** Global
- **Domain details:** Distributed on a 1° regular lat-lon grid (ni = 360, nj = 180; first grid point at 89.5° S, 0.5° E). A coarser ~250 km grid is additionally offered through the GeoMet OGC-API.

---

## Basic details
- **Model type:** Long-range coupled forecast system — ensemble (deterministic + probabilistic); multi-model (two-model in-house MME)
- **Model system / core:** CanESM5 (CanESM5.1-p1) and GEM5.2-NEMO (GEM v5.2 coupled to NEMO 3.6)
- **Range class:** Seasonal to interannual
- **Forecast length:** 12 months
- **Initialization cadence:** Monthly, at month-end (last day of each month, plus a lagged start; see Ensemble generation)
- **Ensemble generation:** Burst + time-lagged — for each model, 10 members initialized on the last day of the month and 10 from a lagged start 4 days earlier
- **Ensemble size:** 40 (20 per model: members 1–10 official-day + 11–20 lagged, per model)
- **Temporal output resolution:** Monthly means (12 monthly records per member)
- **Output aggregation levels:** Monthly and seasonal (running 3-month) means; anomalies relative to the hindcast climatology; tercile and exceedance probabilities

---

## Coupled configuration

**CanESM5 component**
- **Atmosphere:** Spectral dynamical core, T63 truncation (~2.8°), 49 hybrid sigma-pressure levels, model top at 1 hPa
- **Ocean:** CanNEMO (based on NEMO 3.4.1), ORCA1 C-grid 1° × 1°, 45 levels; includes the Canadian Model of Ocean Carbon
- **Sea ice:** LIM2 (Louvain-la-Neuve)
- **Land surface:** CLASS 3.6.2 + CTEM (interactive/dynamic vegetation and terrestrial carbon)
- **Coupling notes:** Coupler CanCPL (ESMF conservative regridding); run-time atmosphere/ocean bias correction applied (Kharin & Scinocca, 2012)

**GEM5.2-NEMO component**
- **Atmosphere:** GEM v5.2, hydrostatic primitive equations, Yin-Yang grid ~1° × 1°, 85 levels, model lid at 0.1 hPa
- **Ocean:** NEMO 3.6, global 1° × 1° (1/3° meridionally near the equator), 50 levels
- **Sea ice:** CICE 6.0 (Delta-Eddington shortwave)
- **Coupling notes:** Coupled every ocean time-step (30 min) via the in-house GOSSIP communicator; model uncertainty sampled with the Stochastic Parameter Perturbation (SPP) scheme

---

## Initialization
Operational (real-time) initial conditions are drawn from ECCC's operational analyses:
- **CanESM5:** Coupled atmosphere–ocean–sea-ice assimilation nudged toward the GDPS atmospheric analysis and the GIOPS ocean analysis; Northern Hemisphere sea-ice thickness constrained near the SMv3 statistical model; land initialized via the CLASS/CTEM response to the assimilating atmosphere.
- **GEM5.2-NEMO:** Atmospheric ICs from the Global Ensemble Prediction System (GEPS); ocean and sea-ice ICs from the GIOPS analysis; land-surface ICs from the GDPS analysis, adjusted to the hindcast climatology.
- **Perturbation method:** Lagged starts (above) plus, for GEM5.2-NEMO, SPP-based model-uncertainty perturbation.

The hindcasts use a *different* initialization chain (ERA5 + ORAS5 reanalyses) — see below.

---

## Hindcasts (reforecasts)
- **Hindcast period:** 1980–2020 (41 years), monthly start dates, 20 members per model (10 official-day + 10 lagged), 12-month integrations.
- **Reference climatology period:** 1991–2020 (30 years) used to generate the official anomaly and probability products, per the datamart documentation. *(Flag: the fact-sheet technical note instead states 1990–2020 / 31 years — minor source inconsistency to confirm.)*
- **CanESM5 hindcast ICs:** Coupled assimilation constrained by ERA5 (atmosphere) and ORAS5 (ocean); radiative forcing follows CMIP6 historical through 2014, then the CMIP6 SSP2-4.5 scenario from 2015 onward.
- **GEM5.2-NEMO hindcast ICs:** ERA5 atmosphere (perturbed via MIDAS), ORAS5 ocean, Had2CIS sea ice (through 2015, GIOPS thereafter), and SPSv6.2 land surface.
- **Distributed alongside forecasts?** Yes — a dedicated `hindcast/` directory on the datamart, required for computing the recommended anomaly forecasts.

---

## Sources of predictability
Skill derives chiefly from the coupled ocean–atmosphere state (ENSO is the dominant driver), reinforced by sea-ice initialization that accounts for long-term Arctic thinning (SMv3) and, in v3, improved QBO representation via retuned non-orographic gravity-wave launching.

---

## Multi-model composition
- **Contributing systems:** CanESM5 and GEM5.2-NEMO — both ECCC/CCCma in-house models. This is a *single-provider* MME, distinct from multi-centre umbrellas like NMME or C3S.
- **Combination method:** Simple pooled ensemble on a common grid — 40 records per file: members 1–10 GEM5.2-NEMO (official-day), 11–20 GEM5.2-NEMO (lagged), 21–30 CanESM5 (official-day), 31–40 CanESM5 (lagged).
- **Common delivered grid:** 1° × 1° (also ~250 km via OGC-API).

---

## What it provides
- Individual ensemble member forecasts (raw; 40 members per file) — variables AirTemp, GeopotentialHeight, PrecipRate, SeaSfcHeight-Geoid, Pressure, WindU, WindV at surface (`Sfc`) and 2 m (`AGL-2m`)
- Deterministic ensemble-mean forecasts
- Anomaly products (AirTempAnomaly, PrecipAccumAnomaly) relative to the hindcast climatology
- Tercile probability products (below / near / above normal) for temperature and precipitation
- Threshold-exceedance probability products (nine percentile thresholds, 10th–90th)
- Hindcast (reforecast) fields
- Verification products (3-month normalized tercile anomalies scored against ERA5)
- Climate indices (CSV)

---

## Data availability
- **Is the data free?** Yes
- **License:** Environment and Climate Change Canada data servers end-use licence (open; attribution required)
- **Is the data downloadable?** Yes
- **Data formats:** GRIB2 (members and products); CSV (indices)
- **Official download location:**  
  - Forecast (members + products), 1°: https://dd.weather.gc.ca/today/model_cansips/100km/forecast/{YYYY}/{MM}/
  - Hindcast: https://dd.weather.gc.ca/today/model_cansips/100km/hindcast/{YYYY}/{MM}/
  - Verification, 1°: https://dd.weather.gc.ca/today/model_cansips/100km/verification/{YYYY}/
- **Access route notes:** Plain HTTPS directory browsing, or automated retrieval via AMQP push as files are published. Also served through MSC GeoMet (WMS/WCS) and the GeoMet OGC-API (members at 250 km); indices via a separate CSV datamart product. File nomenclature, e.g. `{YYYYMM}_MSC_CanSIPS_{Var}_{Level}_LatLon1.0_P{Month}M.grib2` (forecast members) and `{YYYYMM}_MSC_CanSIPS-Hindcast_{Var}_{Level}_LatLon1.0_P{Month}M.grib2` (hindcast).

---

## Notes
- **Single-provider MME.** CanSIPS combines two ECCC models internally; it is *not* a multi-centre product like NMME or C3S, though it contributes to both.
- **Forecast vs hindcast initialization asymmetry.** Real-time runs initialize from operational analyses (GDPS / GIOPS / GEPS), while hindcasts initialize from reanalyses (ERA5 / ORAS5). Worth bearing in mind when comparing real-time anomalies against the hindcast climatology.
- **Use anomalies, not raw fields.** ECCC explicitly recommends subtracting the hindcast climatology to form anomaly forecasts rather than using raw model output.
- **Lagged-start offset — open item.** The datamart documentation describes the lagged ensemble members as initialized **4 days** prior to month-end (forecast mode), whereas the technical note describes the hindcast lagged members as **5 days** beforehand. Confirm whether this is a genuine forecast/hindcast difference or a documentation inconsistency.
- **Out of scope:** the rendered seasonal outlook maps at weather.gc.ca/saisons are viewer-only products built on top of this data; the in-scope artifact is the raw GRIB2.
- **Related ECCC systems** (wire relative links if these entries exist): GEPS (atmospheric ICs for GEM5.2-NEMO), GDPS (land/atmosphere analyses), GIOPS (ocean/sea-ice ICs).

---

## Recent version history
- **v3.0.0 — 11 June 2024:** CanESM5 (5.1-p1) replaced CanCM4i; GEM5.1-NEMO upgraded to GEM5.2-NEMO (new land ICs and physics); ensemble size increased from 20 to 40; run-time bias correction added in CanESM5; enhanced land initialization. Full chronology: https://eccc-msc.github.io/open-data/msc-data/nwp_cansips/changelog_cansips_en/

---

## Official documentation
- Overview: https://eccc-msc.github.io/open-data/msc-data/nwp_cansips/readme_cansips_en/
- Datamart (GRIB2) readme: https://eccc-msc.github.io/open-data/msc-data/nwp_cansips/readme_cansips-datamart_en/
- Technical specifications (PDF): https://collaboration.cmc.ec.gc.ca/cmc/cmoi/product_guide/docs/tech_specifications/tech_specifications_CANSIPS_e.pdf
- Technical note (PDF): https://collaboration.cmc.ec.gc.ca/cmc/cmoi/product_guide/docs/tech_notes/technote_cansips_e.pdf
- Fact sheet (PDF): https://collaboration.cmc.ec.gc.ca/cmc/cmoi/product_guide/docs/fact_sheets/factsheet_cansips_e.pdf
- Discovery metadata (forecast): https://open.canada.ca/data/en/dataset/922781a9-bfef-56b9-a438-493ada629d47
- Discovery metadata (hindcast): https://open.canada.ca/data/en/dataset/0f4c5558-b51e-46e8-9e0c-b75545f217aa
