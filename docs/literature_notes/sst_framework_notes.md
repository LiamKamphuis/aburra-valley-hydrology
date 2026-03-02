# SST Framework Notes

**Source document:** *Rainfall Characterisation & Stochastic Storm Transposition:
Methods, Accuracy, Limitations and Machine Learning Integration for the Aburrá
Valley.* Emergente Research, February 2026. Prepared for SIATA.

---

## 1. Physical and Data Context

### The Aburrá Valley Setting

The Aburrá Valley is a narrow, steep-sided inter-Andean valley (6°N, 75°W) in
the Cordillera Central. The Medellín River runs along the valley floor, fed by
short, steep tributaries draining catchments of 5–50 km². Elevation ranges from
~1,430 m (valley floor) to over 2,800 m on surrounding ridges, producing strong
orographic rainfall gradients.

Climate is tropical bimodal. Convective cells — triggered by afternoon heating
over the ridges — produce intense short-duration pulses exceeding 100 mm/hr,
generating flash floods and debris flows within 15–30 minutes. The Salgar
disaster (May 2015, >100 fatalities) is the reference event for life-safety
stakes in the valley.

### The Data Scarcity Problem

Urban drainage and flood protection infrastructure is designed to specific
return periods: 25-year for road drainage, 100-year for major infrastructure,
up to 1,000-year for dam spillways. With only ~15 years of high-resolution
radar data (SIATA archive: ~2010–2025), classical at-site frequency analysis
cannot reliably estimate return periods beyond ~30–40 years. SST was developed
specifically to address this gap.

### SIATA Network Summary

| Sensor | Quantity | Resolution | Key Output |
|---|---|---|---|
| Rain gauges | 84 | 1 min, point | Accumulation, intensity |
| Met stations | 16 | 1 min, point | T, RH, wind, pressure |
| Disdrometers (OTT Parsivel2) | 6 | 1 min, point | Drop size distribution |
| C-band polarimetric radar | 1 | 5 min, ~128 m | QPE gridded fields |
| River level sensors | ~30 | 1–5 min, point | Stage, discharge |

The C-band radar QPE at 5-minute / 128 m resolution is the primary SST input.

---

## 2. SST: Origins, Aims, and Conceptual Foundation

### Origins

SST originates in the Probable Maximum Precipitation (PMP) methodology of the
mid-20th century, which transposed historical extreme storms to worst-case
locations for dam safety analysis. The modern probabilistic SST framework was
formalised by **Wright et al. (2013, 2014)** and comprehensively reviewed by
**Wright et al. (2020)**.

### Conceptual Foundation: The Stationarity Hypothesis

The central assumption is **spatial stationarity within the transposition domain**:
within a meteorologically homogeneous region, a storm that occurred at one
location could plausibly have occurred anywhere else within the domain under the
same large-scale synoptic forcing. Formally:

> P(storm centre at location x | storm occurs) is uniform over the domain.

This is an approximation, but physically defensible when the domain is carefully
defined to exclude terrain-driven meteorological boundaries. Domain definition
is the most critical and most consequential modelling decision in SST.

### Primary Aims

1. **Flood frequency beyond the observed record.** Generating 10,000+ synthetic
   storms from 15 years of radar observations creates an effective record of
   thousands of storm-years, enabling robust estimation of 500- and 1,000-year
   events.

2. **Preserve observed storm microphysics and spatial structure.** SST works
   with real radar QPE fields — spatial coherence, intensity gradients, and
   storm morphology are drawn from actual observed events, not parametric
   idealisations.

3. **Generate training data for ML models.** SST + hydrological model routing
   generates thousands of (rainfall, discharge) event pairs needed to train
   deep learning models on the full intensity spectrum, including rare extremes
   (Gurbuz & Mantilla, 2024).

4. **Capture storm placement uncertainty.** For any target watershed, transposing
   each storm to all feasible domain locations produces a full probability
   distribution of watershed-average rainfall — not just the worst-case or mean.

5. **Support design-level engineering analysis.** SST-derived flood frequency
   curves with quantified uncertainty at sub-basin level directly inform
   hydraulic infrastructure design and early warning thresholds.

