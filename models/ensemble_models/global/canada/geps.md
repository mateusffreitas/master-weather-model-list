# GEPS (Global Ensemble Prediction System)

## What this model is
The Global Ensemble Prediction System (GEPS) is Canada's global ensemble numerical weather prediction system, providing probabilistic medium-range forecasts and quantifying forecast uncertainty from the medium range into the extended range.

GEPS is the ensemble counterpart to the deterministic [GDPS](../../../nwp_models/global/canada/gem-global.md), built on the same Global Environmental Multiscale (GEM) atmospheric model and run as a coupled atmosphere–ocean–sea ice ensemble using NEMO and CICE for the ice-ocean component. It generates 20 perturbed members plus 1 control by combining perturbed initial conditions from a 256-member ensemble data assimilation system with stochastic representations of model uncertainty.

GEPS plays a central operational role within ECCC's prediction suite: it provides the perturbed background fields used to construct the flow-dependent background-error covariances for the deterministic [GDPS](../../../nwp_models/global/canada/gem-global.md), [RDPS](../../../nwp_models/regional/canada/rdps.md), and [HRDPS](../../../nwp_models/regional/canada/hrdps.md) 4DEnVar analyses, supplies initial and lateral boundary conditions for the regional [REPS](../../regional/canada/reps.md) ensemble, and contributes 21 members (1 control + 20 perturbed) to the multi-center [NAEFS](../usa/naefs.md) and [557th WW GEPS](../usa/557wg-geps.md) products.

The current operational version is **GEPS 8.0.0**, implemented on June 11, 2024 (12 UTC run) as part of Innovation Cycle 4 (IC-4), alongside GDPS 9.0.0 and RDPS 9.0.0. Headline changes from GEPS 7.0 include a horizontal resolution increase from ~39 km to ~25 km, a transition of the data assimilation cycle to a Local Ensemble Transform Kalman Filter (LETKF) recentered on a 4DEnVar analysis, an upgrade of the sea ice component to CICE 6.2.0 with Delta-Eddington radiation, and an extension of the reforecast system from 32 to 39 days with twice-weekly cycles.

---

## Who runs it
- **Organization:** Canadian Meteorological Centre (CMC) / Environment and Climate Change Canada
- **Country / region:** Canada

---

## What area it covers
- **Coverage:** Global
- **Atmospheric grid:** Yin–Yang horizontal grid at uniform 0.23° (~25 km) resolution
- **Ice-ocean grid:** Global 0.25° with 50 z-levels (shared with GDPS via GIOPS initialization)

---

## Basic details
- **Model type:** Global ensemble NWP, coupled to ocean and sea ice
- **Model system / core:** GEM (Global Environmental Multiscale) version 5.2.3, two-way coupled to NEMO 3.6 ocean and CICE 6.2.0 sea ice
- **Dynamical formulation:** Hydrostatic primitive equations
- **Convection-allowing:** No (~25 km horizontal resolution)
- **Ensemble size:** 21 (1 control + 20 perturbed) for the medium-range forecast
- **Horizontal resolution:** ~25 km (0.23° quasi-uniform Yin–Yang grid)
- **Vertical levels:** 84 staggered hybrid levels (Charney–Phillips vertical grid)
- **Model top:** 0.1 hPa
- **Forecast length:**
  - Up to 16 days (384 h) on the 00 and 12 UTC medium-range cycles
  - Extended to 39 days (936 h) on Monday and Thursday 00 UTC runs, supporting subseasonal anomaly products
  - 72 h on the additional 06 and 18 UTC early forecast cycles, used to provide initial and boundary conditions for [REPS](../../regional/canada/reps.md)
- **Update frequency / cycles:** 2× daily medium-range runs (00, 12 UTC) plus 4× daily early forecast runs (00, 06, 12, 18 UTC) that pilot REPS
- **Time step:** 900 seconds (forecast); 720 seconds (12 min) for the LETKF trial-field integrations
- **Time integration:** Implicit, semi-Lagrangian (3D), 2 time-level (Côté et al., 1998a, 1998b)
- **Numerical technique:** Finite differences on the Arakawa C grid (horizontal) and Charney–Phillips grid (vertical)
- **Temporal output resolution:** Typically 3-hourly

---

