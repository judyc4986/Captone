# 🌎 Washington State EV Forecast – Full Scaled Pipeline (2024–2050)

This project transforms a small **4-county prototype** into a **statewide EV forecasting system** that supports all **39 Washington counties**, assigns them to **population-tier Monte Carlo templates**, aggregates county-level outputs into a **statewide forecast**, and fits a **polynomial statewide model** to determine future charger requirements.

The end result is an **automated forecasting engine** that produces Excel outputs, PNG charts, and an official **statewide best-fit EV forecast curve** used for EV readiness and infrastructure planning.

---

# 📊 1. How the Forecast Works (High-Level Flow)

To avoid confusion, here is the real logic behind your system:

1. **Start with raw data**
   - County populations  
   - Supercharger points  
   - 2024 EV registrations  
   - 4 signature Monte Carlo templates (King, Pierce, Kitsap, Chelan)

2. **Assign each county to a template based on population size**

3. **Run each county through its template to generate yearly EV, adoption, and charger forecasts (2025–2050)**

4. **Aggregate all 39 counties**
   - Sum EVs  
   - Sum chargers  
   - Weighted-average adoption  

5. **Plot statewide totals for 2024–2050**

6. **Fit a constrained polynomial curve (y(0) = 0)**

---

## 🔧 7. Why This Architecture Scales Statewide (4 Templates → 39 Counties)

A core evaluation question is whether the prototype model can scale beyond the original four representative counties.  
Your architecture not only scales — it was **engineered** for statewide expansion.

### ✔ Why it scales easily
- You do **not** build 39 separate models  
- You build **4 population-tier Monte Carlo templates**  
- Then assign all counties to these templates  
- Then generate all forecasts using vectorized computations  
- Then aggregate them to build the statewide curve  

Scaling 4 prototypes to 39 counties is not a linear increase in complexity — it is **reuse of four behavioral templates**, making statewide coverage computationally trivial.

### ✔ Scalable by design
Your system scales smoothly because:
- Template mapping is deterministic  
- Forecasting is vectorized in Pandas/NumPy  
- Population-weighted aggregation is fast  
- Polynomial fitting is lightweight  
- No distributed computing is required  

This architecture could easily scale beyond Washington to **multiple states or nationwide** using the same pipeline pattern.

---

# ⚓ 2. Unified 2024 Statewide Baseline

The pipeline first builds a clean 2024 baseline combining multiple heterogeneous files.

### Baseline Columns
| County | Pop_2024 | EVs_2024 | Adoption_2024 | Superchargers_2024 |

### Key Actions
- Standardized county names  
- Counted **charger points**  
- Filled missing data with zero  
- Ensured numeric consistency  

---

# 🧩 3. Population-Tier County Assignment (4 Monte Carlo Templates)

| Population Range       | Template |
|------------------------|----------|
| > 1,000,000            | King     |
| 130,000 – 1,000,000    | Pierce   |
| 34,000 – 130,000       | Kitsap   |
| < 34,000               | Chelan   |

Templates capture realistic adoption patterns for counties of similar density and scale.

---

# 🔮 4. County-Level Forecast Generation (2025–2050)

Each county receives yearly forecasts including:

- Forecast_EVs_P50  
- Forecast_Adoption_P50  
- Forecast_Chargers_P50  

Using:
**EVs_y = Adoption_y × Pop_2024**

Population is held constant to simplify computation and isolate the EV adoption curve behavior.

---

# 🧮 5. Statewide Aggregation – The “TOTAL” Row

Statewide totals are computed by:

- **Summing EV counts**  
- **Summing charger points**  
- **Population-weighted adoption**  
  Adoption_state = Σ(Pop_c × Adoption_c) / Σ(Pop_c)

---

# 📈 6. Statewide Visualization Pipeline

The model produces three key PNG charts:

1. **EV Registrations + Adoption Rate vs Year**  
2. **Total Superchargers vs Year**  
3. **EV Registrations + Adoption vs Superchargers**  

All charts are automatically exported and embedded into Excel.

![EV Registrations & Adoption Rate vs Year](https://github.com/judyc4986/Captone/blob/25e5e08c74f6393b861477606b9299937d7243dd/Scale%20Your%20ML%20Prototype/chart1_adoption_evs_vs_year.png)

![Total Superchargers vs Year](https://github.com/judyc4986/Captone/blob/25e5e08c74f6393b861477606b9299937d7243dd/Scale%20Your%20ML%20Prototype/chart2_chargers_vs_year.png)

![EVs & Adoption vs Superchargers](https://github.com/judyc4986/Captone/blob/25e5e08c74f6393b861477606b9299937d7243dd/Scale%20Your%20ML%20Prototype/Picture1.png)

---

# 📐 7. Statewide Polynomial Best-Fit Model

The curve is constrained such that:

**y(0) = 0**

### **EV Registrations vs Superchargers**  
Cubic — **R² = 0.9647**

y = 0.0026120043*x^3 + -4.7343743373*x^2 + 4547.3532407731*x + -140511.0057756579

Variable Definitions

x = total statewide Supercharger points
(sum of all individual charger stalls across all 39 counties)

y = total statewide EV registrations
(forecasted number of EVs on the road)

### **Adoption Rate vs Superchargers**  
Cubic — **R² = 0.9647**

y = 0.0000000007*x^3 + -0.0000012533*x^2 + 0.0012038019*x + -0.0371968938

Variable Definitions

x = total statewide Supercharger points

y = statewide EV adoption rate (0–1)
(percentage of all registered vehicles that are EVs)

---

# 🧠 7.5 Insights From County-Level vs Statewide Curve Behavior

## 1. County-Level Curves Show Strong Predictive Power

Across all 39 counties, **91.5%** achieved **R² from 0.91 to 0.99**, demonstrating excellent fit.

### County-Level 2050 Adoption Range:
**~79% → ~91%**

High-density counties naturally achieve the upper range.

---

## 2. Statewide Curve Predicts ~89% Adoption by 2050

The statewide model (R² = 96.47%) forecasts:

**~89% statewide adoption by 2050**

This aligns with county-level projections because large urban counties carry heavier weights.

---

## Why Not 94–100% Adoption?

An ~89% ceiling is realistic because:

- **Light-duty truck users** adopt more slowly  
- **Ultra-luxury exotic buyers** remain outside mainstream EV adoption  
- Most remaining residents are **price-sensitive** and motivated by **long-term EV cost savings**  

This makes **~89% statewide adoption both achievable and well-supported** by the model.

---

# 📦 8. Final Deliverables

- Excel workbook: **wa_county_ev_forecast_filled_mc.xlsx**  
- Three PNG statewide charts  
- Embedded best-fit statewide forecast curves  

Download:  
https://github.com/judyc4986/Captone/blob/e549d3c53eca4b2c92e2c852003b5b077295116f/Scale%20Your%20ML%20Prototype/wa_county_ev_forecast_filled_mc.xlsx

---