---

## 3. The RainyDay Implementation

RainyDay (Wright et al.) is the open-source Python implementation of SST. The
workflow for the Aburrá Valley:

1. Define the transposition domain using the SIATA radar footprint (~50×50 km
   centred on the valley), excluding areas with fundamentally different
   storm-generating mechanisms (e.g., Pacific coast vs. Andean interior).
2. Extract storm events from the radar QPE archive: identify contiguous periods
   of significant area-averaged rainfall separated by dry intervals ≥2 hours;
   catalogue each event as a 3D array (time × lat × lon).
3. For each observed storm, randomly sample N placements within the domain
   (typically N = 1,000 per storm × ~15 storms/year = ~15,000 annual
   realisations).
4. Compute the watershed-average rainfall (WAR) time series over the target
   sub-basin for each transposed placement.
5. Identify the annual maximum WAR from the ensemble; fit an extreme value
   distribution (GEV or GPD); read off return period estimates.
6. *(ML pipeline)* Route WAR time series through a hydrological model to
   generate synthetic (rainfall, discharge) training pairs.

---

## 4. How SST Improves on Classical Frequency Analysis

### Sample Size Amplification

| Method | Effective Sample | Reliable T Estimation | Uncertainty at T=100yr |
|---|---|---|---|
| At-site gauge record | 15 years | ~30 years | Very high (>50%) |
| Regional L-moments | 30–60 years (pooled) | ~100 years | High (30–50%) |
| SST (1,000 transpositions) | ~15,000 storm-years | ~1,000 years | Moderate (15–25%) |
| SST (10,000 transpositions) | ~150,000 storm-years | ~10,000 years | Low (<15%) |

### Spatial Structure Preservation

SST retains full storm spatial coherence from radar QPE. For small catchments
(~5 km²), whether a convective core hits the headwaters or lower basin determines
peak discharge far more than total event volume. Classical point gauge methods
and parametric generators cannot represent this.

| Storm Attribute | Parametric Methods | SST |
|---|---|---|
| Spatial coherence of rainfall core | Imposed (symmetric Gaussian or type curve) | Directly from radar observation |
| Temporal structure (rise/fall) | Assumed functional form | Observed temporal dynamics |
| Storm movement / advection | Typically ignored | Included via Lagrangian tracking |
| Multi-cell spatial organisation | Cannot represent | Naturally captured in radar field |
| Orographic enhancement gradients | Requires separate DEM adjustment | Implicitly in observed radar QPE |
| Storm duration and volume trade-off | Assumed relationship | As-observed from individual events |

---

## 5. Accuracy Testing and Validation

### The Fundamental Validation Challenge

SST faces an inherent circularity: it requires long records to validate predictions
of rare events, but was developed precisely because such records do not exist.
The main validation approaches are:

**Split-record cross-validation.** Withhold years from the SST catalog; predict
extremes in the withheld period; compare to observations. Limited to return
periods shorter than the withheld record. Wright et al. (2014, Charlotte NC):
SST-predicted curves encompassed observed annual maxima in withheld years; SST
Q100 consistent with regional regression estimates from much longer records.

**Regional regression comparison.** Compare SST-derived Q100 against IDEAM
regional regression equations for the Colombian Andes. Wright et al. (2014):
SST and regional regression Q100 agreed within ±15–25% — comparable to the
uncertainty of the regression itself.

**Paleoflood and historical evidence.** Historical flood accounts (pre-SIATA
Aburrá Valley events: 1960s, 1980s) provide rough independent data points for
testing SST curves at longer return periods.

**Domain sensitivity analysis.** Vary domain size and shape; measure change in
Q100 estimates. Robust SST: ±10–20% domain area → <5% change in Q100. High
sensitivity indicates the stationarity assumption is being violated at the
domain boundary.

**ML generalisation test (Gurbuz & Mantilla, 2024).** GRU models trained purely
on SST+HLM synthetic data achieve hindcast NSE competitive with a calibrated
physics-based model on real observed events. This empirically confirms that
SST-generated storms preserve sufficient physical realism to generalise beyond
the synthetic training set.

### SST Uncertainty Components

