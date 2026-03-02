# Aburrá Valley Hydrology

Extreme rainfall characterisation and streamflow estimation across the
Aburrá-Medellín Valley, Colombia.

Conducted in collaboration with SIATA (Sistema de Alerta Temprana de Medellín y el
Valle de Aburrá). See [`docs/project_brief.md`](docs/project_brief.md) for full
scope and methodology.

---

## Project Overview

The Aburrá Valley is a narrow, steep-sided inter-Andean valley at 1,500–2,800 m
elevation, subject to intense convective rainfall, a bimodal ITCZ-driven seasonal
regime, and strong ENSO modulation. This project produces:

- Intensity–Duration–Frequency (IDF) curves at multiple valley locations
- Annual Exceedance Probability (AEP) estimates
- Regional Frequency Analysis (RFA) accounting for orographic gradients
- Flood Frequency Analysis (FFA) at key sub-basin outlets
- Nonstationary analysis under ENSO, seasonal, and climate change forcing

---

## Methodological Framework

| Component | Tool / Method | Tier |
|---|---|---|
| Regional Frequency Analysis | L-moments (Hosking & Wallis), GPD via AIC | 1 |
| IDF curve construction | Sub-daily frequency fitting, deregionalisation | 1 |
| Stochastic Storm Transposition | RainyDay (Wright et al. 2013–2020) | 2 |
| Stochastic Weather Generation | GWEX + E-GPD (Evin et al. 2018; Beneyto et al. 2023–2024) | 2–3 |
| Flood Frequency Analysis | SST + distributed hydrological model | 3 |
| Nonstationarity | ENSO stratification, seasonal conditioning, GCM bias correction | 2–3 |

RFA output (orographically stratified regional X₁₀₀) is a prerequisite for GWEX
calibration, not a parallel deliverable.

---

## Repository Structure
```
aburra-valley-hydrology/
│
├── README.md
├── .gitignore
├── requirements.txt
├── environment.yml
│
├── data/
│   ├── raw/                  # Never modified — original SIATA outputs
│   │   ├── radar/
│   │   ├── gauges/
│   │   └── streamflow/
│   ├── processed/            # Cleaned, QC'd, formatted data
│   └── external/             # GCM outputs, DEM, catchment shapefiles
│
├── notebooks/
│   ├── 01_data_exploration/
│   ├── 02_rfa/
│   ├── 03_idf/
│   ├── 04_rainyday/
│   ├── 05_gwex/
│   ├── 06_nonstationarity/
│   └── 07_ffa/
│
├── src/
│   ├── data_processing/
│   ├── rfa/
│   ├── idf/
│   ├── rainyday_custom/
│   │   ├── domain_definition.py
│   │   ├── orographic_kernel.py
│   │   └── enso_stratification.py
│   ├── gwex_integration/
│   └── visualisation/
│
├── results/
│   ├── figures/
│   ├── tables/
│   └── reports/
│
└── docs/
    ├── project_brief.md
    ├── data_sources.md
    └── literature_notes/
        ├── beneyto_literature_notes.md
        └── sst_framework_notes.md
```

---

## Data Sources

Primary data is from the SIATA hydrometeorological network. See
[`docs/data_sources.md`](docs/data_sources.md) for sensor specifications, archive
periods, and access details.

---

## Key References

| Paper | Role |
|---|---|
| Wright et al. (2013, 2014, 2020) | SST core methodology — RainyDay |
| Evin et al. (2018) | GWEX multi-site stochastic WG |
| Beneyto et al. (2023) | E-GPD uncertainty; ξ-regionalisation justification |
| Beneyto et al. (2024) | Climate change FFA pipeline template |
| Hosking & Wallis (1997) | L-moments RFA methodology |
| Gurbuz & Mantilla (2024) | SST + ML streamflow pipeline |
| Velásquez thesis | Aburrá Valley physical characterisation |
| Poveda et al. (2001, 2011) | ENSO–rainfall teleconnections, Colombian Andes |

---

## Status

Work in progress. Pending data audit — see open questions in
[`docs/project_brief.md`](docs/project_brief.md).
