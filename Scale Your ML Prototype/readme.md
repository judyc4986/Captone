# WA Statewide EV Forecast – Scaled Implementation Summary

This project scales the original single-county EV forecast prototype into a **statewide, multi-county, fully automated forecasting pipeline**.  
The implementation supports **39 counties**, integrates **Monte Carlo (MC) templates**, performs **statewide aggregation**, and produces **best-fit modeling + visualizations** in Excel and PNG formats.

---

## 1. Scalable Data Ingestion & Standardization

To integrate population, EV VIN registrations, and supercharger point data, the pipeline builds a unified **2024 statewide baseline dataset**.

### **Key decisions**
- Standardized county names across heterogeneous sources.  
- Converted charger *rows* → charger *points* for accurate aggregation.  
- Baseline structure:  
County | Pop_2024 | Superchargers_2024 | EVs_2024 | Adoption_2024

### **Trade-offs**
- Missing values are filled with zero to maintain a clean and scalable baseline.  
- Unified schema simplifies all downstream integration and processing.

---

## 2. Scalable Monte Carlo Forecasting (Population-Tier Templates)

Instead of building unique MC models for each county, the system uses **four shared MC templates**, assigned by 2024 population tiers:

| Population Range       | Template |
|------------------------|----------|
| > 1,000,000            | King     |
| 130,000 – 1,000,000    | Pierce   |
| 34,000 – 130,000       | Kitsap   |
| < 34,000               | Chelan   |

### **Key decisions**
- Automatically assigns counties to one of the four templates.  
- Pulls yearly values for:  
`Forecast_Chargers`, `Forecast_Adoption_P50`, and `Forecast_EVs_P50`.

### **Trade-offs**
- Reduces county-specific variation but achieves powerful statewide scalability.

---

## 3. Automated Forecast Filling (2025–2050)

The system iterates through all counties and populates yearly forecast values for EVs, chargers, and adoption.

### **Key decisions**
- EV projections are calculated as:  
EVs_y = Adoption_y × Pop_2024
- All forecast columns are converted to numeric to prevent downstream errors.

### **Trade-offs**
- Fixing population at 2024 levels removes demographic complexity but greatly simplifies computation and scale.

---

## 4. Statewide Aggregation (TOTAL Row)

A final **TOTAL** row is appended to the dataset to provide a statewide rollup across all counties.

### **Key decisions**
- Sum all EVs and charger counts across counties.  
- Use a population-weighted formula for statewide adoption:
Adoption_state = ( Σ(Pop_c × Adoption_c) ) / ( Σ(Pop_c) )

### **Trade-offs**
- Slightly higher computation cost, but ensures a statistically correct statewide adoption rate.

---

## 5. Visualization Pipeline (Three Statewide Charts)

The pipeline automatically generates and embeds the following charts into Excel as PNG images:

1. **EV Registrations & Adoption Rate vs Year (2024–2050)**  
2. **Total Superchargers vs Year (2024–2050)**  
3. **EV Registrations + Adoption vs Total Superchargers (with best-fit curves)**

### **Key decisions**
- Scatter-based plots retain yearly variance instead of smoothing real variation.  
- Automated PNG export and Excel sheet insertion using `openpyxl`.

### **Trade-offs**
- Slight increase in file input/output overhead, but ensures reproducible, presentation-ready outputs.

---

## 6. Polynomial Best-Fit Modeling (Constrained to y(0)=0)

The system models the statewide relationship between charger density (x) and EV outcomes (y) using polynomial fits constrained to:

\[
y(0) = 0
\]

### **Does the fitted curve include 2024 data?**
Yes.  
The regression model is trained on **all yearly points from 2024 through 2050**.  
The lists used in the fit (`x`, `evs`, `adopt`) are constructed from:

years = list(range(2024, 2051))

This means the first point in every series is the **2024 baseline value**, and it is fully included in the regression fitting and best-fit curve generation.

### **Selected Best-Fit Equations**

#### 📈 **EV Registrations vs Total Superchargers**
**Best Fit:** Cubic (through origin)  
**R²:** 0.9641  

y = 3773.495357*x  -  3.578990*x^2  +  0.002106*x^3


---

#### 📈 **Adoption Rate vs Total Superchargers**
**Best Fit:** Cubic (through origin)  
**R²:** 0.9641  

y = 0.000999*x  -  0.000001*x^2  +  0.000000*x^3




---

## 7. Final Reporting Assembly

The pipeline produces a complete statewide deliverable:

- Final dataset: **`wa_county_ev_forecast_filled_mc.xlsx`**
- 2024 baseline + 2025–2050 Monte Carlo forecasts  
- TOTAL statewide aggregation row  
- Three PNG charts  
- Best-fit formulas embedded directly into the “Charts” Excel sheet  

### 📂 Download Final Forecast Output  
The complete statewide EV forecast (2024 baseline + 2025–2050 P50 projections) is available here:

👉 **[wa_county_ev_forecast_filled_mc.xlsx](https://github.com/judyc4986/Captone/blob/e549d3c53eca4b2c92e2c852003b5b077295116f/Scale%20Your%20ML%20Prototype/wa_county_ev_forecast_filled_mc.xlsx)**

### **Result**
A fully automated, scalable statewide forecasting system capable of updating seamlessly when new datasets, templates, or counties are introduced.

---

## 8. Included Charts (PNG Outputs)

These PNGs are exported automatically and inserted into Excel.

## 1. EV Registrations & Adoption Rate vs Year (2024–2050)
![EV Registrations & Adoption Rate vs Year](https://github.com/judyc4986/Captone/blob/637caee816078d1ab08b038b1f5dedcabb722421/Scale%20Your%20ML%20Prototype/chart1_adoption_evs_vs_year.png)

## 2. Total Superchargers vs Year (2024–2050)
![Total Superchargers vs Year](https://github.com/judyc4986/Captone/blob/637caee816078d1ab08b038b1f5dedcabb722421/Scale%20Your%20ML%20Prototype/chart2_chargers_vs_year.png)

## 3. EV Registrations + Adoption Rate vs Total Superchargers
![EVs & Adoption vs Superchargers](https://github.com/judyc4986/Captone/blob/637caee816078d1ab08b038b1f5dedcabb722421/Scale%20Your%20ML%20Prototype/chart3_adopt_evs_vs_chargers.png)


---
