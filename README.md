# Booking.com Reviews Analysis

Machine learning project that predicts the rating (1.0 to 10.0) of an individual hotel review on Booking.com based on review metadata, text length, traveler profile, and other features. Three regression models are compared: Linear Regression (Ridge), Random Forest, and Gradient Boosting. The best model is then fine-tuned through `GridSearchCV`.

## Contents

```
booking-reviews-analysis/
├── .venv/                              # Local virtual environment (created on setup)
├── booking_reviews.csv                 # Input dataset (~26.7k reviews, ~50 MB)
├── booking_reviews_analysis.ipynb      # Main analysis notebook
├── requirements.txt                    # Python dependencies
├── .gitignore
└── README.md
```

## Prerequisites

- **Python 3.12.10** (recommended).

## Setup

Run these commands from the project folder (PowerShell on Windows):

```powershell
# 1. Create a virtual environment in the project folder
python -m venv .venv

# 2. Upgrade pip and install all dependencies
.\.venv\Scripts\python.exe -m pip install --upgrade pip
.\.venv\Scripts\python.exe -m pip install -r requirements.txt
```

On macOS / Linux:

```bash
python3 -m venv .venv
.venv/bin/python -m pip install --upgrade pip
.venv/bin/python -m pip install -r requirements.txt
```

## Run the notebook

Launch JupyterLab using the local virtual environment:

```powershell
# Windows (PowerShell)
.\.venv\Scripts\jupyter-lab.exe booking_reviews_analysis.ipynb
```

```bash
# macOS / Linux
.venv/bin/jupyter-lab booking_reviews_analysis.ipynb
```

JupyterLab will open in the default browser. The notebook is already executed, so all tables and charts are visible immediately. To rerun, use `Run > Run All Cells`.

## What the notebook does

1. **Data loading** and basic information about the columns.
2. **Quality audit** (missing values, duplicates, target distribution).
3. **Data cleaning**: removal of technical columns, dropping rows without target, removing duplicates, parsing dates, normalizing nationalities (top 15 + Other).
4. **Feature engineering**: parsing tags into structured fields (`trip_type`, `traveler_type`, `via_mobile`, `stay_nights`), text length, word count.
5. **Exploratory Data Analysis (EDA)** with at least 3 styled tables and 6+ charts including a Pearson correlation heatmap.
6. **ML preparation**: train and test split (80/20), preprocessing pipeline (`StandardScaler` for numeric, `OneHotEncoder` for categorical).
7. **Training of three regression models** with the same preprocessor:
   - Linear Regression (Ridge)
   - Random Forest Regressor (200 trees)
   - Gradient Boosting Regressor (200 trees, learning rate 0.1, subsample 0.8)
8. **Evaluation** through MAE, RMSE, and R^2; comparison through a leaderboard table, predicted vs actual scatter plots, residual histograms, feature importance bar charts, and a 3x3 classification matrix that buckets ratings into Low / Medium / High.
9. **Fine-tuning of the best model** (Gradient Boosting) via `GridSearchCV` with 3-fold cross-validation, comparing performance before and after.
10. **Summary** with concrete numbers and limitations.

## Notes on training time

The full notebook runs in approximately one minute on a modern laptop. The hyperparameter grids for Random Forest and Gradient Boosting are intentionally small (8 combinations each) to keep training time short.

## Target variable

The target is the **`rating` column** (the score of the individual review, range 1.0 to 10.0). The column `avg_rating` (overall hotel rating) is **not** the target. It is used as a feature, since it represents publicly visible context that exists at the moment of submitting a review.

## Cleaning up

To recreate the environment from scratch:

```powershell
Remove-Item -Recurse -Force .venv
python -m venv .venv
.\.venv\Scripts\python.exe -m pip install -r requirements.txt
```
