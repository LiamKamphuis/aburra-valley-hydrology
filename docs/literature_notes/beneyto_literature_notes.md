# Literature Notes — Beneyto et al. (2023) and Beneyto et al. (2024)

*Notes compiled for the Aburrá Valley Hydrology project.*  
*File location: `docs/literature_notes/beneyto_literature_notes.md`*

---

## Beneyto et al. (2023)

**Full citation:** Beneyto, C., Aranda, J.Á., & Francés, F. (2023). Exploring the
uncertainty of Weather Generators' extreme estimates in different practical available
information scenarios. *Hydrological Sciences Journal*, 68(9), 1203–1212.
https://doi.org/10.1080/02626667.2023.2208212

*(Note: A companion 2023 paper — Beneyto, C., Vignes, G., Aranda, J.Á., & Francés,
F. (2023). Sample Uncertainty Analysis of Daily Flood Quantiles Using a Weather
Generator. *Water*, 15, 3489 — addresses the same uncertainty theme from the
flood-quantile angle and is treated as part of the same methodological contribution.)*

---

### Core Contribution

The paper systematically characterises how uncertainty in weather generator (WG)
extreme-quantile estimates varies across realistic data-availability scenarios. Its
central methodological contribution is demonstrating that constraining the E-GPD
tail-shape parameter ξ using a regional X₁₀₀ estimate — rather than fitting ξ freely
from the local record alone — dramatically reduces uncertainty at long return periods.
The paper provides the empirical justification for the ξ-regionalisation step that is
carried forward as standard practice in both the 2024 paper and in the broader GWEX
framework.

---

### Study Setup

- **Weather generator:** GWEX (Evin et al., 2018), a multi-site stochastic model
  using the Extended Generalised Pareto Distribution (E-GPD) for precipitation
  amounts.
- **E-GPD parameterisation:** Three-parameter distribution
  F(x; λ) = [1 − (1 + ξx/σ)₊^(−1/ξ)]^κ, where κ governs the lower tail shape,
  σ is the scale parameter, and ξ controls the rate of upper-tail decay.
- **Experimental design:** Multiple synthetic "available information" scenarios
  simulate the range of record lengths and network densities encountered in practice.
  WG performance is evaluated across these scenarios against known population
  quantiles.
- **Key metric:** Relative Root Mean Square Error (RRMSE) of quantile estimates at
  T = 100 yr, T = 200 yr, and longer return periods.

---

### Key Results

- Freely estimated ξ from short local records produces highly unstable extreme
  quantiles, with RRMSE escalating sharply beyond T = 100 yr — confirming that the
  upper tail is under-identified from typical gauge records alone.
- Fixing ξ via a regional X₁₀₀ estimate (derived from a Hosking–Wallis L-moments
  RFA) reduces RRMSE by approximately **89% at T = 200 yr** compared to
  unconstrained local fitting.
- The benefit of ξ-regionalisation grows with return period: constrained scenarios
  converge far more reliably to true population quantiles at very low AEPs.
- Even with imperfect regional estimates, the constrained approach consistently
  outperforms unconstrained local fitting across all data-availability scenarios.
- Clear operational implication: RFA should always precede WG calibration, and its
  output should directly constrain ξ — not serve merely as a validation benchmark.

---

### Relevance to Aburrá Valley Project

| Aspect | Application |
|---|---|
| **ξ-regionalisation** | The 89% RRMSE reduction at T=200yr is the core justification for running L-moments RFA (Tier 1) *before* GWEX calibration (Tier 2). The RFA output is a required WG input, not a parallel deliverable. |
| **Uncertainty quantification** | Provides the uncertainty framework for interpreting divergence between RainyDay and GWEX AEP curves at long return periods. Where ξ is poorly constrained locally, divergence is expected and should be reported as structured uncertainty rather than model error. |
| **Data-scarcity relevance** | The Aburrá gauge network, while relatively dense by regional standards, still presents short sub-basin records — especially for small tributary catchments. The paper's scenario analysis directly maps onto this situation. |
| **Orographic heterogeneity** | The regional X₁₀₀ used to constrain ξ must itself account for orographic gradients. Separate RFA regions for south/east and northern sub-basins will yield different ξ values and therefore different WG tail behaviours — this is a feature, not a complication. |
| **ENSO conditioning** | The paper does not address nonstationary ξ. For ENSO-stratified analyses, separate regional X₁₀₀ values may be needed for La Niña and neutral/El Niño pooled records — an open methodological extension for this project. |

---

---

## Beneyto et al. (2024)

**Full citation:** Beneyto, C., Aranda, J.Á., & Francés, F. (2024). On the Use of
Weather Generators for the Estimation of Low-Frequency Floods under a Changing
Climate. *Water*, 16(7), 1059. https://doi.org/10.3390/w16071059