SST produces three quantifiable uncertainty layers that classical methods cannot
provide:

1. **Sampling uncertainty** — finite number of transpositions; reduced by
   increasing N (1,000–10,000 per storm).
2. **Catalog uncertainty** — finite observed record; reduced by longer radar
   archives or combining SST with GWEX (complementary approach).
3. **Model uncertainty** — stationarity assumption; domain definition choices;
   storm selection criteria.

---

## 6. Known Limitations and Mitigations

### 6.1 Stationarity Assumption

**Problem:** SST assumes climate stationarity — that the storm catalog built from
the observed record represents future storm statistics. ENSO modulation and
long-term climate change violate this.

**Mitigations:**
- Stratify the storm catalog by ENSO phase (La Niña / neutral / El Niño) and
  run separate SST analyses per phase. Combine using ENSO phase occurrence
  probabilities.
- Seasonal stratification: separate MAM and SON catalogs with phase-specific
  Poisson arrival rates.
- Couple SST with bias-corrected GCM projections for future scenario analysis
  (see Beneyto et al. 2024 for the GWEX-based analogue of this approach).

### 6.2 Domain Definition Sensitivity

**Problem:** The transposition domain boundary is the most consequential modelling
decision in SST, and also the most subjective. Including meteorologically
heterogeneous areas (e.g., mixing Pacific-facing and Andean-interior storm
populations) inflates variance.

**Mitigations:**
- Define separate domains for the south/east sub-basins (higher convective
  exposure) and northern sub-basins (lower intensity, different aspect). This
  directly mirrors the orographic RFA stratification.
- Apply an orographically weighted transposition kernel rather than uniform
  random placement — up-weight domain areas with similar elevation and aspect
  to the target watershed.
- Conduct systematic domain sensitivity analysis (vary boundary ±10–20 km;
  assess Q100 stability).

### 6.3 Antecedent Soil Moisture Independence

**Problem:** Standard SST treats each transposed storm as independent of
antecedent conditions. In the Aburrá Valley, the dangerous compound event
sequence — stratiform preconditioning (raising antecedent soil moisture) followed
by convective triggering — means that flood magnitude is strongly conditioned on
antecedent state. SST by default misses this.

**Mitigations:**
- Classify storm events by antecedent soil moisture state (wet/dry) using the
  gauge record; run separate SST ensembles per state; combine using the
  climatological frequency of each state.
- Explicitly model the stratiform preconditioning → convective triggering
  sequence as a two-stage SST, preserving the temporal dependency between
  storm types.

### 6.4 Short Catalog Length

**Problem:** With ~15 years of radar data, the storm catalog contains ~150–200
significant storm events. This is adequate for SST but the catalog itself may
not include the most extreme storm types.

**Mitigations:**
- Complement SST with GWEX stochastic weather generation: GWEX generates 5,000+
  year synthetic series calibrated to regional quantiles, providing a parallel
  estimate that is not constrained by catalog length.
- Where RainyDay and GWEX AEP curves agree, estimates are robust. Divergence
  beyond T=100yr quantifies the uncertainty bounds and reflects catalog
  limitations.

---

## 7. SST Integration with Machine Learning (Gurbuz & Mantilla, 2024)

### The Core Problem SST Solves for ML

A GRU encoder-decoder trained on 15 years of observed rainfall-runoff events
will: (a) overfit to the storm types that happened to occur in the observation
period; (b) never encounter the intense events generating the most dangerous
flash floods; and (c) have insufficient sample size to learn nonlinear
rainfall-runoff dynamics at the tail. SST addresses all three.

### The SST–HLM–GRU Pipeline
```
SIATA radar archive
        ↓
  RainyDay SST → 10,000 synthetic storm WAR time series
        ↓
  Physics-based HLM (TOPMODEL / SWMM) → synthetic discharge time series
        ↓
  10,000 (rainfall, discharge) event pairs → GRU encoder-decoder training
        ↓
  Trained GRU → operational flash flood forecasting
```

**Key finding (Gurbuz & Mantilla, 2024):** The GRU trained on SST-generated data
achieves hindcast NSE competitive with the calibrated physics-based HLM. This
validates that SST-generated storms have sufficient physical realism to train
models that generalise to real events.

