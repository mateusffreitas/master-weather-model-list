# <MODEL NAME>

## What this model is
<Plain-language description of the long-range forecast system: what it predicts and over what horizon. State up front whether it is sub-seasonal, seasonal, or interannual; whether it is a single coupled model or a multi-model ensemble; and whether output is deterministic, ensemble, or both. If it has a common short name or system identifier (e.g., CFSv2, SEAS5, CanSIPS), introduce it here.>

---

## Who runs it
- **Organization:** <Agency / operator>
- **Country / region:** <Country or multi-national org>
- **Coordinating body / programme (optional):** <e.g., NMME, C3S, SubX, WMO LC-LRFMME — for multi-model umbrellas or where the system feeds a larger framework>

---

## What area it covers
- **Coverage:** <Global / Region name> (long-range coupled systems are typically global)
- **Domain details (optional):** <bounds / delivered grid / notable notes>

---

## Basic details
- **Model type:** Long-range coupled forecast system — <deterministic / ensemble / both>; <single-model / multi-model>
- **Model system / core:** <e.g., CFSv2, GEM-NEMO, IFS coupled to NEMO, CanCM4i> (TBD)
- **Range class:** <Sub-seasonal (≈2–6 weeks) / Seasonal (1–9 months) / Interannual (up to ~13 months)>
- **Forecast length:** <e.g., 9 months / 46 days / 7 months, extended quarterly to 13 months>
- **Initialization cadence:** <e.g., monthly (1st of month) / 4× daily lagged / twice weekly (Mon & Thu)>
- **Ensemble generation:** <Burst (single start date) / Time-lagged (accumulated over consecutive starts) / Both> (TBD)
- **Ensemble size:** <real-time members; note if built by lagging, e.g., "24 = 4 runs/day × 6 days"> (TBD)
- **Temporal output resolution:** <e.g., 6-hourly / daily / monthly means> (TBD)
- **Output aggregation levels:** <which are actually distributed: sub-daily/daily raw fields, monthly statistics, anomalies, seasonal (3-month) means> (TBD)

---

## Coupled configuration
<Long-range systems are coupled Earth-system models — this is the defining difference from medium-range NWP. Fill what applies.>
- **Atmosphere:** <core/model + version; horizontal resolution; vertical levels; model top (optional)>
- **Ocean:** <model (e.g., NEMO, MOM6, HYCOM) + version; resolution; levels> (TBD)
- **Sea ice:** <model (e.g., CICE, SI3, LIM) + version> (TBD)
- **Land surface (optional):** <scheme/model> (TBD)
- **Coupling notes (optional):** <coupling frequency, anomaly vs full-field coupling, flux adjustment, etc.>

---

## Initialization
- **Atmosphere IC:** <analysis/reanalysis source> (TBD)
- **Ocean IC:** <ocean analysis/reanalysis, e.g., ORAS5, GODAS> (TBD)
- **Sea ice / land IC (optional):** <source> (TBD)
- **Perturbation method (optional):** <e.g., SST/atmosphere perturbations, singular vectors, stochastic physics, lagged starts> (TBD)

---

## Hindcasts (reforecasts)
<Integral to long-range systems: hindcasts define the model climatology used for anomalies, calibration, and skill estimates, and are often required to use the real-time forecast meaningfully.>
- **Hindcast period:** <e.g., 1993–2016 / 1991–2020> (TBD)
- **Hindcast cadence / ensemble size:** <e.g., same start dates, fewer members per start> (TBD)
- **Reference climatology period:** <period used for anomaly products, if stated separately> (TBD)
- **Distributed alongside forecasts?** <Yes / No / Separate dataset> (TBD)

---

## Sources of predictability (optional)
<Note the slow-varying drivers the system leans on, where documented: ENSO, MJO, stratospheric state, soil moisture, sea ice, ocean heat content. Useful context for skill expectations and for distinguishing this class from medium-range NWP. Omit if not relevant.>

---

## Multi-model composition (optional — multi-model umbrellas only)
<For systems like NMME, C3S seasonal, or SubX that combine several centres' models into one distributed product.>
- **Contributing systems / centres:** <list>
- **Combination method:** <pooled members on a common grid / weighted / simple multi-model ensemble>
- **Common delivered grid:** <e.g., 1° × 1°>
- **Per-contributor terms (optional):** <note if specific contributors carry extra licence conditions, e.g., non-European in-kind contributions to C3S>

---

## What it provides
Long-range outputs such as:
- ensemble member forecasts
- ensemble mean / spread
- anomalies relative to model climatology
- tercile probabilities (below / near / above normal) (optional, if distributed)
- climate indices (optional — e.g., Niño3.4, IOD, NAO, MJO/RMM)
- <add notable specialty variables or derived products if relevant>

---

## Data availability
- **Is the data free?** Yes / No / Partial
- **License:** <e.g., CC BY 4.0 / Public domain (U.S. government work; CC0-equivalent) / Open Government Licence – Canada / Copernicus licence (registration and licence acceptance required)> (note attribution or share-alike obligations if applicable; TBD)
- **Is the data downloadable?** Yes / No
- **Data formats:** <GRIB2 / NetCDF / both> (include compression notes if relevant)
- **Official download location:**  
  <URL>
- **Access route notes (optional):** <e.g., NOMADS rotating archive vs NCEI long-term archive; CDS API; IRI Data Library OpenDAP/subsetting; registration requirements>

---

## Notes
- <Single-model vs multi-model framing; relationship to any umbrella it feeds (NMME, C3S, SubX) or that it aggregates.>
- <Relationship to the operator's medium-range / NWP systems sharing the same core.>
- <If the public data is a subset of the full operational product, note it.>
- <Distinguish the raw distributed model output (in scope) from viewer-only official outlook products built on top of it, e.g., CPC seasonal outlooks (out of scope).>
- <Anything else: grid quirks, time-lagging conventions, embargoes, changes in ops.>
- <For AI-based or hybrid systems, note the AI approach here and update [`AI_MODELS.md`](../AI_MODELS.md).>

---

## Recent version history (optional)
<Include if the system has documented operational upgrades worth tracking — seasonal systems change model cycle, ocean component, and hindcast period often enough that this is frequently warranted (e.g., SEAS5 → SEAS6, CanSIPS versions). Otherwise omit.>

---

## Official documentation
- <URL>
- <URL>
