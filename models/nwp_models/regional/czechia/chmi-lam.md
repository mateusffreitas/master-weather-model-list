# ALADIN (CHMI – Czech Republic)

## What this model is
ALADIN/CHMI is the operational regional limited-area numerical weather prediction (NWP) system run by the Czech Hydrometeorological Institute (CHMI) for short-range forecasting over Czechia and Central Europe.

It is a non-hydrostatic configuration of the **ALADIN system** with **ALARO-1vB physics**, developed and maintained as part of the Regional Cooperation for Limited Area Modeling in Central Europe (**RC LACE**) consortium. Two domains are produced and openly distributed: a **2.3 km Lambert** parent covering Central Europe and a **1 km Czech-Republic** sub-domain.

The system is notable for its **operational biometeorological outputs** (mean radiant temperature and Universal Thermal Climate Index), which are uncommon among national ALADIN deployments and feed into CHMI's national heat-health warning service.

---

## Who runs it
- **Organization:** Czech Hydrometeorological Institute (Český hydrometeorologický ústav, ČHMÚ)
- **Country / region:** Czech Republic
- **Consortium:** RC LACE (with ALADIN, HIRLAM, and ECMWF cooperation; now under the wider ACCORD umbrella)

---

## What area it covers
- **Coverage:** Central Europe, with Czechia near the centre
- **Domains:**
  - **Lambert_2.3km** — Central Europe parent domain on a Lambert projection, 1069 × 853 grid points, ~2.3 km grid spacing
  - **CZ_1km** — Czech Republic and immediately surrounding terrain at ~1 km grid spacing (high-resolution sub-domain)

---

## Basic details
- **Model type:** Regional deterministic NWP (limited-area)
- **Model system / core:** ALADIN, with **ALARO-1vB** physics package
- **Dynamical formulation:** Non-hydrostatic (NH), spectral, semi-Lagrangian advection, semi-implicit time integration; ICI scheme with 1 iteration
- **Convection-allowing:** Yes (deep convection explicitly resolved at both 2.3 km and 1 km)
- **Horizontal resolution:**
  - Lambert_2.3km: ~2.3 km (linear truncation E539 × 431)
  - CZ_1km: ~1 km
- **Vertical levels:** 87 (mean orography)
- **Time step:** 90 s (Lambert_2.3km)
- **Forecast length:** Up to **+72 h** (00, 06, 12, 18 UTC cycles, all to +72 h since January 2025; previously the 18 UTC run went only to +54 h)
- **Update frequency:** 4× daily (00, 06, 12, 18 UTC)
- **Temporal output resolution:** Hourly
- **Cycle / model code version:** ALADIN cycle **46t1as_op1** (as of the 2025 ACCORD ASW poster); previous cycles documented operationally include 43t2ag (2022), 46t1mp (2023–2024)

---

## Data assimilation
- **Surface analysis:** Optimal interpolation (OI / CANARI) using SYNOP T2m and RH2m; SST treatment is by relaxation toward the LBC0 (ARPEGE) field; snow analysis with relaxation to climatology suppressed since February 2024 to retain realistic snow amounts
- **Upper-air analysis:** **BlendVar** — digital-filter spectral blending (Brožková et al., 2001) at truncation E102 × 81, followed by **3D-Var** (Fischer et al., 2005)
- **Cycling:** Hourly analysis system (VarCan Pack); long cut-off cycle was tightened from 6 h to **3 h** in February 2024
- **Initialisation:** Incremental DFI in the short cut-off production analysis; no DFI in the long cut-off cycle
- **Assimilated observations:** SYNOP, TEMP, AMDAR, **Mode-S EHS** (full European coverage via EMADDC since 2022), SEVIRI (including water-vapour channels reintroduced with updated VARBC in September 2024), wind profilers, HR-AMV, ASCAT
- **Radar reflectivity assimilation:** OPERA volume reflectivity 1D+3D-Var assimilation in e-suite ALE since February 2025

---

## Initial and boundary conditions
- **Initial conditions:** Own BlendVar + CANARI analysis
- **Lateral boundary conditions:** **ARPEGE** (Météo-France global model), 3-hour coupling interval, space-consistent coupling

---

