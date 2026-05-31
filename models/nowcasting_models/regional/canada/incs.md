# INCS (Integrated Nowcasting System)

## What this model is
The Integrated Nowcasting System (INCS) is Environment and Climate Change Canada's operational very-short-range nowcasting system. It produces a consistent, internally coherent set of observed and forecast surface weather elements — from the observation time out to the next 12 hours — for over 450 forecast points across Canada, generally corresponding to observation stations. INCS is the engine behind the Scribe "nowcasting matrices": it blends real-time observations (surface METAR/SPECI and synoptic reports, weather radar, lightning, and satellite) with statistical guidance and NWP so that the first forecast hours mesh with the most recent observed conditions before transitioning toward model-driven values.

Internally, INCS is a point (station-based) nowcast system. It also generates gridded radar and lightning fields (the "comporad" composite and the radar Surface Precipitation Type product) by extrapolating observed radar/lightning forward along a motion vector, but those gridded fields are used internally; the publicly distributed product is the per-station Scribe nowcasting matrix.

---

## Who runs it
- **Organization:** Canadian Meteorological Centre (CMC) / Environment and Climate Change Canada
- **Country / region:** Canada

---

## What area it covers
- **Coverage:** Canada (land points corresponding to observation stations)
- **Domain details:** Over 450 forecast points distributed across Canada, generally co-located with surface observation stations. A station list is published in GeoJSON format. (The MSC readme cites "over 500" points and the Datamart page "over 450"; the station-list GeoJSON is authoritative for the current count.)

---

## Basic details
- **System type:** Nowcasting
- **Nowcasting method:** Seamless blend of observations, statistical guidance, and NWP at point locations (point nowcast). Radar/lightning fields are produced internally by observation extrapolation along a motion vector.
- **Technique / algorithm:** Hourly synthesis of observations + UMOS statistical post-processing + NWP into a "master matrix"; for the internal radar/lightning fields, a displacement (motion) vector is built from a sequence of 5 radar images (or from NWP winds when radar is unavailable) and used to extrapolate the radar composite and lightning forward
- **Underlying / driving model:** GDPS (Global Deterministic Prediction System, G0 run over North America) and HRDPS for the radar Surface Precipitation Type composite; UMOS-GDPS for statistical guidance. (Prior to IC-4 in June 2024 the driving NWP was the RDPS — see Version history.)
- **Probabilistic / ensemble:** Partial — the matrices carry probability of precipitation and per-type precipitation probabilities, but the system is deterministic (not an ensemble)
- **Horizontal resolution:** Not gridded in the public product (point/station output). Internal radar composites are produced at 1 km; the internal comporad field is on the HRDPS national 2.5 km grid.
- **Vertical structure:** Surface-level weather elements only
- **Lead time:** 0–12 h (each matrix also includes ~6 h of preceding observations plus the run-time observation, for 19 hourly rows total)
- **Update frequency:** Hourly
- **Temporal output resolution:** 1 h
- **Latency:** TBD

---

## Inputs
- **Radar:** Canadian and U.S. S-band/NEXRAD network (~180 contributing radars); dual-polarization QPE and particle classification, composited to 1 km. INCS internally ingests radar at 10-minute intervals (selecting the nearest 6-minute file).
- **Satellite:** GOES (GOES-16 East / GOES-18 West) channel data, used with NWP to derive a Cloud Fraction cloud mask
- **Lightning:** Canadian Lightning Detection Network (CLDN UALF2 feed)
- **Surface / other observations:** Hourly surface observations (METAR, SPECI) and synoptic reports
- **NWP fields:** GDPS (and HRDPS for the Canadian-domain radar precipitation-type composite); UMOS-GDPS statistical post-processing

---

## What it provides
Per-station hourly nowcast matrices ("Scribe nowcasting matrices") containing observed and forecast surface weather elements:
- precipitation type (multiple simultaneous types supported, with intensity) and per-type probability of occurrence
- total probability of precipitation (POP)
- precipitation amount (mm/hr water, cm/hr snow) and corresponding type
- temperature and dew point
- wind direction, speed, and gust (km/h)
- sky cover (tenths) and ceiling height (hundreds of feet; flagged in the docs as not reliable for aviation)
- visibility and obstacle-to-visibility type (fog, mist, blowing snow, haze, etc.)

