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


## 📍 County Descriptions & Why These Four Counties Were Selected

This project uses **King, Pierce, Kitsap, and Chelan Counties** because together they represent the full spectrum of EV adoption environments in Washington State.  
Each county reflects different combinations of population density, land area, commuting patterns, and EV market maturity.

### **King County — Urban / High Density**
- Most mature EV market in Washington  
- Dense population & high charger demand  
- Ideal for modeling *urban saturation dynamics*

### **Pierce County — Urban/Suburban Mix**
- Contains both major cities and suburban communities  
- Growing EV presence  
- Represents *middle-tier EV expansion patterns*

### **Kitsap County — Suburban / Commuter Region**
- Heavy commuter flow to King County  
- EV adoption shaped by ferry routes + suburban travel  
- Useful for modeling *suburban charger dependency*

### **Chelan County — Rural / Forest Region**
- Large land area, dispersed towns, long rural corridors  
- Limited EV penetration but high geographic travel needs  
- Critical for modeling *rural coverage requirements* and large-radius charger assumptions*

---

### 🎯 **Purpose of Selecting These Counties**

These counties allow the forecasting model to analyze:

#### **1. EV Adoption Across Diverse Region Types**
- Urban  
- Suburban  
- Commuter-based  
- Rural  

#### **2. Charger Planning Based on Geography & Population**
Different regions require different strategies:
- Urban → congestion and capacity  
- Suburban → commuter and residential mix  
- Rural → geographic coverage & travel corridors  

This ensures the model captures **both population-based and land-area–based** charger requirements.


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

## 🔁 Phase 1–3: Home-Charging Shift & Tesla Supercharger Strategy

Washington State’s policy assumes **80% home-charging reliance by 2030**.  
Each phase reflects how Tesla’s Supercharger strategy and the market respond to this transition.

### 🔵 Phase 1 — 2025–2030: “Push Everything” Growth  
- Strong expansion of **Superchargers** + rapid growth in **home charging**  
- Goal: remove early charging anxiety and drive EV sales  
- Adoption climbs quickly toward the **45% 2030 target**  
- MC paths lean closer to the **Upper bound**

### 🟡 Phase 2 — 2030–2035: Adjustment After Home-Charging Shift  
- Market digests reaching **80% home-charging**  
- Public charger growth slows; system rebalances  
- Adoption curve intentionally **flattens**  
- MC band widens (higher uncertainty during adjustment)

### 🟠 Phase 3 — 2035–2050: Closing Coverage Gaps & Reaching 95%  
- Market less sensitive to home-charging share  
- Tesla expands Superchargers again to eliminate residual range anxiety  
- Adoption accelerates toward **95%**  
- MC paths converge toward the cap (~94%)

---

## **Step 5 — Monotonic Monte-Carlo Simulation**

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


## 🔍 Insights From the Forecast Results (Cross-County Comparison)

The Monte-Carlo outputs reveal meaningful behavioral patterns across all four counties.  
Even though the model architecture is identical for every region, differences in population, land area, and charger requirements produce distinct adoption curve shapes.

---

### **1. King County & Pierce County — Similar EV Growth Behavior**

King and Pierce show **very similar Monte-Carlo adoption curves** because both counties share urban characteristics:

- High population density  
- Strong baseline EV ownership  
- Stable, predictable charger utilization  
- Faster response to Phase 1 and Phase 3 charger expansion  
- Tighter P10–P90 uncertainty bands  

**Insight:**  
Urban counties exhibit **smooth, fast, policy-aligned growth**, with adoption trajectories strongly guided by policy anchors rather than geographic constraints.

---

### **2. Kitsap County — Suburban Growth With a Noticeable Curve Difference**

Kitsap follows the general pattern of King and Pierce but with clear differences:

- Smaller resident population  
- Greater land area relative to population  
- Higher sensitivity to charger geographic coverage early on  
- Adoption in Phase 2 flattens more noticeably  
- Phase 3 acceleration is slightly delayed  

**Insight:**  
Kitsap displays a **hybrid pattern** — positioned between urban and rural behavior — reflecting commuter dynamics, ferry travel, and suburban EV distribution.

---

### **3. Chelan County — Most Distinct Curve, But Same Overall Trend**

Chelan behaves differently from the other three counties due to:

- Very low population density  
- Very large land geography (mountains, forest, rural towns)  
- Much larger required charger service radius (15 miles)  
- Early adoption heavily constrained by geographic coverage  
- Wider uncertainty bands in Phase 1 and Phase 2  

Even so, Chelan still follows the **same long-term trajectory**:

- Rising Phase 1  
- Flattening Phase 2  
- Accelerating Phase 3 toward saturation  

**Insight:**  
Chelan demonstrates how **rural counties depend strongly on public charging infrastructure** in early years, but eventually converge toward statewide policy targets by 2050.

---

### **4. Overall Conclusion Across All Counties**

Despite their differences, all counties show:

- Monotonic adoption growth  
- Alignment with 2030 / 2035 / 2040 / 2050 targets  
- Convergence toward the 95% long-term adoption cap  
- Similar three-phase structure with different slope intensity  

**Insight:**  
The model successfully captures both **statewide policy uniformity** and **regional diversity**, showing that EV adoption is universal but shaped by local population density, land area, charger coverage needs, and home-charging reliance.



[ Chelan County EV Forecast (Excel) ](https://github.com/judyc4986/Captone/blob/main/Experiment%20With%20Various%20Models/chelan_county_ev_projection_mc_monotonic.xlsx)

[ Kitsap County EV Forecast (Excel) ](https://github.com/judyc4986/Captone/blob/main/Experiment%20With%20Various%20Models/kitsap_county_ev_projection_mc_monotonic.xlsx)

[ King County EV Forecast (Excel) ](https://github.com/judyc4986/Captone/blob/main/Experiment%20With%20Various%20Models/king_county_ev_projection_mc_monotonic.xlsx)

[ Pierce County EV Forecast (Excel) ](https://github.com/judyc4986/Captone/blob/main/Experiment%20With%20Various%20Models/pierce_county_ev_projection_mc_monotonic.xlsx)
