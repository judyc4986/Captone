# EV Adoption & Charging Forecasting System – Flask Deployment Plan
Prepared by: Judy Cheng  
Capstone Project – EV Forecasting & Analytics

## Overview
This document describes the **step-by-step deployment plan** for the EV Adoption & Charging Forecasting System. The application will be deployed as a Flask web service using **free hosting** (Render.com Free Tier). The system provides two forecasting modes:

1. Statewide forecasting – using a best-fit cubic regression model  
2. County-level forecasting – using 39 county-specific regression models

## Deployment Objectives

### Goals
- Enable real-time EV forecasting through a simple web interface  
- Charger input → EV/adoption output  
- Support statewide and county-level modes  
- Deploy using **free Flask hosting (Render.com)**  

### Why Flask?
- Lightweight, minimal overhead  
- Python-native  
- Simple routing + API creation  
- Easy deployment to cloud platforms like Render  

## System Architecture Overview

```
                    +----------------------+
                    |    Flask Web App     |
                    +----------+-----------+
                               |
     +-------------------------+-------------------------+
     |                                                   |
+----v----+                                       +------v------+
| Statewide Model JSON        |                  | County Models JSON |
| statewide_model.json        |                  | county_models.json |
+---------+-------------------+                  +---------+---------+
          |                                               |
          +----------------------+------------------------+
                                 |
                        +--------v--------+
                        | Prediction      |
                        | Engine          |
                        | (Polynomial)    |
                        +-----------------+
```

## Statewide Flask Deployment – Step-by-Step

1. Extract statewide regression coefficients  
2. Save coefficients into `statewide_model.json`  
3. Build Flask route `/predict/statewide`  
4. Create input form (total chargers)  
5. Connect prediction engine  
6. Render results UI  
7. Add API endpoint `/api/statewide`  
8. Deploy on **Render.com free tier**  

## County-Level Flask Deployment – Step-by-Step

1. Compute 39 county-level regression models  
2. Save into `county_models.json`  
3. Build route `/predict/county`  
4. Add county dropdown  
5. Add charger input box  
6. Load correct county model dynamically  
7. Predict EV + adoption rate  
8. Add JSON endpoint `/api/county`  
9. Deploy on **Render.com free tier**  

## Forecast Workflow Diagram

```
User Input (Statewide or County + Chargers)
                    |
                    v
            Flask Application
       (Loads correct model / route)
                    |
                    v
          Prediction Engine
         (Polynomial Regression)
                    |
                    v
         HTML Results OR JSON API
```

## Deployment Options

- **Render.com Free Tier (Recommended)**
- Local Flask Development
- AWS EC2 (advanced)
- Docker (optional)

## Deliverables

- Flask app (statewide + county)
- JSON model files
- README.md (this file)
- Screenshots of UI
- Public free deployment URL (Render.com)

## Notes
- This README describes the **plan steps** for deploying the EV forecasting application.
- Diagrams are simplified for mentor review.
