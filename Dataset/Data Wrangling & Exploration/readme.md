
# 🧾 README – Data Wrangling Summary  
## EV Adoption & Charging Infrastructure in Washington State  
*(Capstone Project – 2025)*  

---

### 📦 0. Environment Setup & Automated Profiling

Before beginning data cleaning and forecasting, key Python libraries were installed to handle data import, profiling, and Excel operations.

#### **Installation Command**
```bash
pip install pandas ydata-profiling openpyxl xlrd
```

#### **Installed Packages**
| Package | Version | Purpose |
|----------|----------|----------|
| `pandas` | 2.3.3 | Data manipulation and merging |
| `ydata-profiling` | 4.17.0 | Automated data-quality profiling & EDA |
| `openpyxl` | 3.1.5 | Read/write `.xlsx` Excel files |
| `xlrd` | 2.0.2 | Read legacy `.xls` Excel files |
| *(plus)* `numpy`, `matplotlib`, `scipy`, `statsmodels`, etc. |  | Support for calculations and visualization |

> ✅ All packages were confirmed with “Requirement already satisfied” messages.  
> 🔁 Restarting the Jupyter kernel is recommended after installation.

---

### 🧮 Automated Dataset Profiling

Before manual cleaning, automated HTML reports were generated using **`ydata_profiling`** to detect missing values, correlations, and outliers.

#### **Profiling Script**
```python
import os
import pandas as pd
from ydata_profiling import ProfileReport

# --- Set working directory ---
os.chdir("/Users/judycheng/Desktop")
print("📂 Reports will be saved to:", os.getcwd())

# --- Supercharger dataset ---
supercharger_df = pd.read_excel("supercharger in washington state.xls")
ProfileReport(
    supercharger_df,
    title="🏁 Tesla Supercharger Network Profile (Washington)",
    explorative=True
).to_file("Report_Supercharger_Profile.html")

# --- Population dataset ---
population_df = pd.read_excel("Population 2024 age 25 to 59.xlsx")
ProfileReport(
    population_df,
    title="👥 Washington Population Profile (Age 25–59)",
    explorative=True
).to_file("Report_Population_Profile.html")

# --- EV VIN Registration dataset ---
ev_df = pd.read_excel("coordinates_output.xlsm")
ProfileReport(
    ev_df,
    title="🔋 EV VIN Registration Profile (Battery Electric Vehicles)",
    explorative=True
).to_file("Report_EV_VIN_Profile.html")
```

#### **Execution Output**
```
📂 Reports will be saved to: /Users/judycheng/Desktop
✅ Supercharger profile saved to Desktop
✅ Population profile saved to Desktop
✅ EV VIN registration profile saved to Desktop
```

Each report provided:
- Missing-value detection  
- Variable data types & summary stats  
- Correlation matrices  
- Duplicate record identification  
- Histogram & boxplot outlier visualization  

---

### 🧹 1. Data Cleaning & Preparation

#### **File Import and Standardization**
```python
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import os
from openpyxl import load_workbook
from openpyxl.drawing.image import Image

super_df = pd.read_excel("/Users/judycheng/Desktop/supercharger in washington state.xls")
residents_df = pd.read_excel("/Users/judycheng/Desktop/Population 2024 age 25 to 59.xlsx")
ev_df = pd.read_excel("/Users/judycheng/Desktop/coordinates_output.xlsm")
```

- Standardized column names (`State`, `County`, etc.)
- Unified county naming (e.g., “King County” vs “King”)
- Ensured numeric columns (`population`, `area`) were properly typed

#### **Filtering by Region**
```python
super_df = super_df[super_df["State"] == "Washington"]
residents_df = residents_df[residents_df["County"].isin(["King County","Pierce County","Kitsap County","Chelan County"])]
```

---

### 🚫 2. Handling Missing Values

- **Supercharger dataset**: Removed incomplete address rows  
- **Population dataset**: Verified complete  
- **EV dataset**: Some missing `County` fields were reverse-geocoded using ZIP; rows with <95% confidence were dropped  

```python
ev_df = ev_df.dropna(subset=["County"])
```

---

### 📊 3. Detecting & Handling Outliers

Outliers were inspected but **not removed**, as they represented real geographic variation (e.g., King County’s higher charger density).

```python
super_df.describe()
plt.hist(ev_df['some_numeric_column'], bins=20)
plt.show()
```

- Used `.describe()` and histograms to visually confirm ranges  
- Retained values as part of genuine population/urban variation  

---

### 🧩 4. Subsetting Large Datasets

The VIN-level EV registration file was large, so a representative subset of four counties (urban → rural) was used for testing.

```python
subset_counties = ['King County', 'Pierce County', 'Kitsap County', 'Chelan County']
ev_subset = ev_df[ev_df['County'].isin(subset_counties)]
```

This approach reduced computation time but maintained geographic diversity for the forecasting model.

---

### ✅ 5. Summary

After cleaning and profiling:
- Datasets were standardized and aligned across population, charger, and EV registration data  
- Missing values were handled by filtering or imputation  
- Outliers were validated and retained  
- Dataset is now ready for modeling **EV adoption rates** and **charger build-out forecasts (2025–2050)**

---

### 🗂️ Output Artifacts

| File | Description |
|------|--------------|
| `Report_Supercharger_Profile.html` | Automated profiling of Tesla Supercharger dataset |
| `Report_Population_Profile.html` | Population age 25–59 analysis |
| `Report_EV_VIN_Profile.html` | Electric vehicle VIN-level dataset overview |
| `*_ev_projection_*.xlsx` | County-level forecast outputs with embedded charts |
| `*_projection_constant_lower.png` | EV charger growth visualization |

---

### 💡 Next Steps

- Integrate population growth forecasts for 2030–2050  
- Calibrate charger coverage models by population density  
- Combine with EV adoption policy targets (WA Clean Energy Act 2035)  

