# UKV (UK Variable Resolution Model)

## What this model is
UKV is the **high-resolution regional deterministic weather prediction system** operated by the UK Met Office.

It is a **regional configuration of the Unified Model (UM)** — the Met Office's flagship NWP and climate modelling suite — covering the UK and Ireland. UKV is named for its variable-resolution grid: a fixed-resolution inner domain at 1.5 km grid spacing covers the UK and Ireland, surrounded by a coarser 4 km grid near the lateral boundaries with a smooth variable-resolution transition zone in between. This design lets UKV run at convection-permitting scales over the area of forecast interest while limiting boundary-related artefacts from the coarser parent [UKMO Global](../../global/uk/ukmo-global.md) model that drives it.

UKV is the Met Office's primary short-range, convection-permitting forecast system for the UK and is used operationally for severe weather, aviation, energy, and public forecasting applications. It is also the deterministic counterpart to the [MOGREPS-UK](../../../ensemble_models/regional/uk/mogreps-uk.md) regional ensemble, which shares the same domain and model core.

The current operational version is part of **Operational Suite 47 (OS47)**, implemented on 21 January 2026 — the Met Office's first major science upgrade in over three years and the first run on its new Microsoft Azure-based supercomputer.

---

## Who runs it
- **Organization:** Met Office
- **Country / region:** United Kingdom

---

## What area it covers
- **Coverage:** United Kingdom and Ireland
- **Domain details:**
  - Variable-resolution grid with a 1.5 km fixed-resolution inner domain over the UK and Ireland
  - 4 km grid spacing near the lateral boundaries
  - Smooth variable-resolution transition zone between the inner and boundary regions
  - Lateral boundary conditions provided by the parent [UKMO Global](../../global/uk/ukmo-global.md) model

---

## Basic details
- **Model type:** Regional deterministic NWP
- **Model system / core:** Unified Model (UM), in its Regional Atmosphere/Land 3 (RAL3) science configuration as of OS47
- **Dynamical formulation:** Non-hydrostatic, fully compressible deep-atmosphere equations (ENDGame dynamical core); semi-Lagrangian, semi-implicit time integration
- **Convection-allowing:** Yes (deep convection is explicitly resolved at 1.5 km in the inner domain)
- **Native horizontal resolution:** 1.5 km (inner domain), 4 km (boundary region), variable in transition zone
- **Public distribution grid:** 0.018° (~2 km) regular grid, projected from native
- **Forecast length:**
  - Extended forecasts to 120 hours from selected cycles (03 and 15 UTC)
  - Up to 54 hours from the remaining cycles
- **Update frequency:** 8× daily (00, 03, 06, 09, 12, 15, 18, 21 UTC)
- **Data latency:** Typically 3–6 hours after model run time

---

## Data assimilation
UKV uses **hourly cycling 4D-Var** data assimilation, providing high-frequency analysis updates well-suited to the convection-permitting scale of the inner domain. The Met Office's broader hybrid 4D-Var infrastructure underpins the analysis system, with global-scale information flowing in via the UKMO Global lateral boundary conditions.

OS47 introduced the **JOPA** (Joint Observation Processing Approach) code for observational data processing in the global coupled and marine-only models — the first operational deployment of components from the Met Office's Next Generation Modelling System (NGMS). JOPA adoption for UKV is staged separately and is expected in subsequent operational suites.

---

## What it provides
Convection-permitting deterministic forecasts of:
- Temperature (surface, screen-level, and upper-air)
- Wind components (10 m and upper-air on pressure and height levels)
- Specific and relative humidity
- Surface and mean sea-level pressure
- Total precipitation, precipitation rate, and precipitation type
- Cloud cover (total, high, mid, low) and cloud base / ceiling
- Convective indices and explicitly-resolved storm structure
- Visibility, wind gusts, and aviation-relevant fields
- Surface fluxes, radiation components, and boundary-layer diagnostics

UKV is particularly relied upon for short-range forecasts of small-scale and rapidly evolving weather such as showers, thunderstorms, fog, and local wind effects driven by complex coastlines and orography.

---

## Data availability

