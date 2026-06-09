# Superstore Profit Prediction — Machine Learning Business Problem Solution

**MSc Information Technology Management — Berlin School of Business and Innovation (BSBI)**

**Module:** Machine Learning and Visualization for Data (IITG7003)

**Assessment:** Practical Skills Assessment (Component 1)

**Author:** Juan Osvaldo Ovalle Perez · **Student ID:** Q1122568

---

## 1. Overview

This repository contains the complete, reproducible machine-learning project that accompanies the
written report submitted on Canvas (`report/Q1122568_JuanOvalle_Assignment.docx`). Because the Canvas
submission only accepts the report document, the dataset, source code, figures and results are provided
here so the marker can inspect and re-run everything.

## 2. Business problem

A national retailer (the "Superstore") is profitable overall but loses money on a large share of
individual transactions. The project frames this as a **supervised regression** problem: predict the
**profit (USD)** of an order line from attributes known at the point of sale, and identify the factors
that drive profitability so the business can act on them (especially its discounting policy).

## 3. Dataset

- **Source:** Sample Superstore dataset (US office-supplies & furniture retailer, 2014–2017).
- **Size:** 9,994 order lines × 21 columns (no missing values).
- **Target:** `Profit`. **Key predictors:** `Sales`, `Quantity`, `Discount`, `Ship Mode`, `Segment`,
  `Region`, `Category`, `Sub-Category`, plus engineered date/shipping features.
- File: `data/Sample - Superstore.csv`.

## 4. How to reproduce

```bash
# 1) (optional) create a virtual environment
python -m venv venv && source venv/bin/activate   # Windows: venv\Scripts\activate

# 2) install dependencies
pip install -r requirements.txt

# 3) run the full pipeline (cleaning → EDA → modelling → evaluation → exports)
python superstore_ml_pipeline.py
```

The script regenerates everything in `figures/` and `outputs/`. You can also run individual stages:

```bash
python superstore_ml_pipeline.py eda      # data prep + EDA + Tableau export only
python superstore_ml_pipeline.py model    # modelling + evaluation only
```

## 5. Repository structure

```
superstore_ml_pipeline.py     # single, fully-commented pipeline (all 5 brief tasks)
data/
  Sample - Superstore.csv      # raw dataset
figures/                       # 12 figures used in the report (fig01–fig12)
outputs/
  eda_summary.txt              # key EDA statistics
  model_metrics.csv            # full model-comparison table
  superstore_clean_for_tableau.csv   # cleaned + engineered data for the Tableau (Component 2) work
report/
  Q1122568_JuanOvalle_Assignment.docx  # the written technical report
requirements.txt
```

## 6. Methodology (CRISP-DM)

Data cleaning and **outlier handling** (winsorising `Sales` and `Profit` at the 1st/99th percentiles) →
feature engineering (shipping days, date parts) → preprocessing in a scikit-learn `Pipeline`
(standardisation of numerics, one-hot encoding of categoricals) → training and **hyper-parameter
tuning** of five regressors under **5-fold cross-validation** → evaluation on a held-out 20% test set.

## 7. Results

| Model | MAE (USD) | RMSE (USD) | Test R² | 5-fold CV R² |
|-------|----------:|-----------:|--------:|-------------:|
| **Random Forest** | **12.30** | **34.21** | **0.886** | **0.879 ± 0.010** |
| Gradient Boosting | 16.52 | 35.32 | 0.878 | 0.867 ± 0.012 |
| Linear Regression | 39.26 | 76.47 | 0.430 | 0.534 ± 0.049 |
| Ridge Regression | 39.25 | 76.48 | 0.430 | 0.534 ± 0.049 |
| Lasso Regression | 39.02 | 76.63 | 0.427 | 0.534 ± 0.049 |

**Best model: Random Forest** — explains ~89% of the variance in order-line profit on unseen data.

### Key findings
- **Discount is the decisive controllable lever:** mean profit turns negative beyond roughly a
  20–30% discount (from +$67 at 0% to −$311 at 50%); discount correlates −0.86 with profit margin.
- **Sales value and discount** together account for ~82% of the Random Forest's predictive importance.
- **Tables, Bookcases and Supplies** are structurally loss-making; **Copiers, Phones and Accessories**
  are the strongest profit generators.
- Ensemble trees vastly outperform linear models, showing the profit relationship is non-linear and
  interaction-driven.

## 8. Link to Component 2 (Tableau)

`outputs/superstore_clean_for_tableau.csv` is the cleaned, feature-engineered dataset used as the basis
for the companion **Project Output** (Tableau dashboard and data story), so both assessments share the
same data and analysis.

## 9. Note on tools

Analysis in Python 3 (pandas, scikit-learn, matplotlib). The written report was prepared following the
BSBI Essay Guide (Times New Roman 12, justified, 1.5 spacing, Harvard referencing).