## Data assimilation
- **Data assimilation:** Yes — GEPS runs its own ensemble analysis cycle, distinct from the GDPS deterministic 4DEnVar analysis but tightly integrated with it
- **Method:** **Local Ensemble Transform Kalman Filter (LETKF)** (Buehner, 2020) with cross-validation, run with **256 members** subdivided into 8 sub-ensembles of 32 members each. Sub-ensemble membership is randomly redrawn at each analysis time. The LETKF analyses are recentered on a companion 4DEnVar analysis with a height-dependent recentering coefficient ("hybrid gain")
- **4DEnVar recentering analysis:** Run on the same 25 km Yin–Yang grid as the LETKF, using the LETKF ensemble-mean background trajectory as first guess
- **Background trial fields:** 3- to 9-hour forecasts from a 256-member coupled GEM ensemble integrated with stochastic perturbations (SPP and SKEB, see below) and homogeneous isotropic model-error random fields applied additively to the analysis (Houtekamer et al. 2009, 2014). Observations themselves are no longer perturbed
- **Cadence:** 4× daily (00, 06, 12, 18 UTC) with a 6-hour assimilation window centred on the synoptic hour
- **Cut-off times:**
  - 7 hours for final analyses (00, 06, 12, 18 UTC)
  - 3 hours for the 00 and 12 UTC medium-range forecast runs (subsampled 20-member subset, mean constrained to match the full 256-member ensemble mean)
  - 2 hours for the 00, 06, 12, 18 UTC early forecast runs that pilot REPS
- **Initialization:** Incremental Analysis Update (IAU) (Bloom et al. 1996; Buehner et al. 2015)
- **Radiative transfer model:** RTTOV v13
- **Notable observations assimilated (LETKF):** Radiosonde upper-air and surface, surface stations (SYNOP BUFR, SWOB, METAR), aircraft, atmospheric motion vectors, scatterometer winds, ATOVS level 1b (AMSU-A and AMSU-B/MHS, both GLOBAL and RARS sources), GPS-RO, ATMS, AIRS, IASI, CrIS
- **Additional observations used in the recentering 4DEnVar:** Ground-based GPS-RO, clear-sky radiances (CSR), and SSMIS radiances, in addition to the LETKF observation set
- **Background check and bias correction:** Performed within the GEPS analysis cycle itself

The 256-member LETKF ensemble at the heart of GEPS does double duty: it produces the GEPS perturbed initial conditions, and the 256 perturbed background trajectories are used directly as the flow-dependent background-error covariances in the deterministic GDPS, RDPS, and HRDPS hybrid 4DEnVar analyses. This shared analysis ensemble is the core architectural link between Canada's deterministic and ensemble systems.

---

## Perturbations and design
- **Initial condition perturbations:** Each of the 20 perturbed members is initialized from a different LETKF analysis selected from the 256-member set, with the subsample chosen so that its mean matches the full 256-member ensemble mean. The control member (member 0) is initialized from the recentered ensemble-mean analysis. All members share the same ocean and sea ice initial conditions, taken from the [GIOPS](../../../ocean_models/global/canada/giops.md) analysis (Smith et al. 2016)
- **Model uncertainty representation:** Two complementary stochastic schemes (McTaggart-Cowan et al. 2022a, b):
  - **Stochastic Parameter Perturbation (SPP):** 20 physics parameters and algorithmic choices are perturbed continuously in space and time using Markovian random fields. Perturbed elements span deep convection (trigger thresholds, downdraft detrainment, autoconversion rate, cloud radius), boundary-layer mixing (turbulent exchange coefficients, mixing length, critical Richardson number, TKE transport), cloud microphysics and radiation (condensate threshold, cloud water/ice radii, aerosol concentration, condensation thresholds), gravity-wave drag (launch-level RMS, subgrid-scale flow blocking), and semi-Lagrangian advection accuracy. In v8.0.0, the SPP element distributions are recentered around the new control member, the range for `spp_adv_rhsint` (advection interpolation order) is reduced to address tropical over-dispersion, and the stretching parameter γ is increased by a factor of 2·ln 2 across all elements to compensate for the increased variance from the improved Markovian field representation
  - **Stochastic Kinetic Energy Backscatter (SKEB):** Adds wind increments proportional to diagnosed dissipation from explicit horizontal diffusion, parameterized gravity-wave drag, and (since GEPS 7.0) deep-convective momentum transfer (Charron et al. 2010; Shutts 2005). In v8.0.0 the backscattering fraction `ens_skeb_alph` was reduced from 1.0 to a more physical value of 0.7 to compensate for the improved Markovian field representation
- **Control member:** Receives no model uncertainty perturbation. Its physics configuration is fine-tuned starting from the 25 km GDPS configuration to align with the deterministic system at the new resolution

