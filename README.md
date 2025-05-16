# Mapping Agricultural Flood Damage in Bangladesh  
**Spring 2025 | Data Science for Public Policy**  
**Hertie School**  
**Authors:** Varvara Ilyina, Monserrat Lopez Perez, Aditya Narayan Rai, Manjiri Girish Satam, Isabella Urbano-Trujillo  
**Date:** May 16, 2025

---

## Overview

Floods are a recurrent threat to agriculture in Bangladesh, with remote regions suffering delayed damage assessments and inefficient insurance payouts. This project develops a machine learning pipeline to detect flood-induced vegetation damage using **Sentinel-1 SAR** and **Sentinel-2 optical imagery**. We focus on the Jamalpur district, a highly flood-prone region, and evaluate both **Random Forest** and **XGBoost** classifiers for pixel-level damage prediction.

---

## Data & Preprocessing

We accessed satellite data via the **Sentinel Hub API** for two time windows:

- **Pre-Flood:** June 15–25, 2020  
- **Post-Flood:** August 1–10, 2020  

Key layers used:
- **SAR VV-band (Sentinel-1):** for flood detection via backscatter change  
- **NDWI (Sentinel-2):** for surface water extent  
- **NDVI (Sentinel-2):** for vegetation health

We calculated:
- `SAR_diff = PostSAR - PreSAR`  
- NDWI and NDVI using custom evalscripts  
- A **binary flood mask** (`NDWI > 0.3`)  
- A **vegetation damage mask** (`NDWI > 0.3` and `NDVI < 0.3`)

---

## Model Training

Using the computed features and heuristic masks:

- A **Random Forest** model was trained to classify flooded vs. non-flooded pixels  
- A second **Random Forest** was trained to classify high-damage vs. no-damage vegetation zones  
- We performed **hyperparameter tuning** via `GridSearchCV` with 3-fold cross-validation  
- We also trained a **baseline and tuned XGBoost** model for comparison  

---

## Results

| Metric       | Random Forest | XGBoost |
|--------------|----------------|----------|
| Precision (High Damage) | 0.49           | 0.51     |
| Recall (High Damage)    | 0.59           | 0.57     |
| F1-score (High Damage)  | 0.53           | **0.53**     |
| Overall Accuracy        | 52%            | **53%**     |

- **SAR backscatter difference** was the sole input feature for classification  
- NDWI and NDVI were only used for heuristic label generation  
- **Figure outputs** include SAR imagery, NDWI, NDVI maps, confusion matrices, and predicted damage maps

---

## Next Steps

We aim to enhance this pipeline with:

- **Crop-specific damage detection**, focusing on paddy rice  
- **Deep learning (CNNs)** to capture spatial flood patterns  
- **Risk scoring models** integrating:
  - Flood severity (SAR/NDWI)
  - Vegetation loss (NDVI drop)
  - Crop value
  - Socioeconomic vulnerability