---

### Core Contribution

An end-to-end methodology for estimating high-return-period flood quantiles under
climate change scenarios, integrating: (1) bias-corrected GCM–RCM projections;
(2) L-moments regional frequency analysis of maximum daily precipitation applied to
both historical and future climate data; (3) GWEX stochastic weather generation with
ξ constrained by the regional X₁₀₀; and (4) a fully distributed hydrological model
(TETIS) to convert synthetic precipitation to flood quantiles. The paper demonstrates
that even where total annual precipitation decreases, extreme quantiles and flood
magnitudes increase substantially — and that this effect is amplified in smaller
catchments.

---

### Study Setup

- **Case study:** Rambla de la Viuda, eastern Spain — a semi-arid Mediterranean
  catchment with episodic flash-flood behaviour and limited gauge records. Broadly
  analogous to steep inter-Andean sub-basins in data-scarcity and flood-response
  characteristics.
- **Climate projections:** EUROCORDEX ensemble (12 GCM–RCM combinations), RCP
  scenarios; mid-term (2035–2064) and long-term (2065–2094) projection windows.
- **Bias correction:** Non-parametric quantile mapping applied separately by season
  (DJF, MAM, JJA, SON); stationary bias functions applied forward to future periods.
- **RFA:** Hosking–Wallis L-moments with Index Variable method; Discordance and
  Homogeneity tests; GPD selected by AIC; quantiles deregionalised to grid/station
  level.
- **WG:** GWEX with E-GPD; ξ adjusted per grid cell using the regional X₁₀₀
  (following Beneyto et al. 2023); 5,000-year synthetic series generated for each
  projection.
- **Hydrological model:** TETIS (fully distributed ecohydrological model), calibrated
  and validated in temporal, spatial, and spatio-temporal modes; Nash–Sutcliffe
  efficiency as primary criterion.
- **Output:** Flood quantiles at catchment outlet for T = 5, 10, 25, 50, 100 yr
  under control, mid-term, and long-term scenarios.

---

### Key Results

- **Precipitation:** Extreme quantiles increase 4.3–19.7% across scenarios;
  increase is proportional to return period and more pronounced in the long-term
  projection (T=100yr: +19.7%). Inter-model spread widens at higher return periods.
- **Temperature:** Maximum temperatures rise up to 3.6°C (long-term); heat waves
  intensify in frequency and magnitude, affecting antecedent soil moisture and
  initial flow conditions.
- **Flood quantiles:** Despite some reduction in mean annual flow, high-return-period
  flood quantiles increase by **53–58%** for the study catchment.
- **Catchment-size effect:** Flood quantile increases reach up to **145% for smaller
  sub-catchments** — a critical finding for the Aburrá Valley, where numerous small,
  steep tributary sub-basins (<5 km²) are the primary flood-hazard units.
- **Seasonal bias correction:** Applying quantile mapping separately by season
  (including MAM and SON as distinct strata) is necessary to capture the bimodal
  precipitation regime — directly mirroring the Aburrá Valley's ITCZ-driven
  seasonality.

---

### Relevance to Aburrá Valley Project

| Aspect | Application |
|---|---|
| **Methodological template** | The closest available analogue to the Aburrá Tier 3 workflow: RFA → GWEX (constrained ξ) → distributed HM → future flood quantiles. The pipeline is directly adoptable with valley-specific adaptations. |
| **Seasonal bias correction** | The DJF/MAM/JJA/SON quantile mapping protocol maps directly onto the bimodal ITCZ regime; MAM and SON must be treated as distinct bias-correction strata throughout. |
| **Small-catchment amplification** | The 145% flood-quantile increase in small sub-basins under climate change is directly relevant to the Aburrá tributary network. Small-basin FFA estimates are both the most hazard-critical and the most climate-sensitive — uncertainty bounding is non-optional. |
| **GWEX 5,000-year series** | Operationalises 5,000-yr synthetic generation as standard practice for low-AEP estimation — the target series length for Aburrá GWEX runs. |
| **RFA as prerequisite** | Reinforces the 2023 finding: orographically stratified RFA (separate south/east and northern regions) must precede GWEX parameter fitting. |
| **ENSO nonstationarity** | The paper assumes stationary bias corrections projected forward — an acknowledged limitation. ENSO-phase stratification of bias correction and ξ estimation is an open methodological extension specific to the Aburrá context. |
| **Hydrological model requirement** | Tier 3 requires a calibrated distributed model analogous to TETIS. Whether the Medellín collaborators have one is a key open question; if not, TETIS is a concrete candidate given demonstrated applicability to semi-arid flash-flood catchments. |

---

*Last updated: 2026-03-02*
