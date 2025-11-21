# Washington State EV Adoption & Charger Forecast (2025–2050)
### **Monotonic Monte-Carlo + Time-Series Policy Anchoring Model**  
Created by **Judy Cheng**

---

## 📌 Overview

This project builds a complete forecasting pipeline for **King, Pierce, Kitsap, and Chelan Counties** to estimate:

- **EV adoption rates** (2025–2050)  
- **EV registrations (vehicle counts)**  
- **Public charger build-out (Lower, Upper, Forecast)**  
- **Uncertainty bands (P10 / P50 / P90)** via Monte-Carlo simulation  

The model uses:
- **2024 baseline EV, charger, and population data**
- **Washington State EV policy targets**
- **Deterministic Lower & Upper time-series paths**
- **Monotonic Monte-Carlo adoption simulation**

---

## 📂 Data Inputs (2024 Baseline)

This project uses three main input files:

| File | Purpose |
|------|---------|
| **Supercharger in Washington.xls** | Supercharger locations by county |
| **coordinates_output.xlsm** | EV VIN registration (filtered to 2024) |
| **Population 2024 age 25 to 59.xlsx** | County population denominator |

These define each county’s:

- Starting EV count  
- Starting charger count  
- Population base (age 25–59) used in adoption rate calculations  

---

## ⚙️ Model Architecture

All county scripts (King, Pierce, Kitsap, Chelan) follow the same **5 Step pipeline**.

---

## **Step 1 — Load Data & Policy Targets**

Load:
- EV counts  
- Charger counts  
- Population denominator  

Apply Washington State EV adoption targets:
- **45% by 2030**  
- **60% by 2035**  
- **70% by 2040**  
- **95% by 2050**

These targets anchor both:
- The deterministic Lower/Upper paths  
- Monte-Carlo mean reversion  

---

## **Step 2 — Charger Target Logic**

For each county, two independent charger requirements are computed:

---

### **1. Lower Bound — Geographic Coverage**

Using **π × r² service area per charger**:

| County | Service Radius |
|--------|----------------|
| **King** | 2 miles |
| **Kitsap** | 5 miles |
| **Chelan** | 15 miles (rural coverage) |

This ensures **minimum geographic accessibility** across the entire county.

---

### **2. Upper Bound — Population Capacity**

Using **residents per charger**:

| County | Residents per Charger |
|--------|------------------------|
| **King** | 1 per 1,500 |
| **Pierce / Kitsap** | 1 per 2,500 |
| **Chelan** | 1 per 5,000 |

This models **capacity / throughput** constraints.

---

### 🔧 Result  
Both bounds create the **scenario envelope** for future charger development.

---

## **Step 4 — Lower / Upper Time-Series Envelopes**

Two deterministic adoption & charger trajectories:

---

### **Lower Scenario**
- Conservative charger build  
- Moderated early adoption  
- Slower Phase-2 growth  
- Represents *minimum feasible adoption path*  

---

### **Upper Scenario**
- Tracks policy milestones  
- Front-loaded charger growth  
- Smooth, faster adoption  
- Represents *optimistic policy compliance*  

---

These form the **outer bounds** for Monte-Carlo simulation.

---

## **Step 5 — Monotonic Monte-Carlo Simulation (1,000 Paths)**

The core of the forecasting model.

### ✔ Key Rules Enforced

- Adoption must **always increase** (monotonic)  
- Adoption must stay **between Lower and Upper** bounds  
- Capped at **94%** to avoid unrealistic overshoot  
- **Charger growth accelerates adoption**  
- **Home-charging share reduces dependence on public chargers**  
- **Momentum** of charger build-out affects adoption slope  
- **Phase-specific noise (σ)** added  
- Yearly **P10 / P50 / P90** percentiles generated  

---

### 📈 Final Outputs

- **P10** — Slow adoption  
- **P50** — Median / most likely  
- **P90** — Aggressive adoption  
- **EV counts** = adoption × population  

---

## 🌟 Why Time-Series + Monte-Carlo?  
### **7-Point Summary**

1. **EV adoption is policy-driven**, not historically driven → TS/MC respects mandated targets.  
2. **Historical dataset is tiny** (<10 points) → ML cannot learn meaningful patterns.  
3. Adoption must be **monotonic and capped** → ML cannot enforce this.  
4. Adoption depends on **future charger expansion**, not past behavior → requires assumptions.  
5. Time-series envelopes provide **explicit scenario control**, unlike ML.  
6. Monte-Carlo gives **uncertainty bounds (P10–P90)** → ML cannot reliably produce these.  
7. The model is **transparent & interpretable** → required for policy, finance, engineering.  

---

## 🚫 Why Machine Learning Does Not Work Here

Machine learning fails because:

- Not enough historical EV adoption data exists → ML would overfit instantly.  
- ML cannot respect **2030/2035/2040 policy deadlines**.  
- ML may predict impossible values (negative adoption, >100%).  
- Infrastructure expansion & future policy do **not exist in the training data**.  
- ML extrapolation across 30 years is mathematically unstable.

### **Therefore:**
This is a **policy + assumptions + infrastructure forecasting** problem — not a prediction problem.  
**Time-Series + Monte-Carlo is the only defensible method.**

---

## 📊  Outputs
This workbook includes:
- Scenarios_LU (Lower vs Upper)
- Forecast (MC P10 / P50 / P90)
- Embedded charts (chargers, adoption, EV registrations)

For each county, the script generates:

[ Chelan County EV Forecast (Excel) ](https://github.com/judyc4986/Captone/blob/main/Experiment%20With%20Various%20Models/chelan_county_ev_projection_mc_monotonic.xlsx)

[ Kitsap County EV Forecast (Excel) ](https://github.com/judyc4986/Captone/blob/main/Experiment%20With%20Various%20Models/kitsap_county_ev_projection_mc_monotonic.xlsx)

[ King County EV Forecast (Excel) ](https://github.com/judyc4986/Captone/blob/main/Experiment%20With%20Various%20Models/king_county_ev_projection_mc_monotonic.xlsx)

[ Pierce County EV Forecast (Excel) ](https://github.com/judyc4986/Captone/blob/main/Experiment%20With%20Various%20Models/pierce_county_ev_projection_mc_monotonic.xlsx)
