# 🚗⚡ Tesla EV Growth Strategy · Washington EV Hub
Live Site → **https://home-page-ev.onrender.com/**

A forecasting system built to model how **charging infrastructure, population density, and EV adoption** shape Washington State’s transition toward a sustainable electrified future.

---

# 🌟 Why the EV Forecasting System Is Useful

The EV transition depends not only on vehicle availability but also on whether drivers have **reliable access to charging infrastructure**. Washington State’s climate goals of reducing greenhouse gas emissions by **45% by 2030**, **70% by 2040**, and **95% by 2050** require accelerating EV adoption.

This platform helps planners understand how **charging availability + population density + EV demand** interact and influence statewide electrification success.

---

# 🗺️ 1. Visualizing Population Density vs. Charging Availability

The statewide GIS map overlays population density with existing supercharger locations, showing:

- Counties with **high population but insufficient charging**
- Regions where scarcity creates **high charging anxiety**
- **Uneven distribution** caused by corridor-first infrastructure planning

This reveals counties where adding chargers produces the **highest impact**.

---

# 💰 2. Budget Allocation & County Prioritization

Because EV adoption increases with charging access, the system helps planners:

- Allocate budgets based on **population density**
- Prioritize counties where chargers yield **maximum EV adoption**
- Compare **ROI** of charger deployment

Example insight:

> **“County A needs more chargers due to high density and demand, while County B gains major adoption increases from smaller expansions.”**

---

# 📈 3. Forecast Curves Predict Charging Needs to Meet Policy Goals

Pre-generated cubic and logistic formulas derived from Monte Carlo simulations allow the system to forecast:

- How EV registrations change with added chargers  
- How adoption rates shift across counties  
- Charger counts required to meet **2030–2050 climate mandates**  
- Which counties are **on track** or **falling behind**

Example insights:

- _“Adding 20 superchargers in King County measurably increases EV registrations.”_
- _“WA needs 100+ new chargers statewide by 2030.”_

---

# 🎯 4. Practical Tool for Planners, Utilities & EV Strategists

The platform combines:

- GIS mapping  
- Monte Carlo modeling  
- Curve-fitting formulas  
- Interactive forecasting  

It supports:

- Transparent communication  
- Data-driven planning  
- Identification of underserved communities  
- Policy and infrastructure decision-making  

---

# ✅ Summary

This forecasting system:

- Identifies infrastructure gaps  
- Guides budget allocation  
- Predicts EV adoption impacts  
- Supports Washington climate mandates  
- Centralizes forecasting inspired by Tesla infrastructure strategy  

---

# 🛠️ 1. API Documentation

## 1.1 Base Route
```
GET /
```

---

## 1.2 Statewide Forecast Tool
```
GET /statewide
POST /statewide
```
Loads statewide cubic formulas from Excel and predicts EV metrics.

Data source:
```
static/data/County-level formula_cleaned.xlsx
```

---

## 1.3 County-Level Forecast Tool
```
GET /county
POST /county
```
Retrieves county-specific cubic & logistic formulas from Excel.

---

## 1.4 Static Asset Endpoints
```
/chart/<filename>
/map/<filename>
```
Serves pre-rendered PNG charts and GIS maps.

---

# 🔧 1.5 Forecast Evaluation Engine

Formulas were created offline using cubic regression (EVs vs. Superchargers) and logistic fits (Adoption Rate vs. Superchargers). During runtime, the app **evaluates** these formulas from Excel.

Example formula in Excel:

```
y = 0.0021*x**3 - 4.7*x**2 + 4500*x - 140000
```

### evaluate_formula(formula_str, x_value)

```python
def evaluate_formula(formula_str, x_value):
    """
    Evaluates a pre-computed polynomial/logistic forecasting formula.

    These formulas were generated previously during the model scale-up phase
    using cubic regression (EVs vs Superchargers) and logistic fits
    (Adoption Rate vs Superchargers), and then exported into Excel files.

    Parameters
    ----------
    formula_str : str
        Cleaned mathematical expression from the Excel file.
        Example: "y = 0.0021*x**3 – 4.7*x**2 + 4500*x – 140000"

    x_value : float
        Number of supercharger points input by user.

    Returns
    -------
    float
        The predicted EV registration or adoption value.
    """
```

This enables fast, transparent, reproducible forecasting.

---

# 🧩 2. Installation & Build Instructions

## 2.1 Clone Repositories
```bash
git clone https://github.com/judyc4986/ev_home_hub
git clone https://github.com/judyc4986/ev_render_app
git clone https://github.com/judyc4986/ev_forecast_app
```

---

## 2.2 Create Virtual Environment
```bash
python3 -m venv venv
source venv/bin/activate
```

---

## 2.3 Install Requirements
```bash
pip install -r requirements.txt
```

---

## 2.4 Run Apps Locally
```bash
python app.py
```

Local access:  
👉 http://127.0.0.1:5000/

---

# 🚀 2.5 Deploy to Render

### Build Command
```
pip install -r requirements.txt
```

### Start Command
```
gunicorn app:app
```

---

# 📄 User Demonstration Guide (PDF)

A downloadable **User Demonstration Guide** is available for stakeholders who want a full walkthrough.

The PDF includes:

- System overview  
- GIS + model methodology  
- Statewide & county-level forecasting examples  
- Charger deployment scenario planning  
- Deployment architecture diagrams  

👉 **Download:** `User_Demonstration_Guide.pdf`

https://github.com/judyc4986/Captone/blob/558f20b6941a01763682c19f325467c227360d17/Deployment/Deploy%20to%20Production/User%20Demonstration%20Guide.pdf

---