## What it provides
Deterministic forecasts of standard meteorological variables on the model grid, including:
- 2 m temperature, dew point, and humidity
- 10 m wind speed, direction, and gust components
- mean sea-level pressure and surface pressure
- hourly and accumulated precipitation, precipitation type, snowfall, snow water equivalent
- low / mid / high / total cloud cover and visibility (incl. visibility in cloud and in precipitation)
- shortwave and longwave radiation fluxes (downward), sunshine duration
- maximum simulated reflectivity, lightning flash density
- pressure-level fields (geopotential, temperature, humidity, wind, vertical velocity, theta) on standard isobaric levels
- convective diagnostics — Most Unstable CAPE/CIN, Storm Relative Helicity, updraft and downdraft helicity tracks, wind shear (added/revised May 2023)
- biometeorological outputs — **mean radiant temperature (Tmrt)** and **Universal Thermal Climate Index (UTCI)**, supporting CHMI's biometeorological forecasting and heat-health warning system

---

## Data availability
- **Is the data free?** Yes
- **License:** TBD (the CHMI open-data portal does not publish an explicit licence on the NWP ALADIN directory)
- **Is the data downloadable?** Yes
- **Data formats:** GRIB (distributed as **bzip2-compressed `.grb.bz2`** files; one file per variable per cycle, containing all forecast steps)
- **Official download location:**  
  https://opendata.chmi.cz/meteorology/weather/nwp_aladin/  
  with sub-directories `Lambert_2.3km/{00,06,12,18}/` and `CZ_1km/{00,06,12,18}/`
- **Reference files at the same location:**
  - `Popis_obsahu.xlsx` — content description (parameter list)
  - `grib2table` and `gribtab` — GRIB code tables for the distributed parameters

---

## Notes
- The current high-resolution Lambert_2.3km configuration has been operational since **February 2019**, replacing the previous 4.6 km version with improved resolution, orography, and radiation diagnostics. The CZ_1km sub-domain is a higher-resolution downscaling on the same operational chain.
- The publicly distributed files use a `ALADLAMB4opendata_*` filename pattern keyed to cycle date/hour and parameter group. Filenames for the CZ_1km domain use a different prefix on the corresponding sub-directory.
- **Companion / related ALADIN deployments in the consortium** share much of the same code base and tunings; cross-references to other entries in this repository:
  - [ALADIN Slovakia](../slovakia/aladin-slovakia.md) — RC LACE sister system; SST treatment and L63→L87 cloud retuning developed jointly with CHMI.
  - [ALARO Belgium](../belgium/alaro-belgium.md) — RMI 1.3 km tunings provided via cooperation with CHMI.
- Operational HPC: NEC SX Aurora TSUBASA (48 nodes, AMD EPYC 7402 + 8× NEC Vector Engines 20B per node); a NEC LX series cluster (320 Intel Broadwell nodes) was also in use through 2024. Workflow management via ecFlow.
- For the parent global model that supplies LBCs, see [ARPEGE Global](../../global/france/arpege-global.md).

---

## Recent version history

### February 2025 — radar reflectivity DA e-suite
The **e-suite ALE** introduced 1D+3D-Var assimilation of OPERA volume radar reflectivity, using the Wattrelot et al. (2014) method with the Abel & Boutle (2012) rain size distribution in the observation operator (consistent with the ALARO microphysics) and a threshold-method + error-inflation treatment to mitigate excessive drying from undetected pixels.

### January 2025 — 18 UTC cycle extended to +72 h
The 18 UTC production cycle was prolonged from +54 h to +72 h, bringing all four daily cycles to the same forecast length.

### October 2024 — Abel & Boutle rain size distribution
The Abel & Boutle (2012) rain size distribution was implemented for simulated reflectivity diagnostics and the radar reflectivity observation operator, replacing the earlier Marshall–Palmer distribution and giving more realistic reflectivities in light rain (lower) and heavy rain (higher).

### September 2024 — SEVIRI water-vapour channels reintroduced
SEVIRI water-vapour channels were brought back into the data-assimilation observation set with an updated VARBC bias-correction configuration.

