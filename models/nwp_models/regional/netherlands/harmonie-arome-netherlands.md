# HARMONIE-AROME Netherlands (KNMI – Cy43 P1)

## What this model is
HARMONIE-AROME Netherlands is the **convection-permitting regional numerical weather prediction (NWP) model** run cooperatively by the **United Weather Centres-West (UWC-West)** partnership of KNMI (Netherlands), the Icelandic Met Office, DMI (Denmark), and Met Éireann (Ireland), and distributed publicly by KNMI through the KNMI Data Platform.

The dataset documented here — `harmonie_arome_cy43_p1` — is the **Dutch-domain** output of the same UWC-West model run that produces the [European DINI domain (P3)](./harmonie-knmi.md), packaged at near-native resolution on a regular lat-lon grid covering the Netherlands, Belgium, and surrounding North Sea waters. It is KNMI's primary public deterministic forecast product for the Netherlands and is optimised for short-range forecasting of small-scale and rapidly evolving weather such as showers, thunderstorms, fog, and local wind effects.

---

## Who runs it
- **Operating partnership:** United Weather Centres-West (UWC-West)
  - KNMI (Royal Netherlands Meteorological Institute)
  - Icelandic Met Office (Veðurstofa Íslands)
  - DMI (Danish Meteorological Institute)
  - Met Éireann (Ireland)
- **Public distributor (this dataset):** KNMI
- **Country / region:** Netherlands

---

## What area it covers
- **Coverage:** Netherlands, Belgium, and adjacent North Sea waters
- **Domain type:** Dutch-domain cutout of the larger UWC-West DINI integration
- **Distribution grid bounds:**
  - North: 56.002°N
  - South: 49.0°N
  - West: 0.0°E
  - East: 11.281°E
- **Grid dimensions:** 390 × 390 cells

---

## Basic details
- **Model type:** Regional deterministic NWP (non-hydrostatic, convection-permitting)
- **Model system:** HARMONIE-AROME
- **Model version:** Cycle 43 (Cy43); operational at UWC-West since 20 June 2024 after a one-year test period, replacing the deprecated KNMI-only Cy40 P1 dataset
- **Native horizontal resolution:** ~2 km
- **Public distribution grid (P1):** ~0.029° longitude × ~0.018° latitude on a **regular** lat-lon grid (bi-linear interpolation from the native Lambert conformal grid; ~2 km in both directions at Dutch latitudes — close to native resolution)
- **Vertical levels:** 90 (increased from 65 in the previous Cy40 configuration)
- **Forecast length:** Up to ~60 hours
- **Update frequency:** Hourly (1× per hour)
- **Temporal output resolution:** 1 hour
- **Run type:** Deterministic (P1)

---

## Model characteristics
P1 is a **near-surface and boundary-layer subset** (parameters up to ~300 m above ground level) of the same convection-permitting 2 km UWC-West model run that drives the European P3 dataset. Compared to P3, P1 trades geographic coverage for two practical advantages:
- a **regular** (rather than rotated) lat-lon grid, simplifying client-side processing and removing the need to handle wind rotation
- distribution at near-native resolution rather than at a regridded 0.05° spacing

It is convection-permitting and optimised for:
- convective precipitation and thunderstorms
- low clouds and fog
- local wind effects, including coastal and lake-effect circulations
- frontal precipitation over the Low Countries

The model runs are also distributed under several other dataset names that emphasise different variable subsets and use cases — including a deterministic aviation-focused product (`uwcw_extra_lv_ha43_nl_2km`) and ensemble products (P2a, P2b) on related grids.

---

## Data availability
- **Is the data free?** Yes
- **License:** Creative Commons Attribution 4.0 (CC BY 4.0); attribution to KNMI required
- **Data formats:** GRIB1
- **Data retention:** **Files have been retained on the KNMI Data Platform from 8 January 2026 onward** as a continually growing rolling archive. Per KNMI's announcement, this archive policy was introduced specifically for P1 due to its popularity and was **not applied retroactively** — runs prior to 8 January 2026 are not available. Other HARMONIE datasets (P3 Europe, BES Caribbean, P2/P4 ensembles) continue to use the standard 72-hour rolling-deletion policy. For older P1 archival data, contact the KNMI licensing office at `licentiebureau@knmi.nl` — this is a paid, personalised product.
- **Dataset landing page:**
  https://dataplatform.knmi.nl/dataset/harmonie-arome-cy43-p1-1-0

