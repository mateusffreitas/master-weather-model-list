# WRF-SMN Argentina

## What this model is
WRF-SMN Argentina is a high-resolution, convection-permitting regional deterministic numerical weather prediction (NWP) model operated by Argentina's national weather service for short-range forecasting over Argentina and surrounding parts of South America.

It is based on the **WRF-ARW (Advanced Research WRF)** dynamical core. SMN's documentation describes the operational version as WRF 4.0 (with the dynamical core as documented in Skamarock et al. 2019).

---

## Who runs it
- **Organization:** Servicio Meteorológico Nacional (SMN-Arg)
- **Country / region:** Argentina

---

## What area it covers
- **Coverage:** Argentina, Chile, Uruguay, Paraguay, and parts of Bolivia and Brazil, plus adjacent oceans
- **Map projection:** Lambert conformal
- **Grid center:** ~35°S, 65°W
- **Grid dimensions:** 999 × 1249 grid points

---

## Basic details
- **Model type:** Regional deterministic NWP
- **Model system / core:** WRF-ARW (version 4.0 per SMN documentation)
- **Dynamical formulation:** Non-hydrostatic
- **Convection-allowing:** Yes (4 km grid; cumulus parameterization disabled, deep convection explicitly resolved)
- **Horizontal resolution:** ~4 km
- **Vertical levels:** 45
- **Model top:** ~10 hPa
- **Forecast length:** Up to 72 hours
- **Update frequency / cycles:** 4× daily (00, 06, 12, 18 UTC), matching the file-naming convention in SMN's data-structure documentation and confirmed by inspection of the public S3 bucket. (The dataset description on the AWS Open Data registry currently says "two forecast cycles a day initialized at 00 UTC and 12 UTC" — that text appears to be stale relative to the actual published data.)
- **Temporal output resolution:** 10-minute precipitation, hourly surface fields, plus daily Tmin/Tmax aggregations

---

## Initial and boundary conditions
- **Initial conditions:** NCEP GFS (0.25°)
- **Lateral boundary conditions:** NCEP GFS (0.25°), updated hourly
- **Data assimilation:** None at the regional level — the regional run inherits the GFS analysis

---

## Physics configuration
Key physical parameterizations:
- **Microphysics:** WSM6 (single-moment, 6 hydrometeor classes)
- **Longwave radiation:** RRTM
- **Shortwave radiation:** Dudhia
- **Planetary boundary layer:** MYJ (Mellor–Yamada–Janjić)
- **Land surface model:** NOAH 4-layer (0–10 cm, 10–40 cm, 40–100 cm, 1–2 m)
- **Cumulus parameterization:** None (deep convection explicitly resolved)

The model uses a variable time step.

---

## What it provides
Deterministic forecasts of near-surface and atmospheric fields, distributed as:

**10-minute frequency:**
- Accumulated precipitation

**Hourly:**
- 2 m temperature and relative humidity
- 10 m wind speed and direction
- Surface pressure
- Accumulated precipitation
- Incoming and outgoing longwave radiation
- Incoming shortwave radiation
- Soil temperature and soil moisture (top 0–10 cm layer)
- Freezing-level height (height AMSL of 0 °C isotherm)

**Daily:**
- Minimum and maximum 2 m temperature

2 m temperature, 10 m wind speed, and the daily Tmin/Tmax products are **statistically debiased** against in-situ surface observations (Cutraro et al. 2020). If the debiasing procedure fails for a given file, raw model output is delivered instead.

---

## Data availability
- **Is the data free?** Yes
- **License:** Creative Commons Attribution 2.5 Argentina (CC BY 2.5 AR) — a localized port of CC BY 2.5. Allows sharing and adaptation including for commercial use, with attribution to SMN required. License deed: https://creativecommons.org/licenses/by/2.5/ar/deed.en
- **Is the data downloadable?** Yes
- **Data formats:** NetCDF (CF conventions)
- **Official download location:**
  - AWS Open Data Registry: https://registry.opendata.aws/smn-ar-wrf-dataset/
  - S3 bucket: `s3://smn-ar-wrf/` (region `us-west-2`)
  - Browser index: https://smn-ar-wrf.s3.amazonaws.com/index.html
  - CLI: `aws s3 ls --no-sign-request s3://smn-ar-wrf/`

File path convention:
`DATA/WRF/DET/{YYYY}/{MM}/{DD}/{HH}/WRFDETAR_{freq}_{YYYYMMDD}_{HH}_{fhr}.nc`
where `{freq}` is `10M`, `01H`, or `24H`, `{HH}` is the cycle (00 or 12), and `{fhr}` is the lead time (3-digit, in hours for `10M`/`01H` and in days for `24H`).

---

## Notes
- WRF-SMN Argentina explicitly resolves deep convection and is optimized for high-impact weather over southern South America.
- The published WRF version (4.0, with documentation citing Skamarock et al. 2019) reflects what SMN documents on its open data portal. The SMN documentation site appears to date from around 2022, so the actual operational version may have advanced since then; this is the version SMN states publicly.
- Some surface variables (T2, magViento10, Tmax, Tmin) are post-processed with a statistical debiasing step against surface observations. The methodology is documented in Cutraro et al. 2020 (linked in *Official documentation*).
- The horizontal grid is described in the documentation as a Lambert-conformal projection centered at 35°S, 65°W with x and y coordinates expressed in metres relative to the grid center.
- For comparison with other South American regional WRF systems documented in this repository, see [WRF (IDEAM Regional Forecast System)](../colombia/wrf-ideam.md) for Colombia.

---

## Official documentation
- General information: https://odp-aws-smn.github.io/documentation_wrf_det_en/General_information/
- Data structure: https://odp-aws-smn.github.io/documentation_wrf_det_en/Data_structure/
- Data format: https://odp-aws-smn.github.io/documentation_wrf_det_en/Data_format/
- Data access: https://odp-aws-smn.github.io/documentation_wrf_det_en/Data_access/
- Documentation root (English): https://odp-aws-smn.github.io/documentation_wrf_det_en/
- Documentation root (Spanish): https://odp-aws-smn.github.io/documentation_wrf_det/
- AWS Open Data registry entry: https://registry.opendata.aws/smn-ar-wrf-dataset/
- Skamarock et al. (2019), *A Description of the Advanced Research WRF Model Version 4*, NCAR Technical Note NCAR/TN-556+STR. https://www2.mmm.ucar.edu/wrf/users/docs/technote/v4_technote.pdf
- Cutraro et al. (2020), debiasing methodology (SMN repository): http://repositorio.smn.gob.ar/handle/20.500.12160/1405
- SMN model configuration report: http://repositorio.smn.gob.ar/handle/20.500.12160/1402
