# 🌎 Washington State EV Forecast – Full Scaled Pipeline (2024–2050)

This project transforms a small **4-county prototype** into a **statewide EV forecasting system** that supports all **39 Washington counties**, assigns them to **population-tier Monte Carlo templates**, aggregates county-level outputs into a **statewide forecast**, and fits a **polynomial statewide model** to determine future charger requirements.

The end result is an **automated forecasting engine** that produces Excel outputs, PNG charts, and an official **statewide best-fit EV forecast curve**.

---

## 1. How the Forecast Works (High-Level Flow)

To avoid confusion, here is the real logic behind your system:

1. **Start with raw data**
   - County populations  
   - Supercharger points  
   - 2024 EV registrations  
   - 4 signature MC templates (King/Pierce/Kitsap/Chelan)  

2. **Assign each county to a template based on population size**

3. **Run each county through its template to generate yearly EV, adoption, and charger forecasts (2025–2050)**

4. **Aggregate all 39 counties**
   - Sum EVs  
   - Sum chargers  
   - Weighted-average adoption  

5. **Plot statewide totals for 2024–2050**

6. **Fit a constrained polynomial curve (y(0)=0)**  

7. **Use the fitted curve to answer infrastructure planning questions**

---

## 2. Unified 2024 Statewide Baseline

The pipeline first builds a clean 2024 baseline combining multiple heterogeneous files.

### Baseline Columns
| County | Pop_2024 | EVs_2024 | Adoption_2024 | Superchargers_2024 |

### Key Actions
- Standardized all county names  
- Counted **charger points**  
- Filled missing data with zero  
- Ensured numeric consistency  

---

## 3. Population-Tier County Assignment (4 Monte Carlo Templates)

| Population Range       | Template |
|------------------------|----------|
| > 1,000,000            | King     |
| 130,000 – 1,000,000    | Pierce   |
| 34,000 – 130,000       | Kitsap   |
| < 34,000               | Chelan   |

Templates capture realistic adoption curves for each population tier while maintaining scalability.

---

## 4. County-Level Forecast Generation (2025–2050)

Each county receives yearly forecasts including:

- Forecast_EVs_P50  
- Forecast_Adoption_P50  
- Forecast_Chargers_P50  

Using:
EVs_y = Adoption_y × Pop_2024

Population fixed at 2024 simplifies computation.

---

## 5. Statewide Aggregation – The “TOTAL” Row

Statewide totals are computed by:

- Summing EV counts  
- Summing charger points  
- Weighted-average adoption:

Adoption_state = Σ(Pop_c × Adoption_c) / Σ(Pop_c)

---

## 6. Statewide Visualization Pipeline

Three PNG charts:

1. EV Registrations + Adoption Rate vs Year  
2. Total Superchargers vs Year  
3. EV Registrations + Adoption vs Superchargers  

Exported and embedded into Excel.

---

## 7. Statewide Polynomial Best-Fit Model

Constrained to:

y(0) = 0

### EV Registrations vs Superchargers  
Cubic — **R² = 0.9647**

y = 0.0026120043*x^3 + -4.7343743373*x^2 + 4547.3532407731*x + -140511.0057756579

- **x** = total statewide **Supercharger points**  
- **y** = total statewide **EV registrations**  
This model answers: “If WA has x chargers, how many EVs (y) can the system support?”

### Adoption Rate vs Superchargers  
Cubic — **R² = 0.9647**

y = 0.0000000007*x^3 + -0.0000012533*x^2 + 0.0012038019*x + -0.0371968938

- **x** = total statewide **Supercharger points**  
- **y** = statewide **EV adoption rate** (0–1)  
This model answers: “If WA has x chargers, what adoption rate (y) can be reached?"
---

## ⭐ Why This Is a Good Forecast

### **1. Real county-level behavior is preserved**
Population-tier templates capture urban, suburban, and rural differences without needing 39 individual models.

### **2. County → State aggregation is statistically correct**
Statewide adoption is computed using **population-weighted logic**, not simple averaging.

### **3. Anchored in real 2024 baselines**
All forecasts begin from true 2024 EV counts, charger points, and populations.

### **4. Polynomial curve reflects real-world adoption dynamics**
The cubic shape reflects diminishing returns and early steep growth — consistent with real EV adoption behavior.

---

## 🔍 What the R² Value Means

R² (coefficient of determination) measures how well the fitted curve explains the actual data.

### **R² = 0.9641 means:**

- **96.41% of the variation** in EV outcomes is explained by charger availability  
- The model captures the **true underlying pattern** extremely well  
- Only ~3.6% of variation is noise or outside influences  
- This level of fit is considered **excellent** in EV forecasting and infrastructure planning  

### Bottom Line  
A high R² confirms that the statewide cubic curve is both **statistically strong** and **practically meaningful**, making it suitable for long-term charger build-out planning.

---


## 8. Final Deliverables

- **wa_county_ev_forecast_filled_mc.xlsx**  
- Three PNG charts  
- Best-fit curves embedded in Excel  

📂 Download:  
https://github.com/judyc4986/Captone/blob/e549d3c53eca4b2c92e2c852003b5b077295116f/Scale%20Your%20ML%20Prototype/wa_county_ev_forecast_filled_mc.xlsx

---

## 9. Included Charts

## 1. EV Registrations & Adoption Rate vs Year (2024–2050)
![EV Registrations & Adoption Rate vs Year](https://github.com/judyc4986/Captone/blob/637caee816078d1ab08b038b1f5dedcabb722421/Scale%20Your%20ML%20Prototype/chart1_adoption_evs_vs_year.png)

## 2. Total Superchargers vs Year (2024–2050)
![Total Superchargers vs Year](https://github.com/judyc4986/Captone/blob/637caee816078d1ab08b038b1f5dedcabb722421/Scale%20Your%20ML%20Prototype/chart2_chargers_vs_year.png)

## 3. EV Registrations + Adoption Rate vs Total Superchargers
![EVs & Adoption vs Superchargers](https://github.com/judyc4986/Captone/blob/637caee816078d1ab08b038b1f5dedcabb722421/Scale%20Your%20ML%20Prototype/chart3_adopt_evs_vs_chargers.png)

---