**Critical caveat:** In forecast mode (using forecast rather than observed
rainfall), neither the GRU nor the HLM significantly outperforms temporal
persistence at lead times >1–2 hours. The bottleneck is **rainfall forecast
uncertainty (nowcasting accuracy)**, not the discharge model. Investment in
convLSTM/transformer nowcasting is at least as important as refining the
SST–GRU pipeline.

### The SST–ML Feedback Loop

An emerging research direction: use GRU underperformance patterns to diagnose
SST catalog gaps.

1. Train GRU on SST+HLM synthetic data; evaluate on held-out observed events.
2. Identify where the ML model underperforms (typically at highest intensities).
3. Use underperformance patterns to identify which storm types are
   underrepresented in the SST catalog.
4. Revise the domain definition or storm perturbation strategy to generate more
   of those storm types.
5. Re-train the GRU on the revised catalog; iterate until convergence.

This converts ML model performance into a diagnostic for SST catalog quality —
a research opportunity not yet appearing in the Wright et al. or
Gurbuz-Mantilla literature.

---

## 8. Proposed Action Plan for the Aburrá Valley

| Phase | Duration | SST-Specific Activities | Deliverable |
|---|---|---|---|
| 1: Data & QC | 0–3 months | Radar QPE QC: attenuation correction, blockage removal, Z–R calibration with Parsivel | Clean radar QPE archive ready for SST ingestion |
| 2: Classical Characterisation | 2–5 months | Local Z–R, IDF curves, spatial variogram; first SST run with default domain; storm catalog review | SST storm catalog (~500 events); draft return period curves |
| 3: SST Refinement | 4–7 months | Domain sensitivity analysis; stratified transposition by storm type; antecedent moisture conditioning | Validated SST return period curves with uncertainty bounds |
| 4: ML Training Data | 5–9 months | Route SST storms through HLM; generate (rainfall, discharge) training pairs; train and evaluate GRU | Trained GRU encoder-decoder; hindcast NSE vs. HLM comparison |
| 5: Operational Integration | 8–12 months | ConvLSTM nowcast → GRU discharge forecast pipeline; uncertainty quantification; SIATA integration | Operational prototype; peer-reviewed paper |

---

## 9. Key References

**Stochastic Storm Transposition**
- Wright, D.B., Smith, J.A., Villarini, G., & Baeck, M.L. (2013). Estimating the
  frequency of extreme rainfall using weather radar and stochastic storm
  transposition. *Journal of Hydrology*, 488, 150–165.
- Wright, D.B., Smith, J.A., & Baeck, M.L. (2014). Flood frequency analysis using
  radar rainfall fields and stochastic storm transposition. *Water Resources
  Research*, 50(3), 1592–1615.
- Wright, D.B., Yu, G., & England, J.F. (2020). Six decades of rainfall and flood
  frequency analysis using stochastic storm transposition: Review, progress, and
  prospects. *Journal of Hydrology*, 585, 124816.

**ML for Rainfall-Runoff and SST Integration**
- Gurbuz, B., & Mantilla, R. (2024). Using a physics-based hydrological model and
  storm transposition to investigate machine-learning algorithms for streamflow
  prediction. *Journal of Hydrology*.
- Kratzert, F., Klotz, D., Brenner, C., Schulz, K., & Herrnegger, M. (2018).
  Rainfall-runoff modelling using Long Short-Term Memory (LSTM) networks. *HESS*,
  22(11), 6005–6022.

**Radar QPE**
- Bringi, V.N., & Chandrasekar, V. (2001). *Polarimetric Doppler Weather Radar*.
  Cambridge University Press.

**Nowcasting**
- Ravuri, S., et al. (2021). Skilful precipitation nowcasting using deep generative
  models of radar. *Nature*, 597, 672–677.

**Climate and Hydrology of the Aburrá Valley**
- IDEAM (2015). *Estudio Nacional del Agua*. Bogotá.
- SIATA (2020). *Informe Técnico: Red Hidrometeorológica del Valle de Aburrá*.
  Medellín.
