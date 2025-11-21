# Washington State EV Forecasting Project
### *Dataset Cleaning, Profiling & Forecasting Pipeline*

## 📘 Overview
This repository contains the full workflow for preparing, cleaning, profiling, and forecasting **county-level EV adoption and charging-infrastructure requirements** for Washington State.  
Three primary datasets are cleaned and merged to produce 2025–2050 EV and charger build-out projections for King, Pierce, Kitsap, and Chelan counties.
 

## 📂 Project Datasets
| Dataset | File Name | Purpose |
|--------|------------|---------|
| Tesla Supercharger Network | supercharger in washington state.xls | Existing supercharger count |
| Population (Age 25–59) | Population 2024 age 25 to 59.xlsx | Demographic driver |
| EV VIN Registration Data | coordinates_output.xlsm | EV counts by county |

## ⚙️ Environment Setup
```bash
pip install pandas ydata-profiling openpyxl xlrd
```

## 🧪 Dataset Profiling (EDA)
Generated HTML reports:
- Report_Supercharger_Profile.html  
- Report_Population_Profile.html  
- Report_EV_VIN_Profile.html  

## 🧽 Cleaning & Structuring the Datasets
### Tesla Supercharger Dataset
- Filtered WA rows  
- Normalized county names  
- Validated coordinates  
- Counted superchargers  

### Population Dataset
- Selected population column  
- Standardized county names  
- Verified numeric integrity  

### EV VIN Registration Dataset
- Filtered WA EVs  
- Cleaned invalid VINs  
- Standardized county names  
- Counted EV registrations  

## 🔧 Preparing Final Inputs for Forecasting Models
### Policy Adoption Anchors
| Year | Target EV Adoption |
|------|--------------------|
| 2030 | 45% |
| 2035 | 60% |
| 2040 | 70% |
| 2050 | 95% |

### Lower Bound — Geo-Coverage Model
- Based on charger radius  
- Even annual build-out  

### Upper Bound — Population-Based Model
- Based on residents-per-charger  
- Fast → moderate → slow rollout  

## 📈 Forecast Output Files
Examples:
- king_county_ev_projection_constant_lower.xlsx  
- pierce_county_ev_projection_constant_lower.xlsx  
- kitsap_county_ev_projection_constant_lower.xlsx  
- chelan_county_ev_projection_2025_2050.xlsx  

## 📊 Excel Workbook Structure
### Sheet 1 — Forecast Table
- EV adoption rate  
- EV counts  
- Required chargers  

### Sheet 2 — Charts
- EV adoption chart  
- EV count chart  
- Charger build-out chart  

## ✅ Summary
This workflow ensures:
- Clean, standardized datasets  
- Reliable county-level forecasting  
- Automated charts and structured output  
- Consistent modeling from 2025–2050  
