# Model Testing & Validation — EV Adoption & Charger Forecast (WA: King, Pierce, Kitsap, Chelan)

This step rigorously evaluates multiple ML/DL approaches to discover the **best-performing, most realistic** model for EV adoption and charger build-out (2025–2050). It uses deterministic **Lower/Upper** policy scenarios as anchors and a **policy-aware, monotonic Monte Carlo (MC)** forecast for probabilistic results.

---

## 1) Objective
- Automate comparisons across models and select the most **accurate, explainable, policy-consistent** approach.
- Quantify uncertainty with **P10/P50/P90** bands and verify calibration against bounds.
- Ensure findings generalize across **four county prototypes** (Urban, Urban/Suburban, Suburban, Rural/Forest).

---

## 2) Automated Modeling Harness
- **Pipelines:** Python + scikit-learn API wrappers over multiple candidates.
- **Candidates:** ARIMA/ETS, Logistic/Gompertz, Random Forest/XGBoost, Quantile Regression, **Monotonic MC** (primary).
- **Data splits:** Rolling time-series CV; grouped CV by county type (urban/suburban/rural).
- **Artifacts:** Per-county/year tables, charts with **phase shading**, and Excel workbooks.

---

## 3) Performance Metrics
- **Error (levels/deltas):** RMSE, MAE, sMAPE  
- **Quantiles:** Pinball loss @ P10/P50/P90; **Coverage** (% actuals within P10–P90)  
- **Fit diagnostics:** R²/Adjusted R² for parametrics; bias/variance by phase and county type  
- **Phase weighting (optional):** Slightly up-weight Phase-2 realism

---

## 4) Loss Functions Compared
- **MSE / RMSE** (sensitive to large errors)  
- **MAE / Huber** (robust to outliers)  
- **Pinball (Quantile) Loss** for interval quality  
- **Monotonic penalty** (custom): discourages downward year-over-year moves in adoption

---

## 5) Hyperparameter Tuning
- **Search:** Grid/Random/Optuna/Ray Tune  
- **Parametric curves:** capacity (K), growth (r), inflection (t₀)  
- **MC:** phase weights `target_w_upper`, pull `kappa`, charger push `beta`, noise `sigma`, cap `target_cap` (< 0.95)  
- **Tree/boosting:** depth, learning rate, min samples, regularization  
- **Quantile models:** α levels, regularization

---

## 6) Cross-Validation Design
- **Rolling origin:** train ≤ year *t*, validate on t+1…t+h  
- **Grouped CV:** hold out full counties or county types to test transferability  
- **Backtests:** replay recent years to evaluate interval coverage and drift

---

## 7) Ensembling
Blend complementary signals while preserving monotonicity:
1. **Lower/Upper** deterministic anchors  
2. **Logistic/Gompertz** S-curve (smooth long-run shape)  
3. **MC P50** (phase-aware median)  
- **Final P50** = weighted blend minimizing RMSE subject to monotonic and cap constraints  
- P10/P90 primarily from **MC quantiles**; optionally widened to match target coverage

---

## 8) Overfitting vs Generalization Checks
- **Overfit red flags:** >100% adoption, non-monotonic dips, oscillations between years, narrow bands with poor coverage  
- **Healthy generalization:** smooth S-curves, monotone MC paths, P10–P90 coverage ~80%, consistent phase behavior

---

## 9) Best Model(s) Presented
**Primary:** **Policy-Aware, Monotonic Monte Carlo**
- Enforces **non-decreasing** adoption
- Encodes **phase-specific realism** (funding → home-charging → saturation)
- Produces **P10/P50/P90** with good coverage and alignment to policy milestones

**Secondary:** **Parametric Logistic/Gompertz**
- Compact & interpretable long-run shape; used in ensemble for stability

---

## 10) Real-World Phase Assumptions (driving MC behavior)

### Phase 1 (2025–2030) — *Closer to Upper; fast early lift*
**Drivers:** DCFC buildout, incentives/mandates, model proliferation, improving TCO, social proof  
**MC intent:** Median leans **upward**; friction temporarily compressed  
**Parameters:** `target_w_upper≈0.55–0.65`, `beta` medium-high, `kappa` moderate, `sigma` low-med  
**Prototype nuance:** Urban (King) strongest tilt; Rural (Chelan) benefits but slightly less

### Phase 2 (2030–2040) — *Early slowdown near Lower (2030–35), then re-accel*
**Why it slows (2030–35):** Home-charging saturation, policy tapering, interconnection bottlenecks, MFH/curbside gaps, replacement-cycle pause  
**Why it accelerates (post-2035):** Fleet turnover, ecosystem maturity (reliability/charging speed), used-EV depth/leasing, workplace & MFH charging scale-up  
**MC intent:**  
- **2030–35:** median hugs **Lower** → `target_w_upper≈0.20–0.30`, lower `kappa`, `beta` damped by rising `home_share`, low `sigma`  
- **2036–40:** raise `kappa`/`beta` to reflect re-acceleration  
**Prototype nuance:** Suburban (Kitsap/Pierce) slowdown more pronounced; Urban (King) re-accelerates after curbside/workplace progress; Rural (Chelan) depends on corridor/tourism & utility upgrades

### Phase 3 (2040–2050) — *Sustained rise but capped < 95%*
**Drivers:** Saturation/last-mile frictions, grid/site constraints, policy shift to reliability/recycling/integration  
**MC intent:** Monotonic ↑ toward **cap** (e.g., `target_cap=0.94`) to reflect persistent frictions  
**Parameters:** higher `kappa` to capped target, `beta` moderate, `sigma` low-med; cap enforces realism  
**Prototype nuance:** Urban (King) nears cap sooner; Rural (Chelan) later due to spatial/grid costs

**Parameter cheat-sheet**

| Concept | Phase 1 | Phase 2 (2030–35 / 2036–40) | Phase 3 |
|---|---|---|---|
| `target_w_upper` | ↑ 0.55–0.65 | ↓ 0.20–0.30 → ↑ ~0.6–0.7 | ~0.6–0.7 (capped) |
| `kappa` (pull) | medium | low → medium-high | medium-high |
| `beta` (charger push) | medium-high | low (home_share dampening) → medium | medium |
| `sigma` (noise) | low-med | low | low-med |
| `target_cap` | — | — | < 0.95 (e.g., 0.94) |
| Monotonicity | enforced | enforced | enforced |

---

## 11) Deliverables
- **Excel**: `Scenarios_LU` (Lower/Upper), `Forecast` (MC P10/P50/P90 & chargers), **Charts** (phase-shaded)  
- **PNGs**: Chargers, Adoption (with bands), EV registrations (P10/P50/P90)  
- **Report**: This README + parameter settings per county

---

### TL;DR
- We compared multiple families (time-series, S-curves, tree-based, quantile, simulation).  
- The **Monotonic MC** is the most realistic and best-performing when judged on **error + coverage + policy alignment**, especially with **phase assumptions** explicitly encoded and tuned.  
- Ensemble with parametric S-curves adds stability without sacrificing realism.

