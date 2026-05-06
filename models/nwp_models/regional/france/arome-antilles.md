# AROME Antilles

## What this model is
AROME Antilles is the operational convection-permitting regional deterministic numerical weather prediction model run by Météo-France over the French Antilles and the surrounding Caribbean.

It is the Caribbean member of the wider **AROME Outre-Mer** suite — a family of tropical and equatorial AROME configurations covering France's overseas territories, internally referred to by Météo-France as the **PAROTRO** (AROME-Tropical) configuration. The system is designed for **short-range forecasting and nowcasting** of high-impact tropical weather, with particular strength in heavy rainfall, deep convection, tropical disturbances, and local wind effects in the Lesser Antilles island chain.

AROME Antilles shares the same dynamical core, physics packages, vertical grid, and distribution architecture as [AROME France](./arome-france.md), differing in domain, forecast range, and update cadence.

---

## Who runs it
- **Organization:** Météo-France
- **Country / region:** France (French Antilles)

---

## What area it covers
- **Coverage:** French Antilles (Guadeloupe, Martinique, Saint Martin, Saint-Barthélemy) and surrounding Caribbean region
- **Operational domain:** ANTIL0025 (also referred to as CARAIB0025 in some Météo-France channels)
  - Approximate bounds: **22.45°N – 10.4°N**, **67.8°W – 52.2°W**
  - Native grid is trapezoidal (not strictly rectangular)

---

## Basic details
- **Model type:** Regional deterministic NWP (non-hydrostatic, convection-permitting)
- **Model system:** AROME (spectral limited-area model, ALADIN-NH dynamical core); internal designation PAROTRO
- **Dynamical formulation:** Non-hydrostatic, spectral, with semi-Lagrangian advection and semi-implicit time integration
- **Convection-allowing:** Yes (deep convection explicitly resolved at 1.3 km native resolution; shallow convection parameterized)
- **Native horizontal resolution:** ~1.3 km (same native resolution as AROME France)
- **Public distribution grid:** 0.025° (~2.5 km) regular latitude–longitude
- **Vertical levels:** 90
- **Forecast length:** Up to 48 hours
- **Update frequency:** 4× daily (00, 06, 12, 18 UTC)
- **Temporal output resolution:** Hourly (48 separate forecast steps per cycle)
- **Lateral boundary conditions:** Provided by the parent [ARPEGE global model](../../global/france/arpege-global.md)

---

## What it provides
Deterministic forecasts distributed in **parameter group packages** following WMO GFNC naming conventions. Available packages:

**Surface fields**
- **SP1** — Mean sea-level pressure, 10 m wind (U, V, direction, speed, gusts), 2 m temperature and humidity, total cloud cover, precipitation, snow, downward shortwave flux, graupel
- **SP2** — Surface pressure and temperature, low/mid/high cloud cover, CAPE, boundary-layer height, total column water, 2 m min/max temperature, dewpoint, specific humidity
- **SP3** — Column water vapour, evaporation flux, latent and sensible heat fluxes, longwave and shortwave radiation fluxes (including clear-sky), surface stress (U, V), brightness temperatures at satellite channels 62 and 104

**Isobaric fields** (19 standard pressure levels, 100 to 1000 hPa unless otherwise noted)
- **IP1** — Temperature, humidity, wind (U, V), geopotential height
- **IP2** — Cloud water, cloud rain, cloud snow, cloud ice water content, cloud fraction
- **IP3** — Dewpoint, specific humidity, wind direction and speed, vertical velocity, vertical velocity squared, total precipitation
- **IP4** — Turbulent kinetic energy (10 levels), radar reflectivity (5 levels, 200 to 925 hPa)
- **IP5** — Tropopause temperature/pressure, equivalent potential temperature (16 levels), wind and geopotential at the 2000 m and 1500 m isentropic levels

**Height-above-ground fields** (14 levels, 20 to 3000 m unless otherwise noted)
- **HP1** — Temperature, humidity, wind (U, V, direction, speed), pressure, geopotential
- **HP2** — TKE, cloud water/rain/snow/fraction, cloud ice water content, dewpoint, specific humidity
- **HP3** — Radar reflectivity (7 levels: 500, 750, 1000, 1500, 2000, 2500, 3000 m)

---

## Data availability

AROME Antilles data is openly licensed under the Etalab Open Licence (version 2.0) and is available through the same distribution channels as the other Météo-France public NWP products. All channels carry GRIB2 files with `grid_ccsds` compression.

### 1. Météo-France public API (`public-api.meteofrance.fr`)
The official developer portal provides AROME Outre-Mer data through both bulk GRIB packages and an OGC Web Coverage Service (WCS) for single-variable subsetting.

- **Bulk packages:**
  `https://public-api.meteofrance.fr/previnum/DPPaquetAROMOM/v1/models/AROMOM/grids/0.025/zones/ANTIL/packages/{SP1|SP2|SP3|IP1|IP2|IP3|IP4|IP5|HP1|HP2|HP3}/productAROOM?...`
- **Authentication:** Free API key required. Register and manage keys at https://portail-api.meteofrance.fr
- **Documentation:** https://confluence-meteofrance.atlassian.net/wiki/spaces/OpenDataMeteoFrance/pages/621019138/