The control member is built from the recentered ensemble mean rather than from an independent deterministic analysis — a design choice that keeps the ensemble statistically consistent with its own analysis cycle while remaining tightly tied to GDPS through the shared LETKF backgrounds and 4DEnVar recentering.

---

## Coupled ice-ocean component
- **Ocean model:** NEMO 3.6 (Madec 2008)
- **Sea ice model:** CICE 6.2.0 (Hunke and Lipscomb 2010; Hunke et al. 2021), upgraded from earlier CICE versions in v8.0.0
- **Sea ice physics (new in v8.0.0):**
  - Radiation scheme: Delta-Eddington (R_ice = R_pnd = R_snw = 2.0; Briegleb and Light 2007)
  - Conductivity scheme: "bubbly" (Pringle et al. 2007)
- **Coupling:** Two-way atmosphere–ocean–ice coupling via the in-house GOSSIP coupler. New GEM–NEMO coupling weights were introduced in v8.0.0
- **Ice-ocean initial conditions:** From the [GIOPS](../../../ocean_models/global/canada/giops.md) analysis (sea ice concentration, sea ice thickness, sea ice snow depth, and sea surface temperature). All ensemble members share the same ocean and sea ice initial state
- **Geophysical fields:** Regenerated for the new 25 km Yin–Yang grid using the unified `prep_geophy` package; filtered topography produced with a scale-selective low-pass filter (McTaggart-Cowan et al. 2019)

---

## What it provides
Probabilistic global forecasts including:
- Individual ensemble member forecasts (1 control + 20 perturbed)
- Ensemble mean and ensemble spread
- Surface and upper-air fields: temperature, wind components, geopotential height, specific and relative humidity, mean sea level and surface pressure, dew point temperature and depression, omega, relative vorticity, boundary-layer height
- Quantitative precipitation forecast (QPF), precipitation rate and type
- Cloud cover and cloud condensate
- Sea surface conditions (SST, sea ice concentration) from the coupled ice-ocean component
- Probability and percentile-based products derived from the 21-member ensemble
- Subseasonal anomaly products derived from the 39-day Monday/Thursday 00 UTC runs in conjunction with the GEPS reforecast database
- Initial and lateral boundary conditions for the regional [REPS](../../regional/canada/reps.md) ensemble (via the 06/18 UTC early forecast cycles)
- Perturbed background trajectories used as flow-dependent background-error covariances by the [GDPS](../../../nwp_models/global/canada/gem-global.md), [RDPS](../../../nwp_models/regional/canada/rdps.md), and [HRDPS](../../../nwp_models/regional/canada/hrdps.md) deterministic 4DEnVar analyses

---

## Reforecast system
GEPS includes an operational reforecast system used to calibrate medium- and extended-range probabilistic products, particularly the monthly forecasts derived from the 39-day Monday/Thursday 00 UTC runs (Hagedorn 2008; Gagnon et al. 2014b).

- **Coverage:** 20 years
- **Cadence:** Twice weekly (Monday and Thursday 00 UTC), extended in v8.0.0 from the previous Thursday-only schedule
- **Forecast length:** 39 days (extended from 32 days in v7.0)
- **Members per reforecast date:** 4 (reduced from the 21 used operationally to control computational cost)
- **Effective historical database:** ~80 reforecasts per calendar date (4 members × 20 years)
- **Atmospheric initial conditions:** ERA5 reanalyses with random homogeneous isotropic perturbations applied to the 4 members (same amplitude as in earlier GEPS versions; Houtekamer et al. 2009)
- **Land surface initial conditions:** Generated by running the offline Surface Prediction System (SPS; Carrera et al. 2010) forced with ERA5 at the lowest model level (rather than the diagnostic level), to maintain consistency with the operational forecast system
- **Ocean initial conditions:** ORAS5 reanalysis (Zuo et al. 2015)
- **Sea ice initial conditions:** Had2CIS (HadISST2.2 combined with digitized CIS sea ice charts over the Arctic and Great Lakes) prior to January 2016; CMC GIOPS analysis from January 2016 onward — chosen to minimize discontinuity given Had2CIS and GIOPS share a similar climatology
- **Sea ice thickness:** ORAS5 monthly reanalysis
- **Stochastic perturbations:** Same SPP and SKEB schemes as the operational forecast system, including the v8.0.0 improvements

