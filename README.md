# What Drives Used Car Prices?

This repository contains my CRISP-DM practical application on used car pricing.

The business question is: **what actually drives price, and how can a dealership use that insight to price inventory better?**

I used a Kaggle used-car dataset (about 426K listings), cleaned it, built regularized regression models, and translated the output into practical actions for a dealer team.

## Project Goal
Build a pricing support workflow that helps a used car dealership:
- avoid overpricing
- avoid underpricing
- make pricing decisions more consistent across inventory

## CRISP-DM Flow Used
- Business Understanding
- Data Understanding
- Data Preparation
- Modeling
- Evaluation
- Deployment

All work is documented in the python notebook `prompt_II.ipynb`.

## Repository Content
```
.
├── README.md
├── data
│   └── vehicles.csv
├── images
│   ├── crisp.png
│   └── kurt.jpeg
└── prompt_II.ipynb
```

##  Key findings

### Data preparation outcome
- Raw data: **426,880 rows, 18 columns**
- `price <= 0`: **32,895 rows (7.706%)**
- High missingness in some fields (`size`, `cylinders`, `condition`, `VIN`, `drive`, `paint_color`)
- Final modeling data: **333,851 rows**
- Removed during cleaning: **93,029 rows (21.793%)**
- Split sizes:
  - Train: **233,695**
  - Validation: **50,078**
  - Test: **50,078**

### Modeling summary
3-fold CV:
- `Ridge_Poly2`: MAE **0.3904**, RMSE **0.8320**
- `Ridge`: MAE **0.3982**, RMSE **0.8385**
- `Lasso`: MAE **0.4599**, RMSE **0.9023**

Tuning (`GridSearchCV`):
- Ridge best alpha: **0.1**
- Lasso best alpha: **0.0005**

### Final approved model
**Ridge_Poly2** (selected after comparison)
- Validation: MAE **0.3810**, RMSE **0.8255**
- Test: MAE **0.3846**, RMSE **0.8214**

## Dealer summary points
- The model is good enough to use as a **pricing support tool**.
- Recommended workflow:
  1. generate predicted price
  2. compare to listed price
  3. bucket each unit as `Likely Overpriced`, `Near Market`, or `Likely Underpriced`
  4. require review for large gaps

Current scoring snapshot:
- Near Market: **143,466**
- Likely Underpriced: **124,881**
- Likely Overpriced: **65,504**

## Monitoring Plan
- Track MAE/RMSE monthly
- Track input drift (year, odometer, brand mix)
- Retrain if MAE degrades by >10% for 2 straight months
- Retrain quarterly at minimum

## How to Run
1. Clone the repository
2. Create/activate a virtual environment
3. Install dependencies (`pandas`, `numpy`, `scikit-learn`, `seaborn`, `matplotlib`, `jupyter`)
4. Open `prompt_II.ipynb` and run all cells

## Notes
- This project uses listing data, not sold-price outcomes.
- Next improvement would be adding transaction-level signals (sold price, days to sell, margin).

## Author
Edgar Mercado
Berkeley ML/AI - Practical Application II (CRISP-DM) 