INCS output also feeds the aTAGS (autoTAF Automatic Guidance System) aviation guidance product downstream.

---

## Data availability
- **Is the data free?** Yes
- **License:** Environment and Climate Change Canada end-user licence (MSC Open Data)
- **Is the data downloadable?** Yes
- **Data formats:** Compressed ASCII text matrices (`.Z`). [Verify: the Datamart prose calls the files "GRIB2," but the file nomenclature, worked example, and `.Z` compression all indicate ASCII matrices — the GRIB2 mention appears to be boilerplate. Confirm against a downloaded file.]
- **File naming:** `SCRIBE.NWCSTG.MM.DD.HHZ.n.Z` (e.g. `SCRIBE.NWCSTG.10.24.06Z.n.Z`)
- **Official download location:**
  https://dd.weather.gc.ca/today/nowcasting/matrices/
- **Delivery mechanism:** MSC Datamart over HTTPS; also retrievable in real time via AMQP. A station list is available in GeoJSON.

---

## Notes
- INCS is a **point/station** nowcast system, which is unusual for this category — most nowcasting products are gridded. Its internally produced gridded radar/lightning extrapolation fields are not part of the public distribution; only the per-station matrices are published.
- The ceiling-height field is included for technical continuity and is explicitly flagged by ECCC as unreliable, particularly for aviation use.
- A forecast is always produced for a station even when its surface observations are incomplete or missing, falling back on remote-sensing (radar, satellite, lightning) and ultimately NWP weather elements.
- **Successor in development:** ECCC is developing a gridded nowcasting system — "Nowcast of Weather Elements on Grid" (NCWEonG / WEonG) — intended to complement or eventually replace the point-based INCS once its components are ready. Worth tracking in `STATUS.md`.

---

## Recent version history
### Version 2.3.0 — 11 June 2024 (IC-4)
- Driving NWP switched from RDPS to GDPS (G0 run over a North American domain) following the retirement of the RDPS as a standalone regional system under CMC's fourth innovation cycle (IC-4)
- GOES-derived Cloud Fraction mask recomputed using GDPS (rather than RDPS) data
- Radar Surface Precipitation Type (SPTP) composite adapted to IC-4 versions of GDPS (American domain) and HRDPS (Canadian domain)
- UMOS-GDPS statistical guidance replaced UMOS-RDPS
- System re-packaged into a self-contained "maestro" component; added robustness in computing the motion vector from NWP winds when radar data is missing
- Evaluated over 20 March – 22 May 2024; impacts of the RDPS→GDPS change found to be very small (a slight cold bias in 2 m dew point noted, within expected range)

### Earlier
- v2.1.0 — internal comporad composite moved to the HRDPS national 2.5 km grid
- v2.0.0 (4 Jan 2023) — GOES-18 introduced as GOES West for the Cloud Fraction mask
- A full chronology is maintained in the ECCC nowcasting changelog (see Official documentation).

---

## Official documentation
- Product page (MSC Open Data): https://eccc-msc.github.io/open-data/msc-data/nwp_nowcasting/readme_nowcasting_en/
- Datamart data/format documentation: https://eccc-msc.github.io/open-data/msc-data/nwp_nowcasting/readme_nowcasting-datamart_en/
- Changelog: https://eccc-msc.github.io/open-data/msc-data/nwp_nowcasting/changelog_nowcasting_en/
- Technical Note (INCS v2.3.0): https://collaboration.cmc.ec.gc.ca/cmc/cmoi/product_guide/docs/tech_notes/technote_rdps-incs_e.pdf
- Scientific review (SCRIBE 3.0): https://collaboration.cmc.ec.gc.ca/cmc/cmoi/product_guide/docs/lib/e_scribe3.pdf
