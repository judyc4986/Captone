# 📊 Population and EV Datasets

This folder contains the **core datasets** used in the **Washington State EV Infrastructure Analysis and Forecasting Capstone Project**.

---

## 📂 Dataset Overview

### 1. 🏁 Supercharger Charge Points in the U.S.
**File:** `supercharger_in_washington_state.xls`  
**Source:** [Tesla Supercharger Network Map](https://www.tesla.com/supercharger)  
**Description:**  
Contains location and metadata for Tesla Supercharger sites in Washington and across the U.S. Used for infrastructure density and proximity analysis.

**Key Columns**
| Column | Description |
|--------|--------------|
| `Name` | Station name |
| `Address` | Physical address |
| `State`, `County`, `City` | Geographic identifiers |
| `Latitude`, `Longitude` | GPS coordinates |
| `Stalls` | Number of charging stalls |
| `Status` | Station operational status (Open / Coming Soon) |

---

### 2. 👥 Washington State Population by Age and County
**File:** `Population_2024_age_25_to_59.xlsx`  
**Source:** Washington State Office of Financial Management (OFM), U.S. Census Bureau  
**Description:**  
Provides population estimates by **county and age group** (25–59), representing the primary working-age segment for EV adoption forecasting.

**Key Columns**
| Column | Description |
|--------|--------------|
| `County` | County name |
| `Total` | Total population aged 25–59 |
| `Year` | Data year or projection year |

---

### 3. 🔋 EV VINs Registration (Battery Electric Vehicles)
**File:** `coordinates_output.xlsm`  
**Source:** Washington State Department of Licensing (DOL) — Electric Vehicle Registration Data  
**Description:**  
Anonymized sample of **Battery Electric Vehicle (BEV)** VIN registrations with geographic coordinates for mapping EV ownership density.

**Key Columns**
| Column | Description |
|--------|--------------|
| `VIN` | Partial, anonymized Vehicle Identification Number |
| `Make`, `Model`, `Year` | Vehicle attributes |
| `County`, `State` | Registration location |
| `Latitude`, `Longitude` | Geolocation of registration |

---

## 🧭 Usage

These datasets are used by scripts in the `/scripts` folder to:
- Estimate **EV adoption rates** by county  
- Forecast **charger expansion** under lower and upper-bound policy scenarios  
- Visualize **charger density vs. population distribution**

**Example Load Script**
```python
import pandas as pd

super_df = pd.read_excel("data/raw/supercharger_in_washington_state.xls")
residents_df = pd.read_excel("data/raw/Population_2024_age_25_to_59.x_
