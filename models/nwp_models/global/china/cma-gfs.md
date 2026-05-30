# CMA-GFS (formerly GRAPES-GFS)

## What this model is
CMA-GFS is the operational global deterministic numerical weather prediction system developed and run by the China Meteorological Administration. It is the core global medium-range forecasting system of Chinese operational meteorology and provides initial and boundary conditions for CMA's downstream regional and specialty models.

The model was previously known as **GRAPES-GFS** (Global/Regional Assimilation and Prediction Enhanced System — Global Forecasting System). The system was renamed **CMA-GFS** in 2021. The legacy "GRAPES" name still appears in directory paths on CMA's public distribution endpoint and in older literature, but the current official name is CMA-GFS.

---

## Who runs it
- **Organization:** China Meteorological Administration (CMA) / CMA Earth System Modeling and Prediction Centre (CEMC)
- **Country / region:** China

---

## What area it covers
- **Coverage:** Global
- **Domain details:** Regular latitude–longitude grid, C-grid horizontal staggering, Charney–Phillips vertical staggering

---

## Basic details
- **Model type:** Deterministic global NWP
- **Model system / core:** CMA-GFS dynamical core (descended from GRAPES); semi-implicit semi-Lagrangian (SISL) with a predictor–corrector time integration scheme
- **Dynamical formulation:** Non-hydrostatic, fully compressible, shallow-atmosphere approximation in spherical coordinates
- **Convection-allowing:** No (global medium-range resolution)
- **Horizontal resolution:** ~0.125° (~12.5 km) since V4.0; was ~0.25° (~25 km) in V3.x
- **Vertical levels:** 87
- **Model top:** ~0.1 hPa (~63 km)
- **Forecast length:**
  - 240 hours (10 days) for 00 and 12 UTC cycles
  - 120 hours (5 days) for 06 and 18 UTC cycles
- **Update frequency / cycles:** 4× daily (00, 06, 12, 18 UTC)
- **Temporal output resolution:**
  - 6-hourly for 00 and 12 UTC cycles
  - 3-hourly for 06 and 18 UTC cycles

---

## Data assimilation
- **Data assimilation:** Yes — global 4D-Var
- **Method / cadence:** Incremental 4D-Var (Courtier et al., 1994) with a 6-hour assimilation window, run 4× daily. In V4.0 the system adopted a **strong-constraint** framework, replacing the earlier weak-constraint approach that used a digital filter. The V4.0 incremental analysis uses outer-loop / inner-loop horizontal resolutions of 0.25° / 1.0° with model time steps of 450 s (outer) and 900 s (inner).
- **Radiative transfer models:** RTTOV (v12.x) and the Advanced Radiative transfer Modeling System (ARMS, developed by CMA). ARMS was integrated in V4.0.
- **Satellite data assimilation:** Satellite-derived observations account for roughly 80% of assimilated data, including from Chinese FY (Fengyun) satellites — e.g. AMSU-A, MHS, ATMS, MWTS, MWHS, IASI, HIRAS, and FY-4A GIIRS (hyperspectral IR) and AGRI. Assimilation of AMSU-A surface-sensitive channels over land and three-dimensional cloud detection for FY-4A GIIRS are recent additions.

---

## What it provides
Deterministic global forecasts of:
- Temperature, humidity, and wind (including near-surface wind at 10, 30, 50, 70, 100, 120, 140, 160, 180, and 200 m above ground)
- Surface and mean sea level pressure
- Precipitation (including precipitation phase/type, added in V4.0)
- Cloud and hydrometeor fields
- Upper-air variables on 30 pressure levels

---

## Data availability
- **Is the data free?** Yes
- **License:** Distributed through CMA's GISC/WMC Beijing node of the WMO Information System (WIS) as WMO "essential" data under the **WMO Unified Data Policy (Resolution 1, Cg-Ext-2021, successor to Resolution 40)**, which provides for free and unrestricted international exchange. No CC-style license is published for this specific product; users should follow WMO data-policy attribution conventions. (TBD — no product-specific open-data license located.)
- **Is the data downloadable?** Yes
- **Data formats:** GRIB2
- **Official download location:**
  http://data.wis.cma.cn/DCPC_WMC_BJ/open/nwp/gmf_gra/

Data is organized into four cycle directories:
- `t0000/` — 00 UTC cycle, forecast hours 0–240 at 6-hour intervals
- `t0600/` — 06 UTC cycle, forecast hours 0–120 at 3-hour intervals
- `t1200/` — 12 UTC cycle, forecast hours 0–240 at 6-hour intervals
- `t1800/` — 18 UTC cycle, forecast hours 0–120 at 3-hour intervals

The `gmf_gra` path component retains the legacy "GRAPES" naming despite the rebranding to CMA-GFS.

---