### February 2024 — long cut-off cycle to 3 h, snow / surface assimilation retuning
The long cut-off assimilation cycle was tightened from 6 h to **3 h**. Surface analysis settings were retuned: relaxation to climatology was suppressed for snow (recovering realistic snow amounts), the snow-roughness treatment was switched to `LZ0SNOWH=T` / `RZ0_TO_HEIGHT=0.1` (improving 10 m wind bias over forested snow-covered areas), and the snow-fraction parameter `WCRIN` was raised from 4 to 10 (reducing 2 m cold bias). The Lopez evaporation parameterization and retuned auto-conversions to snow and cloud water were also introduced.

### May 2023 — new convective diagnostics
A revised convective-diagnostics package became operational, in cooperation with CHMI's convection-specialist team. New products include storm relative helicity, updraft and downdraft helicity tracks, and combinations of MUCAPE/CIN/moisture convergence and CAPE/wind shear, useful for distinguishing squall lines from supercells and for tornado-environment indicators.

### April 2023 — model cycle upgrade to CY46T1mp
The operational model cycle was upgraded to **CY46T1mp** (ALARO-1vB).

### February 2022 — prognostic graupel
The ALARO microphysics scheme was upgraded with **fully prognostic graupel**, replacing the earlier pseudo-prognostic treatment. A new graupel fall-speed relation (matching ICE3-style behaviour) was introduced together with retuned precipitation-type thresholds for small and big hail. This change also enabled a straightforward lightning-density diagnostic.

### January 2022 — Mode-S EHS via EMADDC
Mode-S EHS aircraft observations from the previous KNMI/MUAC airspace (Belgium, Netherlands, Luxembourg, Germany) were replaced by **European-wide EMADDC** processing, with shorter assimilation window (1 h, ±30 min around analysis) reflecting the very high data frequency.

### February 2019 — current 2.3 km / L87 non-hydrostatic configuration
The non-hydrostatic 2.3 km, 87-level configuration with ALARO-1vB physics replaced the previous 4.6 km version. The mean radiant temperature diagnostic was added in autumn 2018, followed by the Universal Thermal Climate Index (UTCI) in January 2019.

---

## Official documentation
- CHMI open-data portal (NWP ALADIN): https://opendata.chmi.cz/meteorology/weather/nwp_aladin/
- Novák, M. (2021): *UTCI as the NWP model ALADIN (CHMI) output – first experiences*, Geographia Polonica 94(2), 237–249. https://doi.org/10.7163/GPol.0203
- Bučánek, A., Trojáková, A., Benáček, P. (2018): *Data assimilation activities at CHMI*, RC LACE DAWD + DAsKIT, Bucharest.
- Trojáková, A. et al. (2022): *Numerical Weather Prediction activities at CHMI*, ACCORD All-Staff Workshop poster.
- Brožková, R., Bučánek, A., Mašek, J., Němec, D., Smolíková, P., Trojáková, A. (2023): *Numerical Weather Prediction activities at CHMI*, EWGLAM 2023 poster.
- Brožková, R. et al. (2024): *Numerical Weather Prediction activities at CHMI*, ACCORD All-Staff Workshop 2024 poster.
- Brožková, R., Bučánek, A., Mašek, J., Němec, D., Šljivić, A., Ševčík, J., Smolíková, P., Trojáková, A. (2025): *Numerical Weather Prediction activities at CHMI*, 5th ACCORD All-Staff Workshop poster, Zalakaros.

### Key references
- Termonia, P. et al. (2018): *The ALADIN System and its canonical model configurations AROME CY41T1 and ALARO-0 CY40T1*, Geosci. Model Dev. 11, 257–281. https://doi.org/10.5194/gmd-11-257-2018
- Wang, Y. et al. (2018): *27 years of regional cooperation for limited area modelling in Central Europe (RC LACE)*, Bull. Amer. Meteor. Soc. 99, 1415–1432. https://doi.org/10.1175/BAMS-D-16-0321.1
- Wattrelot, E., Caumont, O., Mahfouf, J.-F. (2014): *Operational implementation of the 1D+3D-Var assimilation method of radar reflectivity data in the AROME model*, Mon. Wea. Rev. 142, 1852–1873.
- Abel, S.J., Boutle, I.A. (2012): *An improved representation of the raindrop size distribution for single-moment microphysics schemes*, Q. J. R. Meteorol. Soc. 138, 2151–2162.
