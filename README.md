# Urban Crime Analysis — SF Crime Classification Pipeline

> **Top 5% on the SF Crime Classification Kaggle Competition** | 2.2 log-loss | 39-class multiclass | 878K+ records

A scalable, end-to-end ML pipeline built on **Databricks** (PySpark + Delta Lake) to classify San Francisco crime incidents into 39 categories. The pipeline covers distributed data ingestion, feature engineering, model training, and Kaggle submission generation.

---

## Pipeline Overview

```
train.csv / test.csv
       │
       ▼
01_ingestion_delta       → Clean + store as Delta tables (Unity Catalog)
       │
       ▼
02_feature_engineering   → 47 features: time, spatial, cyclical, address, interactions
       │
       ▼
03_model_training        → XGBoost + LightGBM, automated model selection
       │
       ▼
04_inference_submission  → Batched inference → submission.csv
```

---

## Tech Stack

| Layer | Technology |
|-------|-----------|
| Compute | Databricks (PySpark, Serverless) |
| Storage | Delta Lake, Unity Catalog |
| ML | XGBoost, LightGBM, Scikit-learn |
| Experiment Tracking | MLflow (architecture) |
| Language | Python 3.11 |

---

## Dataset

[SF Crime Classification](https://www.kaggle.com/c/sf-crime) — Kaggle competition dataset.

| Split | Rows |
|-------|------|
| Train | 878,049 |
| Test  | 884,262 |
| Classes | 39 crime categories |

> `train.csv` and `test.csv` are not included in this repo (too large). Download from Kaggle and upload to your Databricks Volume before running.

---

## Feature Engineering (47 features)

| Group | Features |
|-------|----------|
| Time | Hour, Minute, Month, Year, DayOfMonth, DayOfYear, WeekOfYear, Quarter, Season |
| Binary flags | IsWeekend, IsNight, IsLateNight, IsMorning, IsAfternoon, IsEvening, IsRushHour, IsBusinessHours |
| Cyclical encoding | HourSin/Cos, MonthSin/Cos, DowSin/Cos, DaySin/Cos |
| Spatial | X, Y, DistCenter, CoordFreq, X/Y rounded, GridX, GridY |
| Address | IsBlock, IsIntersection, StreetNum, StreetNumBin |
| Categorical | District_enc, DayOfWeek_enc, DistrictRate |
| Interactions | District×Hour, District×DayOfWeek, Hour×DayOfWeek, District×IsNight, District×IsWeekend |

---

## Results

| Model | Accuracy | Log-Loss | ROC-AUC (weighted) |
|-------|----------|----------|--------------------|
| Logistic Regression | 29.5% | 1.987 | 0.669 |
| Random Forest | 38.2% | 1.782 | 0.759 |
| XGBoost | 37.4% | 1.778 | 0.762 |
| **LightGBM** | **37.6%** | **1.773** | **0.763** |

**Kaggle leaderboard: top 5% (2.2 log-loss, 2,331 teams)**

---

## How to Run (Databricks Free Edition)

### 1. Setup
- Sign up at [databricks.com](https://www.databricks.com/try-databricks)
- Go to **Catalog → workspace → default → Volumes → Create Volume** → name it `sf_crime_data`
- Upload `train.csv` and `test.csv` to the volume

### 2. Import Notebooks
**Workspace → Import** → upload each `.ipynb` from `SF_Crime_Databricks/` in order

### 3. Run in order
```
01_ingestion_delta.ipynb
02_feature_engineering.ipynb
03_model_training.ipynb
04_inference_submission.ipynb
```

Each notebook reads from Delta tables written by the previous one. Run top-to-bottom.

### 4. Download submission
**Catalog → workspace → default → Volumes → sf_crime_data → submission.csv → ⋮ → Download**

---

## Repository Structure

```
UrbanCrimeAnalysis/
├── SF_Crime_Databricks/
│   ├── 01_ingestion_delta.ipynb       # Data ingestion → Delta Lake
│   ├── 02_feature_engineering.ipynb   # PySpark feature engineering
│   ├── 03_model_training.ipynb        # XGBoost + LightGBM training
│   └── 04_inference_submission.ipynb  # Inference + submission.csv
├── Feature_Engineering_Models_Comparison.ipynb   # Original EDA & model comparison
├── Kaggle_Submission.ipynb                        # Original Kaggle submission
├── SF_Data_visualization.ipynb                    # Data visualization
└── README.md
```

---

## Resume Description

> Developed a scalable Databricks ML pipeline for distributed processing of 878K+ crime records using PySpark and Delta Lake. Engineered 47+ features across time, spatial, and categorical dimensions. Trained XGBoost and LightGBM classifiers with automated model selection. Achieved **top 5%** on the SF Crime Classification Kaggle leaderboard (2.2 log-loss, 39-class multiclass).