The fifth forecast week added in v8.0.0 (extending from 32 to 39 days) and the new Monday cycle together expand both the lead-time coverage and the sample size of the reforecast database used for skill calibration of the monthly forecast.

---

## Relationship to other models
- **[GDPS](../../../nwp_models/global/canada/gem-global.md):** Deterministic counterpart, sharing the GEM atmospheric core, the NEMO/CICE ice-ocean components, and the LETKF ensemble at the heart of ECCC's data assimilation system. GDPS uses GEPS's 256-member LETKF backgrounds as its full ensemble-derived B matrix; GEPS in turn uses a 4DEnVar recentering analysis run on the same grid as GDPS
- **[RDPS](../../../nwp_models/regional/canada/rdps.md) and [HRDPS](../../../nwp_models/regional/canada/hrdps.md):** Both deterministic regional systems also draw flow-dependent background-error covariances from the same 256-member GEPS 8.0.0 LETKF ensemble
- **[REPS](../../regional/canada/reps.md):** Regional short-range ensemble nest, initialized and laterally driven by the GEPS 06/18 UTC early forecast cycles
- **[NAEFS](../usa/naefs.md):** North American multi-center bias-corrected ensemble combining 21 GEPS members with 31 GEFS members for a 52-member product
- **[557th WW GEPS](../usa/557wg-geps.md):** U.S. Air Force statistical multi-model ensemble (note the acronym collision — see Notes) that uses the Canadian GEPS as one of three contributing single-center ensembles

---

## Data availability
- **Is the data free?** Yes (no registration required for MSC Open Data)
- **License:** Environment and Climate Change Canada Data Servers End-use Licence (attribution required; commercial use permitted) — https://eccc-msc.github.io/open-data/licence/readme_en/
- **Is the data downloadable?** Yes
- **Data formats:** GRIB2
- **Official download location:** https://dd.weather.gc.ca/today/ensemble/geps/grib2

---

## Notes
- GEPS is part of ECCC's Innovation Cycle 4 (IC-4) prediction suite. The June 11, 2024 implementation upgraded GEPS, GDPS, and RDPS together, with the 25 km GEPS LETKF ensemble providing the shared analysis backbone for all three deterministic-ensemble pairs.
- Open data licensing is genuinely open — no registration required, direct file access via the Datamart. Same as GDPS, RDPS, HRDPS, GIOPS, and RIOPS.
- As with all ensemble systems, GEPS output should be interpreted probabilistically rather than as a single forecast. The system's value is in calibrated probabilities and ensemble spread, not in any single-member view.
- The acronym "GEPS" is used for two unrelated systems in operational forecasting. The Canadian GEPS described here (Global Ensemble Prediction **System**) is Environment Canada's single-center coupled global ensemble. A separate product called [557th WW GEPS](../usa/557wg-geps.md) (Global Ensemble Prediction **Suite**) is a U.S. Air Force multi-model statistical ensemble that uses the Canadian GEPS as one of its inputs.

---

## Recent version history

### GEPS v8.0.0 — operational June 11, 2024 (current)
Implemented at the 12 UTC run on June 11, 2024 as part of Innovation Cycle 4 (IC-4), alongside GDPS 9.0.0 and RDPS 9.0.0.

Headline changes from v7.0:
- **Horizontal resolution increased from ~39 km to ~25 km** (Yin–Yang grid at 0.23° uniform resolution)
- **GEM upgraded to version 5.2.3** for the forecast component (5.2.1 for the assimilation component)
- **LETKF analysis recentered on a 4DEnVar analysis** with a height-dependent (rather than uniform) recentering coefficient — the "hybrid gain" approach revisited and aligned with the GDPS configuration
- **RTTOV upgraded to v13**
- **All-sky observation handling improvements:** correction to the water saturation functions; modification of hyperspectral infrared QC for albedo changes; updated source for sea-ice snow depth and ice thickness
- **Sea ice component upgraded to CICE 6.2.0** with **Delta-Eddington radiation** (R_ice = R_pnd = R_snw = 2.0) and **"bubbly" thermal conductivity** scheme — same configuration as GIOPS 3.5.0 and GDPS 9.0.0
- **New GEM–NEMO coupling weights**
- **SPP and SKEB schemes improved** through better-represented Markovian perturbation fields. SPP element distributions are recentered around the new control; the range for `spp_adv_rhsint` is reduced to address tropical over-dispersion; the stretching parameter γ is increased by 2·ln 2 across all SPP elements; the SKEB backscattering fraction `ens_skeb_alph` is reduced from 1.0 to a more physical 0.7
- **Background check and bias correction now performed within GEPS** itself
- **Geophysical fields regenerated** for the 25 km grid using the unified `prep_geophy` package, with scale-selective filtered topography (McTaggart-Cowan et al. 2019)
- **Reforecast system extended:**
  - Forecast length extended from 32 to 39 days (adding a fifth forecast week)
  - Cadence doubled from weekly (Thursday 00 UTC) to twice-weekly (Monday and Thursday 00 UTC)
  - Reforecast members reduced to 4 (from the 20+ used operationally) to control computational cost while maintaining ~80 reforecasts per calendar date across the 20-year period
  - Sea ice initial conditions from CMC GIOPS analysis from January 2016 onward (replacing Had2CIS), with no significant climatological discontinuity

