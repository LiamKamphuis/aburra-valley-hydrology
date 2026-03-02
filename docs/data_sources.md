# Data Sources

Primary data for this project is drawn from the SIATA hydrometeorological network
and supplementary national and global datasets. This file records sensor
specifications, archive periods, known data quality issues, and access pathways.

---

## SIATA — Sistema de Alerta Temprana de Medellín y el Valle de Aburrá

SIATA operates one of Latin America's most capable urban hydrometeorological
monitoring networks, specifically designed for the Aburrá Valley's complex terrain
and flash flood risk environment.

### Sensor Network

| Sensor Type | Quantity | Temporal Resolution | Spatial Coverage | Key Variables |
|---|---|---|---|---|
| Rain gauges (tipping bucket) | 84 | 1 minute | Point — valley-wide | Rainfall accumulation, intensity |
| Meteorological stations | 16 | 1 minute | Point | Rain, temperature, RH, pressure, wind |
| Disdrometers (OTT Parsivel2) | 6 | 1 minute | Point | Drop size distribution (DSD) |
| C-band polarimetric radar | 1 | 5 minutes | ~128 m grid | Z_H, Z_DR, K_DP, ρ_hv, QPE |
| River level sensors | ~30 | 1–5 minutes | Point | Stage, derived discharge |

### C-band Radar

The radar is the cornerstone of the SST workflow. Key specifications:

- **Spatial resolution:** ~128 m grid
- **Temporal resolution:** 5 minutes
- **Archive period:** ~2010–2025 (~15-year record)
- **Polarimetric variables:** Z_H (reflectivity), Z_DR (differential reflectivity),
  K_DP (specific differential phase), ρ_hv (co-polar correlation coefficient)
- **QPE product:** Dual-polarisation QPE calibrated against Parsivel disdrometers

**Known data quality issues to address in pre-processing:**
- Beam blockage on western and southern ridges (requires sector-specific correction)
- Wet radome attenuation during heavy rainfall
- Z–R relationship uncertainty (calibrated locally against Parsivel DSD data)

### Rain Gauge Network

- 84 tipping-bucket gauges at 1-minute resolution across the valley
- Network is relatively dense by regional standards but still undersamples
  convective cores at the 1–5 km scale
- Key use: radar QPE bias correction; at-site frequency analysis for RFA

### River Level Sensors

- ~30 sensors on the Medellín River mainstem and major tributaries
- Stage–discharge rating curves required for conversion to discharge
- Record continuity: variable by station — audit required

---

## Open Data Audit Questions

The following questions remain open pending engagement with SIATA and the
Medellín collaborator:

1. What is the exact radar archive start date and are there significant gaps?
2. Are processed QPE gridded products available, or does raw radar processing
   need to be performed?
3. What rain gauge records predate the radar archive (pre-2010)?
4. Are rating curves and discharge records available at the required sub-basin
   outlet locations?
5. Does the collaborator have an existing storm catalog, or does one need
   to be built from scratch?
6. Are bias-corrected GCM outputs (CMIP6 / CORDEX-SA) available for the region?
7. What hydrological model, if any, is already calibrated for the valley?

---

## Supplementary and External Datasets

### Topography and Catchment Geometry

| Dataset | Source | Resolution | Use |
|---|---|---|---|
| DEM | GeoMedellín / IGAC | ~5–12 m (LiDAR available locally) | Catchment delineation, slope, aspect |
| Catchment shapefiles | POMCA Río Medellín | Vector | Sub-basin boundaries |

### Climate Projections

| Dataset | Source | Resolution | Use |
|---|---|---|---|
| CMIP6 GCM outputs | ESGF nodes | ~100 km | Long-term climate change forcing |
| CORDEX-SA RCM outputs | ESGF / IDEAM | ~25–50 km | Bias-corrected regional projections |

Bias correction should be applied separately by season (MAM, SON, DJF, JJA)
to respect the bimodal ITCZ regime. See Beneyto et al. (2024) for methodology.

### National Hydrology and Climatology

| Dataset | Source | Use |
|---|---|---|
| Estudio Nacional del Agua | IDEAM (2015, 2022) | Regional hydrology context, flood regionalisation |
| ENSO indices (ONI, MEI) | NOAA CPC | ENSO phase stratification for nonstationary analysis |
| Historical flood records | IDEAM / DAGRD | Pre-radar paleoflood validation data points |

---

## Data Management

- Raw data stored in `data/raw/` — never modified after ingestion
- All processing steps scripted in `src/data_processing/` and documented
- Processed outputs stored in `data/processed/` with version-dated filenames
- External datasets stored in `data/external/` with provenance notes

Raw data directories (`data/raw/`, `data/processed/`) are excluded from version
control via `.gitignore`. Processed data should be reproduced from raw inputs
using the processing scripts.