### How to access — KNMI Open Data API

KNMI distributes its open data exclusively through the **KNMI Data Platform Open Data API**. There is no anonymous FTP or open S3 bucket for HARMONIE; an API key is always required.

**API endpoint pattern:**

```
https://api.dataplatform.knmi.nl/open-data/v1/datasets/harmonie_arome_cy43_p1/versions/1.0/files
```

To download a specific file, list the dataset to discover filenames, then request a presigned download URL:

```
GET /open-data/v1/datasets/harmonie_arome_cy43_p1/versions/1.0/files
GET /open-data/v1/datasets/harmonie_arome_cy43_p1/versions/1.0/files/{filename}/url
```

The API key is passed in the `Authorization` HTTP header (no `Bearer` prefix).

### API key options

There are two flavours of API key, both free:

1. **Anonymous key** — published directly in the [Open Data API documentation](https://developer.dataplatform.knmi.nl/open-data-api). Shared across all anonymous users, rate-limited globally, and reissued annually with a fixed expiry date (the current key is valid until **1 July 2026**, after which a new key is published on the same page). Suitable for one-off downloads and exploration; not recommended for unattended scripts because it will silently stop working when it expires.

2. **Registered key** — obtained by creating a free account on the [KNMI Developer Portal](https://developer.dataplatform.knmi.nl/) and clicking *Request API Key* for the Open Data API. Tied to a verified email address, does **not** expire, and provides a private quota (currently **1,000 requests per hour**, increased from 200 in late 2022). Recommended for any automated or production use.

For full-dataset bulk downloads that exceed the standard quota, KNMI also issues **bulk keys** on request (email `opendata@knmi.nl` with subject "KDP complete dataset download" and the dataset name and version, using the same email address registered on the Developer Portal). The growing P1 archive in particular is well-suited to bulk download for research use cases.

### Update notifications
A separate **Notification Service** (also accessible via a Developer Portal API key) can push events when new files are published, removing the need for polling. Note that as of mid-2024 KNMI announced the Notification Service is being deprecated and is **not accepting new key requests**, while a replacement is being scoped — for new integrations, polling the Open Data API is currently the only supported pattern.

---

## Notes
- All KNMI HARMONIE Cy43 products (Netherlands P1, Europe P3, Caribbean BES, ensemble P2/P4, hybrid-level P5, etc.) come from the same UWC-West model run and differ only by domain, distributed grid, retained variable set, and (for P2/P4) deterministic vs ensemble output.
- The grid in P1 is **regular** lat-lon (in contrast to P3's rotated grid), so wind components are aligned with true geographic east/north and no rotation correction is needed when computing wind direction.
- GRIB files are encoded in GRIB **edition 1**, with parameters identified by `indicatorOfParameter` (not the GRIB2 category/number scheme). KNMI publishes a parameter code table for P1 at https://english.knmidata.nl/open-data/harmonie.
- Forecasts are issued **hourly**, but this does not mean the model itself is re-run every hour — it reflects the publication frequency of new forecast files from the operational UWC-West run cycle.
- P1's **growing retained archive from 8 January 2026** makes it materially more useful for historical research and verification work than the other KNMI HARMONIE products, which are limited to the most recent 72 hours.

---

## Official documentation
- KNMI Data Platform — dataset page:
  https://dataplatform.knmi.nl/dataset/harmonie-arome-cy43-p1-1-0
- KNMI Open Data API documentation:
  https://developer.dataplatform.knmi.nl/open-data-api
- KNMI Developer Portal:
  https://developer.dataplatform.knmi.nl/
- KNMI HARMONIE overview and parameter tables:
  https://english.knmidata.nl/open-data/harmonie
- UWC-West collaboration background (Icelandic Met Office):
  https://en.vedur.is/about-imo/news/new-icelandic-met-office-weather-and-climate-supercomputer-becomes-operational
