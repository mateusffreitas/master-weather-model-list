# <MODEL NAME>

## What this model is
<One or two plain-language sentences describing what the model is and what it's used for.>

---

## Who runs it
- **Organization:** <Agency / operator>
- **Country / region:** <Country or multi-national org>

---

## What area it covers
- **Coverage:** <Global / Region name>
- **Domain details (optional):** <bounds / grid name / notable notes>

---

## Basic details
- **Model type:** Deterministic NWP
- **Model system / core:** <e.g., ICON, IFS, GEM, WRF, HARMONIE-AROME, COSMO>
- **Dynamical formulation:** <Hydrostatic / Non-hydrostatic> (TBD)
- **Convection-allowing:** <Yes / No> (typically Yes when horizontal resolution is ≤ ~4 km; TBD)
- **Horizontal resolution:** <e.g., ~13 km / 0.25° / ~2.5 km> (native vs published grid if different)
- **Grid dimensions (optional):** <e.g., 1024 × 768 grid points> (may instead be noted under "What area it covers"; TBD)
- **Vertical levels:** <e.g., 120> (TBD if unknown)
- **Model top (optional):** <e.g., ~75 km> (TBD)
- **Forecast length:** <e.g., up to 240 h / 10 days> (mention per-cycle differences if applicable)
- **Update frequency / cycles:** <e.g., 4× daily (00/06/12/18 UTC)>
- **Temporal output resolution:** <e.g., 1h / 3h> (TBD)

---

## Data assimilation (optional)
- **Data assimilation:** <Yes/No> (TBD)
- **Method / cadence (optional):** <e.g., 3D-Var every 3h> (TBD)

---

## Initial and boundary conditions (for limited-area models)
- **Initial conditions:** <e.g., IFS / GFS / ICON Global> (TBD)
- **Boundary conditions:** <source + update frequency> (TBD)

---

## What it provides
Deterministic forecasts of:
- <temperature, wind, precipitation, pressure, humidity, clouds, etc.>
- <add notable specialty variables if relevant>

---

## Data availability
- **Is the data free?** Yes / No / Partial
- **License:** <e.g., CC BY 4.0 / CC BY-SA 4.0 / Etalab Open Licence / Public domain (U.S. government work; CC0-equivalent) / KNMI Open Data licence / Copernicus Marine Service licence> (note attribution or share-alike obligations if applicable; TBD)
- **Is the data downloadable?** Yes / No
- **Data formats:** <GRIB2 / NetCDF / both> (include compression notes if relevant)
- **Official download location:**  
  <URL>

---

## Notes
- <Anything important: subsets, grid quirks, trapezoidal domains, changes in ops, public-vs-full-ops differences, etc.>
- <Relationship to siblings: ensemble counterpart, regional nest, parent global model.>
- <For AI-based or hybrid systems, note the AI approach here and update [`AI_MODELS.md`](../AI_MODELS.md).>

---

## Recent version history (optional)
<Include this section if the model has documented operational upgrades worth tracking. See `nbm.md`, `rrfs.md`, `raqdps.md`, `ukmo-global.md` for examples. Otherwise omit.>

---

## Official documentation
- <URL>
- <URL>