---

## Official documentation
- GEPS open data page: https://eccc-msc.github.io/open-data/msc-data/nwp_geps/readme_geps-datamart_en/
- GEPS readme: https://eccc-msc.github.io/open-data/msc-data/nwp_geps/readme_geps_en/
- Technical specifications (current, v8.0.0): https://collaboration.cmc.ec.gc.ca/cmc/CMOI/product_guide/docs/tech_specifications/tech_specifications_GEPS_e.pdf
- ECCC operational systems changelog: https://eccc-msc.github.io/open-data/msc-data/changelog_multisystems_en/
- Licence: https://eccc-msc.github.io/open-data/licence/readme_en/

### Key references
- Buehner, M. (2020). Local Ensemble Transform Kalman Filter with Cross Validation. *Mon. Wea. Rev.*, 148, 2265–2282.
- Buehner, M., et al. (2015). Implementation of Deterministic Weather Forecasting Systems Based on Ensemble–Variational Data Assimilation at Environment Canada. Part I: The Global System. *Mon. Wea. Rev.*, 143, 2532–2559. https://doi.org/10.1175/MWR-D-14-00354.1
- Charron, M., G. Pellerin, L. Spacek, P. L. Houtekamer, N. Gagnon, H. L. Mitchell, and L. Michelin (2010). Toward Random Sampling of Model Error in the Canadian Ensemble Prediction System. *Mon. Wea. Rev.*, 138, 1877–1901.
- Côté, J., et al. (1998a). The Operational CMC-MRB Global Environmental Multiscale (GEM) Model: Part I — Design Considerations and Formulation. *Mon. Wea. Rev.*, 126, 1373–1395.
- Houtekamer, P. L., X. Deng, H. L. Mitchell, S.-J. Baek, and N. Gagnon (2014). Higher Resolution in an Operational Ensemble Kalman Filter. *Mon. Wea. Rev.*, 142, 1143–1162.
- Houtekamer, P. L., H. L. Mitchell, and X. Deng (2009). Model Error Representation in an Operational Ensemble Kalman Filter. *Mon. Wea. Rev.*, 137, 2126–2143.
- McTaggart-Cowan, R., L. Separovic, R. Aider, M. Charron, M. Desgagné, P. L. Houtekamer, D. Paquin-Ricard, P. Vaillancourt, and A. Zadra (2022a). Using stochastic parameter perturbations to represent model uncertainty, Part I: Implementation and parameter sensitivity.
- McTaggart-Cowan, R., L. Separovic, M. Charron, X. Deng, N. Gagnon, P. L. Houtekamer, and A. Patoine (2022b). Using stochastic parameter perturbations to represent model uncertainty, Part II: Comparison with existing techniques in an operational ensemble.
- McTaggart-Cowan, R., et al. (2019). Modernization of Atmospheric Physics Parameterization in Canadian NWP. *J. Adv. Model. Earth Syst.*, 11, 3593–3635. https://doi.org/10.1029/2019MS001781
- Shutts, G. (2005). A kinetic energy backscatter algorithm for use in ensemble prediction systems. *Q. J. R. Meteorol. Soc.*, 131, 3079–3102. https://doi.org/10.1256/qj.04.106
- Smith, G. C., et al. (2016). Sea ice Forecast Verification in the Canadian Global Ice Ocean Prediction System. *Q. J. R. Meteorol. Soc.*, 142, 659–671. https://doi.org/10.1002/qj.2555
- Zuo, H., M. A. Balmaseda, and K. Mogensen (2015). The new eddy-permitting ORAP5 ocean reanalysis: description, evaluation and uncertainties in climate signals. *Climate Dynamics*. https://doi.org/10.1007/s00382-015-2675-1
