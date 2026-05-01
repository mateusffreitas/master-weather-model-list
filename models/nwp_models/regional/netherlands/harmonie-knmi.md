# HARMONIE-AROME Europe / DINI (KNMI – Cy43 P3)

## What this model is
HARMONIE-AROME Europe is the **convection-permitting regional numerical weather prediction (NWP) model** run cooperatively by the **United Weather Centres-West (UWC-West)** partnership of KNMI (Netherlands), the Icelandic Met Office, DMI (Denmark), and Met Éireann (Ireland), and distributed publicly by KNMI through the KNMI Data Platform.

The dataset documented here — `harmonie_arome_cy43_p3` — is the European-scale **DINI domain** (**D**enmark–**I**celand–**N**etherlands–**I**reland) output, providing near-surface and boundary-layer parameters and a small set of pressure levels on a rotated lat-lon grid. The same underlying model run is also available over the smaller Dutch domain (P1) and Caribbean domain (BES); see the related KNMI HARMONIE entries.

---

## Who runs it
- **Operating partnership:** United Weather Centres-West (UWC-West)
  - KNMI (Royal Netherlands Meteorological Institute)
  - Icelandic Met Office (Veðurstofa Íslands)
  - DMI (Danish Meteorological Institute)
  - Met Éireann (Ireland)
- **Public distributor (this dataset):** KNMI
- **Country / region:** Netherlands (distribution); multi-national (operations)

---

## What area it covers
- **Coverage:** Large parts of north-western and central Europe, extending from East Greenland to southern Italy
- **Domain name:** DINI (Denmark–Iceland–Netherlands–Ireland)
- **Distribution grid bounds:**
  - North: 62.6°N
  - South: 38.75°N
  - West: 25.0°W
  - East: 16.0°E

---

## Basic details
- **Model type:** Regional deterministic NWP (non-hydrostatic, convection-permitting)
- **Model system:** HARMONIE-AROME
- **Model version:** Cycle 43 (Cy43); operational at UWC-West since 20 June 2024 after a one-year test period, replacing the deprecated KNMI-only Cy40 P3 dataset
- **Native horizontal resolution:** ~2 km
- **Public distribution grid (P3):** ~0.05° on a **rotated** lat-lon grid (bi-linear interpolation from the native Lambert conformal grid)
- **Vertical levels:** 90 (increased from 65 in the previous Cy40 configuration)
- **Forecast length:** Up to ~60 hours
- **Update frequency:** Hourly (1× per hour)
- **Temporal output resolution:** 1 hour
- **Run type:** Deterministic (P3)

---

## Model characteristics
The dataset name "Europe" is somewhat misleading: P3 is a **near-surface, boundary-layer, and limited-pressure-level subset** extracted from the same convection-permitting 2 km model that drives the Dutch P1 dataset. It is suitable for:
- frontal precipitation and convective systems over Northwest Europe
- boundary-layer-relevant fields (wind, temperature, humidity up to ~300 m)
- regional pressure and temperature patterns
- a small set of standard pressure levels

The full model output (including upper-level, hybrid-level, ensemble, and renewable-energy-focused variables) is distributed under separate dataset names (P5, P4a/b, P2a/b, etc.).

---

## Data availability
- **Is the data free?** Yes
- **License:** Creative Commons Attribution 4.0 (CC BY 4.0); attribution to KNMI required
- **Data formats:** GRIB1
- **Data retention:** **Only the most recent 72 hours** are kept on the KNMI Data Platform. There is no rolling archive for P3 (unlike P1, which began retaining files from 8 January 2026 onward). For historical runs, contact the KNMI licensing office at `licentiebureau@knmi.nl` — this is a paid, personalised product.
- **Dataset landing page:**
  https://dataplatform.knmi.nl/dataset/harmonie-arome-cy43-p3-1-0

### How to access — KNMI Open Data API

KNMI distributes its open data exclusively through the **KNMI Data Platform Open Data API**. There is no anonymous FTP or open S3 bucket for HARMONIE; an API key is always required.

**API endpoint pattern:**

```
https://api.dataplatform.knmi.nl/open-data/v1/datasets/harmonie_arome_cy43_p3/versions/1.0/files
```

To download a specific file, list the dataset to discover filenames, then request a presigned download URL:

```
GET /open-data/v1/datasets/harmonie_arome_cy43_p3/versions/1.0/files
GET /open-data/v1/datasets/harmonie_arome_cy43_p3/versions/1.0/files/{filename}/url
```

The API key is passed in the `Authorization` HTTP header (no `Bearer` prefix).

### API key options

There are two flavours of API key, both free:

1. **Anonymous key** — published directly in the [Open Data API documentation](https://developer.dataplatform.knmi.nl/open-data-api). Shared across all anonymous users, rate-limited globally, and reissued annually with a fixed expiry date (the current key is valid until **1 July 2026**, after which a new key is published on the same page). Suitable for one-off downloads and exploration; not recommended for unattended scripts because it will silently stop working when it expires.

2. **Registered key** — obtained by creating a free account on the [KNMI Developer Portal](https://developer.dataplatform.knmi.nl/) and clicking *Request API Key* for the Open Data API. Tied to a verified email address, does **not** expire, and provides a private quota (currently **1,000 requests per hour**, increased from 200 in late 2022). Recommended for any automated or production use.

For full-dataset bulk downloads that exceed the standard quota, KNMI also issues **bulk keys** on request (email `opendata@knmi.nl` with subject "KDP complete dataset download" and the dataset name and version, using the same email address registered on the Developer Portal).

### Update notifications
A separate **Notification Service** (also accessible via a Developer Portal API key) can push events when new files are published, removing the need for polling. Note that as of mid-2024 KNMI announced the Notification Service is being deprecated and is **not accepting new key requests**, while a replacement is being scoped — for new integrations, polling the Open Data API is currently the only supported pattern.

---

## Notes
- All KNMI HARMONIE Cy43 products (Netherlands P1, Europe P3, Caribbean BES, ensemble P2/P4, hybrid-level P5, etc.) come from the same UWC-West model run and differ only by domain, distributed grid, retained variable set, and (for P2/P4) deterministic vs ensemble output.
- The grid in P3 is **rotated** lat-lon — wind components are defined relative to the rotated grid axes, not true north. Software that ingests these files needs to be aware of this when computing geographic wind direction.
- GRIB files are encoded in GRIB **edition 1**, with parameters identified by `indicatorOfParameter` (not the GRIB2 category/number scheme). KNMI publishes a parameter code table for P3 at https://english.knmidata.nl/open-data/harmonie.
- The DINI domain is the operational European backbone for all four UWC-West partners; DMI in particular distributes its own subset of the same domain as the [HARMONIE-AROME (DMI) DINI dataset](../denmark/harmonie-dmi.md). The KNMI P3 distribution and the DMI DINI distribution are sourced from the same model run but are packaged independently.

---

## Official documentation
- KNMI Data Platform — dataset page:
  https://dataplatform.knmi.nl/dataset/harmonie-arome-cy43-p3-1-0
- KNMI Open Data API documentation:
  https://developer.dataplatform.knmi.nl/open-data-api
- KNMI Developer Portal:
  https://developer.dataplatform.knmi.nl/
- KNMI HARMONIE overview and parameter tables:
  https://english.knmidata.nl/open-data/harmonie
- UWC-West collaboration background (Icelandic Met Office):
  https://en.vedur.is/about-imo/news/new-icelandic-met-office-weather-and-climate-supercomputer-becomes-operational
