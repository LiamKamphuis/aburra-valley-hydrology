# Aburrá Valley Hydrology — Project Brief

## Project Overview

This project aims to characterise extreme rainfall and produce streamflow estimates
at multiple locations across the Aburrá-Medellín Valley, Colombia. The primary 
deliverables are intensity-duration-frequency (IDF) curves, annual exceedance 
probability (AEP) estimates, regional frequency analysis (RFA), flood frequency 
analysis (FFA), and extreme event modelling under nonstationary conditions. A 
secondary goal is to assess the influence of ENSO phases, seasonality, orographic 
effects, and long-term climate change on rainfall and flood estimates.

The work is being conducted in collaboration with a Medellín-based company using 
data from SIATA (Sistema de Alerta Temprana de Medellín y el Valle de Aburrá), 
which operates one of Latin America's most capable hydrometeorological monitoring 
networks, including a high-resolution radar network calibrated for the valley's 
complex terrain.

---

## Physical Setting

The Aburrá Valley is a narrow, steep-sided inter-Andean valley at approximately 
1,500–2,800 m elevation, oriented roughly north-south. The Medellín River drains 
the valley floor, receiving inputs from numerous steep tributary sub-basins with 
highly variable catchment areas.

**Rainfall regime:** Strongly bimodal, driven by the double passage of the ITCZ, 
producing two wet seasons — March–April–May (MAM) and September–October–November 
(SON). The Choco Jet provides sustained Pacific moisture during wet seasons. 
Mesoscale convective systems (MCS) are strongly phase-locked to the diurnal cycle.

**ENSO modulation:** La Niña phases intensify storms and increase flood frequency; 
El Niño phases suppress rainfall. This produces significant interannual 
nonstationarity that must be accounted for in frequency analysis.

**Orographic complexity:** Strong elevation gradients produce spatially 
heterogeneous rainfall. South and east sub-basins experience systematically higher 
convective exposure than northern sub-basins due to aspect and elevation difference 
relative to prevailing winds. A single valley-wide homogeneity assumption is 
physically unjustifiable.

**Storm types:** Two functionally distinct storm types operate in the valley. 
Stratiform systems (wide, low-intensity, basin-scale coverage) act as 
preconditioners by raising antecedent soil moisture. Convective cores (narrow, 
high-intensity, 0.8–8.5 km²) act as flood triggers. The dangerous compound 
sequence — stratiform preconditioning followed by convective triggering — was 
demonstrated in the Salgar flash flood of May 2015, which produced a peak flow of 
~225 m³/s in a 57 km² basin with mean slopes of 57.6%.

**Catchment scale sensitivity:** Smaller sub-basins (< ~5 km²) show high 
sensitivity to rainfall structure and spatial distribution. Larger basins show more 
stable, scale-averaged responses. This has direct implications for SST domain 
calibration and FFA at different valley locations.

---

## Methodological Framework

**Stochastic Storm Transposition (SST) — RainyDay framework:** Resamples and 
transposes observed radar storm footprints across a defined domain to generate 
synthetic precipitation records orders of magnitude longer than the observed 
catalog. Key customisations required for the Aburrá Valley include separate domains 
for south/east versus northern sub-basins, seasonal stratification of Poisson 
arrival rates by MAM/SON and potentially by ENSO phase, and an 
orographically-weighted transposition kernel.

**Stochastic Weather Generator — GWEX framework:** Parametric multi-site WG using 
the Extended Generalised Pareto Distribution (E-GPD) for precipitation amounts. The 
shape parameter ξ controls upper tail behaviour. Calibrating ξ against a regional 
X₁₀₀ reduces RRMSE by ~89% at T=200yr compared to default settings. Generates very 
long synthetic series (5,000+ years) suitable for estimating very low AEPs.

**Integration strategy:** RainyDay and GWEX run in parallel as complementary 
approaches. Where AEP curves agree, estimates are robust. Where they diverge — 
typically beyond T=100yr — the divergence quantifies uncertainty bounds.

**Regional Frequency Analysis:** Hosking and Wallis L-moments methodology with the 
Index Variable method. Discordance and Homogeneity tests to identify homogeneous 
regions accounting for orographic gradients. Generalised Pareto Distribution fitted 
via AIC. Local quantiles deregionalised per station/grid.

---

## Key Papers

| Paper | Role in Project |
|-------|----------------|
| Wright et al. (2013, 2020) | SST core methodology and suggested improvements |
| Beneyto et al. (2023) | WG uncertainty quantification, E-GPD framework |
| Beneyto et al. (2024) | Climate change FFA methodology, GWEX application |
| Evin et al. (2018) — GWEX | Core WG implementation underlying Beneyto framework |
| Hosking & Wallis (1997) | L-moments RFA methodology |
| Velásquez thesis | Aburrá Valley physical characterisation, storm types |
| Gurbuz et al. (2024) | ML-hydrology pipeline, GRU/LSTM for streamflow |
| Kratzert et al. (2018) | LSTM hydrology foundations |
| Poveda et al. (2001/2011) | ENSO-rainfall teleconnections in Colombian Andes |
| Papalexiou (2022) — CoSMoS-2s | Sub-daily stochastic generation for IDF curves |

---

## Open Questions Pending Data Audit

1. What is the SIATA radar record length and temporal/spatial resolution?
2. What rain gauge network exists and what are the record periods?
3. Are streamflow gauges available at the required valley locations?
4. Does the employer already have a storm catalog, or does one need to be built?
5. Are bias-corrected GCM outputs available for the region?
6. What hydrological model, if any, is already calibrated for the basin?

---

## Planned Deliverables

**Tier 1 — Core:** Regional frequency analysis with IDF curves at multiple valley 
locations accounting for orographic gradients. AEP estimates at key sub-basin 
outlets.

**Tier 2 — Extended:** SST-based frequency analysis using RainyDay with 
valley-specific domain customisation. ENSO and seasonal conditioning of frequency 
estimates.

**Tier 3 — Aspirational:** GWEX-RainyDay integration for uncertainty bounding. 
Climate change projections of extreme quantiles. Full FFA with hydrological model.