### 2. data.gouv.fr open-data portal (`object.data.gouv.fr`)
The French government's open-data S3-style object storage hosts the same GRIB packages with no authentication required.

- **URL pattern:**
  `https://object.data.gouv.fr/meteofrance-pnt/pnt/{run}/aromeom/0025/ANTIL/{package}/aromeom__0025__ANTIL__{package}__{packageTime}__{run}.grib2`
  where `{run}` is in ISO-8601 form (e.g. `2026-05-01T00:00:00Z`).
- **Dataset landing page:** https://www.data.gouv.fr/datasets/paquets-arome-outre-mer-antilles-resolution-0-025deg
- **Authentication:** None
- **Use case:** Best for users who want unauthenticated bulk access without registering for an API key.

### 3. Community AWS mirror (`mf-models-on-aws.s3.amazonaws.com`)
A community-run mirror, not affiliated with Météo-France, that redistributes Météo-France data on AWS. Publicly listed in the AWS Open Data Registry.

- **AWS Open Data Registry listing:** https://registry.opendata.aws/meteo-france-models/
- **Documentation:** https://mf-models-on-aws.org/en/doc/datasets/v1/
- **Authentication:** None
- **Caveat:** Operated independently and not subject to Météo-France's operational change-management process. Coverage of AROME Outre-Mer domains on the mirror should be verified against the mirror's own documentation, as not all Météo-France products are mirrored.

### Summary
- **Is the data free?** Yes (Etalab Open Licence v2.0)
- **Is the data downloadable?** Yes
- **Data formats:** GRIB2 (compressed, `grid_ccsds`)

---

## Related AROME systems

AROME Antilles is one of five tropical/equatorial domains in the **AROME Outre-Mer** suite operated by Météo-France:

- **AROME Antilles** (this entry) — Caribbean / French Antilles (ANTIL0025)
- **[AROME French Guiana](./arome-french-guiana.md)** — northern South America (GUYANE0025)
- **[AROME New Caledonia](./arome-new-caledonia.md)** — southwest Pacific (NCALED0025)
- **[AROME French Polynesia](./arome-polynesia.md)** — South Pacific (POLYN0025)
- **[AROME Réunion–Mayotte](./arome-reunion-mayotte.md)** — south-western Indian Ocean (INDIEN0025)

All five Outre-Mer configurations share the same underlying PAROTRO model, 1.3 km native resolution, 90 vertical levels, 0.025° public distribution grid, 48-hour forecast length, and hourly output. They differ in domain and update cycles. The metropolitan [AROME France](./arome-france.md) configuration uses a different domain (EURW1S40), longer forecast range (51 hours), and faster update cadence (8× daily).

A companion ensemble system, **PEAROME-OM** (Prévision d'Ensemble AROME Outre-Mer), has been operational since 21 February 2023 and provides 16-member probabilistic guidance (1 control + 15 perturbed members, run at 2.5 km native rather than 1.3 km) for the Antilles, French Guiana, Réunion, New Caledonia, and Polynesia domains. PEAROME-OM is documented separately under ensemble models.

---

## Notes
- The native AROME grid is trapezoidal; GRIB files distributed on the regular latitude–longitude grid may contain **missing values** outside the effective domain. Users may need to crop or mask data when post-processing.
- The native horizontal resolution of 1.3 km is finer than the public distribution grid of 0.025° (~2.5 km). Forecasts on the open-data servers are interpolated from the native grid.
- AROME Outre-Mer configurations operate in environments very different from metropolitan France (deep tropical convection, weak synoptic forcing, large maritime fetches), and forecast skill characteristics differ accordingly.
- The same domain is referred to inconsistently in Météo-France documentation and distribution channels as **ANTIL0025** (operational grid identifier in the technical PDF), **CARAIB0025** (used in some catalog metadata), and **CARAïBES** (used as the zone label on data.gouv.fr); all refer to the same model output. Note that the data.gouv.fr dataset description page lists a broader bounding box (9.7°N–22.9°N, 75.3°W–51.7°W) than the operational grid published in the technical PDF (10.4°N–22.45°N, 67.8°W–52.2°W); the operational grid is the authoritative source.
- Lateral boundary conditions come from the parent [ARPEGE](../../global/france/arpege-global.md) global model.

---

## Official documentation
- AROME Outre-Mer technical description (PDF, Météo-France, version 02/01/2024): https://donneespubliques.meteofrance.fr/client/documentation/description-paquets-modele-aromeom.pdf
- PEAROME-OM technical description (PDF, Météo-France, version 30/10/2025): linked from the Météo-France public data portal
- Météo-France public API documentation: https://portail-api.meteofrance.fr
- API Paquets Modèles overview (Confluence): https://confluence-meteofrance.atlassian.net/wiki/spaces/OpenDataMeteoFrance/pages/621019138/
- data.gouv.fr Antilles dataset page: https://www.data.gouv.fr/datasets/paquets-arome-outre-mer-antilles-resolution-0-025deg
- Météo-France public data portal: https://donneespubliques.meteofrance.fr
- AWS Open Data Registry — Atmospheric Models from Météo-France: https://registry.opendata.aws/meteo-france-models/