## Notes
- CMA-GFS is the deterministic counterpart to CMA's global ensemble system (CMA-GEPS), which also uses the GRAPES/CMA dynamical core and was described in earlier literature as GRAPES-GEPS.
- Distribution is via CMA's WIS public endpoint operated by the World Meteorological Centre Beijing. The `gmf_gra` directory path preserves the legacy GRAPES naming; documentation referring to "GRAPES-GFS" or "GFS GRAPES" describes the same system now officially called CMA-GFS.
- CMA-GFS provides initial/boundary conditions for CMA's national and regional limited-area systems (e.g. CMA-MESO) and specialty models (e.g. CMA-TYM for typhoons), and is a key data source for AI-based forecasting research at CMA.
- After more than 20 years of independent development, CMA reports the Northern Hemisphere predictable-days score exceeding 8 days for the first time with V4.0, narrowing the gap with ECMWF and NCEP.

---

## Recent version history

### CMA-GFS V4.0 — operational 22 May 2023
(Passed operational review 21 February 2023.)
- Horizontal resolution increased from 0.25° (~25 km) to 0.125° (~12.5 km)
- Northern Hemisphere predictable days exceeded 8 for the first time
- Precipitation phase/state products added
- **Cloud microphysics:** graupel-related microphysical processes added to the Liu-Ma scheme (graupel colliding with cloud water, ice crystals, and snow; auto-conversion of ice crystals and snow to graupel; graupel melting and sublimation); cloud/rain evaporation rate restricted to improve precipitation efficiency
- **Convection (NSAS scheme):** sub-cloud-layer environmental relative humidity added to the convection trigger over land; entrainment-rate sensitivity to environmental humidity increased; quasi-equilibrium closure optimized — reducing spurious light rain and missed heavy rain
- **Mass conservation:** a mass-conservation correction algorithm introduced to address long-integration mass loss and the associated decay of synoptic systems (e.g. the subtropical high)
- **Efficiency:** the 3D reference profile replaced by a **2D reference profile** (time step extended from 240 s to 300 s at 0.125°); the Helmholtz solver switched from GCR to a **preconditioned classical Stiefel iteration (PCSI)**; radiation and predictor–corrector algorithms optimized — overall integration efficiency increased by ~1/3
- **Radiative transfer / DA:** ARMS integrated alongside RTTOV; strong-constraint 4D-Var framework adopted

### GRAPES-GFS V3.1 — operational April 2021
- Methane oxidation scheme added (improves upper-stratosphere water vapor and reduces temperature/height bias)
- Updated land surface model (including supercooled soil water)
- Improved moist physical processes / cumulus convection (revised trigger, auto-conversion, sub-cloud treatment, shallow-convection entrainment/detrainment)

### CMA-GFS V3.0 — dynamical core upgrade, operational June 2020
(Described in Shen et al., 2023.)
- Classical 2TL SISL scheme extended to a predictor–corrector formulation (avoiding the temporal extrapolation of nonlinear residuals and midpoint winds that caused instability)
- 3D reference profile replaces the original isothermal reference profile
- Hybrid height-based terrain-following vertical coordinate introduced
- New spectral-filter terrain and reduced artificial damping
- Time integration now second-order accurate; time step extended by ~50%; ~30% efficiency improvement
- Combined with corresponding 4D-Var modifications, this constituted CMA-GFS V3.0 (replacing V2.4)

---

## Official documentation
- World Meteorological Centre Beijing: http://www.wmc-bj.net/
- CMA-GFS V4.0 operational announcement (CMA): https://www.cma.gov.cn/en2014/research/News/202306/t20230601_5545266.html
- CMA-GFS V4.0 operational review (CMA): https://www.cma.gov.cn/en2014/news/News/202303/t20230306_5344412.html
- GRAPES-GFS V3.1 upgrade (WMC Beijing): http://www.wmc-bj.net/publish/cms/view/40ee0aaf59874b0c821993238e7726b4.html
- CMA Earth System Modeling and Prediction Centre: https://www.cma.gov.cn/

### Key references
- Shen, X. S., Y. Su, H. L. Zhang, and J. L. Hu, 2023: New Version of the CMA-GFS Dynamical Core Based on the Predictor–Corrector Time Integration Scheme. *J. Meteor. Res.*, **37**(3), 273–285. doi:10.1007/s13351-023-3002-0
- Zhang, J., J. Sun, X. Shen, et al., 2023: Key Model Technologies of CMA-GFS V4.0 and Application to Operational Forecast. *J. Appl. Meteor. Sci.*, **34**(5), 513–526. doi:10.11898/1001-7313.20230501 (in Chinese)
- Zhang, L., et al., 2019: The operational global four-dimensional variational data assimilation system at the China Meteorological Administration. *Quart. J. Roy. Meteor. Soc.*, **145**, 1882–1896. doi:10.1002/qj.3533
- Xiao, H., et al., 2023: Assimilation of AMSU-A Surface-Sensitive Channels in CMA_GFS 4D-Var System over Land. *Wea. Forecasting*, **38**(9), 1777–1790. doi:10.1175/WAF-D-23-0032.1
- Wang, L., et al., 2025: Impact of a New Three-Dimensional Cloud Detection Method of FY4A GIIRS in the CMA-GFS. *Wea. Forecasting* (early online release). doi:10.1175/WAF-D-24-0087.1