### Free Open Data — AWS Open Data Registry
- **Free?** Yes
- **License:** Creative Commons Attribution-ShareAlike 4.0 (CC BY-SA 4.0); attribution and ShareAlike required (note: this differs from ECMWF's CC-BY-4.0 by adding the ShareAlike obligation)
- **Resolution:** ~2 km (0.018°)
- **Format:** NetCDF (CF-compliant)
- **Retention:** **2-year rolling archive** — significantly longer than typical operational open-data services, which usually retain only the most recent few days. The archive contains data from approximately late 2023 onward.
- **Official location:**
  - https://registry.opendata.aws/met-office-uk-deterministic/

### Microsoft Planetary Computer
A second, parallel distribution of the same data was launched on the Microsoft Planetary Computer in February 2026, alongside the equivalent UKMO Global release:
- **Near-surface collection:** https://planetarycomputer.microsoft.com/dataset/met-office-uk-deterministic-near-surface
- **Height levels collection:** https://planetarycomputer.microsoft.com/dataset/met-office-uk-deterministic-height
- **Pressure levels collection:** https://planetarycomputer.microsoft.com/dataset/met-office-uk-deterministic-pressure
- **Whole atmosphere collection:** https://planetarycomputer.microsoft.com/dataset/met-office-uk-deterministic-whole-atmosphere
- **Dataset group landing page:** https://planetarycomputer.microsoft.com/dataset/group/met-office-uk-deterministic
- The Planetary Computer release includes a 2-year historical archive (>600 TB combined with the UKMO Global dataset), pitched at researchers training and evaluating AI weather models against authoritative high-resolution forecasts.

### Caveats for both channels
The data is offered on a free, **unsupported** basis — the Met Office does not recommend either distribution for critical business purposes. Service-desk support is available Mon-Fri 09:00-17:00 UTC with a 3-5 business day target response time, and only for non-operational queries.

---

## Notes
- UKV's native 1.5 km inner-domain resolution is finer than the public 0.018° (~2 km) distribution grid. Forecasts on the open-data servers are projected from the native variable-resolution grid; users requiring full native resolution should consult the Met Office about commercial/research data services.
- The Met Office uses a science configuration scheme for the UM: UK NWP systems use **Regional Atmosphere/Land 3 (RAL3)** as of OS47 (Bush et al., 2025), while global systems use **Global Coupled 5 (GC5)**. RAL3 was operationalised with OS47 in January 2026, replacing the previous regional configuration that had been in use under OS46.
- UKV runs on the Met Office's **new Microsoft Azure-based supercomputer**, which became operational in 2024 and on which OS47 was the first major modelling suite to run.
- Defence Regional Models and Crisis Area Models — used by the Met Office to support military operations and disaster relief — share the same RAL3 science configuration as UKV but can be deployed rapidly to other domains. These are not part of the open-data UKV distribution.
- The asymmetric forecast length (extended to 120 h at the 03 and 15 UTC cycles, 54 h at the others) gives UKV both a short-range update-heavy schedule and a medium-range capability via the extended cycles. Most users consume the 54-hour cycles.
- The companion ensemble system is [MOGREPS-UK](../../../ensemble_models/regional/uk/mogreps-uk.md), which shares UKV's domain and dynamical core.

---

## Recent version history

### OS47 / PS47 / RAL3 — operational 21 January 2026 (current)
First major science upgrade in over three years, and the first running on the Met Office's new Microsoft Azure supercomputer. Highlights for UKV:
- New **RAL3** science configuration replacing the previous regional configuration used under OS46
- **Improved UK cloud forecasts (including fog)**, with measurable benefits for aviation
- **Improved UK winter temperature forecasts**, benefiting energy and gritting/de-icing applications
- Better modelling of light rain in fronts, and improved cloud base heights
- Introduction of **CASIM cloud microphysics** — note that cloud ice water content is now calculated differently and now requires the sum of two STASH fields (0-012 and 0-271) rather than the single legacy field; this is handled transparently in StaGE for users consuming gridded products via Weather DataHub
- Companion change: MOGREPS-UK regional ensemble upgraded under the same OS47 release
- Open-data product changes: the `height_asl_on_pressure_levels` parameter was replaced by `geopotential_height_on_pressure_levels`; precision changes; new parameters; new vertical levels and timesteps for some fields
- Operational data availability is approximately 10–20 minutes later than under OS46

### OS46 — operational May 2022 to January 2026
Previous operational configuration, succeeded by OS47.

### Earlier history
The Unified Model entered Met Office operational service in 1991 as the world's first NWP model designed to span weather-and-climate timescales with a single codebase. The current ENDGame dynamical core replaced the older "New Dynamics" formulation in 2014. UKV's variable-resolution design with a 1.5 km inner domain was introduced as part of the Met Office's transition to convection-permitting operational forecasting in the 2010s, replacing earlier 4 km UK-area configurations.

---

## Official documentation
- Met Office numerical weather prediction overview: https://www.metoffice.gov.uk/research/approach/modelling-systems/unified-model/weather-forecasting
- Parallel Suite 47 (PS47) overview: https://www.metoffice.gov.uk/services/data/parallel-suite-47-ps47-overview
- Met Office Weather DataHub upcoming changes: https://datahub.metoffice.gov.uk/support/changes-and-updates
- Met Office services data pages: https://www.metoffice.gov.uk/services/data
- AWS Open Data Registry: https://registry.opendata.aws/met-office-uk-deterministic/
- Microsoft Planetary Computer dataset group: https://planetarycomputer.microsoft.com/dataset/group/met-office-uk-deterministic
- CEDA UKV dataset record: https://catalogue.ceda.ac.uk/uuid/f47bc62786394626b665e23b658d385f/

### Key references
- Bush, M., et al. (2025). *The Met Office Unified Model Regional Atmosphere 3 and JULES Regional Land 3 configurations.* (RAL3 documentation reference cited in the Met Office NWP overview.)
- Wood et al. (2014). *An inherently mass-conserving semi-implicit semi-Lagrangian discretization of the deep-atmosphere global non-hydrostatic equations.* QJRMS, 140, 1505–1520. (Documents the ENDGame dynamical core)
- Tang et al. (2013). *The benefits of the Met Office variable resolution NWP model for forecasting convection.* Meteorological Applications, 20, 417–426. (Documents the UKV variable-resolution design)
