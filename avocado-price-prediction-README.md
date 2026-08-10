# 🥑 Avocado Price Prediction

Predicting weekly average Hass avocado prices across US regions from sales volume, packaging mix, type, and seasonality.

**Suggested repo name:** `avocado-price-prediction`

## Overview

Avocado prices vary a lot by region, season, and whether the avocados are conventional or organic. This project uses three years of Hass Avocado Board retail-scan data to build a model that predicts the weekly average price and to quantify the effect of volume, type, region, and seasonality on price.

## Dataset

- **Source:** Hass Avocado Board weekly retail-scan data (popularized via [Kaggle](https://www.kaggle.com/datasets/neuromusic/avocado-prices), mirrored on GitHub)
- **Size:** 18,249 weekly records, 54 US regions, 2015–2018
- **Target:** `AveragePrice` (USD per avocado)
- **Features:** `Total Volume`, PLU volumes (4046/4225/4770), bag sizes, `type` (conventional/organic), `year`, `month`, `region`

## Repository Structure

```
avocado-price-prediction/
├── README.md
├── requirements.txt
└── notebooks/
    └── 07_Avocado_Price_Prediction.ipynb
```

## Getting Started

```bash
git clone https://github.com/<your-username>/avocado-price-prediction.git
cd avocado-price-prediction
pip install -r requirements.txt
jupyter notebook notebooks/07_Avocado_Price_Prediction.ipynb
```

**requirements.txt**
```
pandas
numpy
matplotlib
seaborn
scikit-learn
xgboost
jupyter
```

## Methodology

1. **EDA** — price distribution by type, price trend over time, most expensive regions, price vs. volume (log scale), correlation heatmap.
2. **Feature engineering** — extracted `month`/`day_of_year` from date; kept `type` and `region` as categorical predictors.
3. **Preprocessing** — `ColumnTransformer` (`StandardScaler` for numeric, `OneHotEncoder` for `type`/`region`).
4. **Model comparison** — Linear Regression, Ridge, Random Forest, Gradient Boosting, XGBoost via 3-fold CV (RMSE).
5. **Tuning** — `GridSearchCV` over Random Forest (n_estimators, max_depth, min_samples_leaf).
6. **Evaluation** — RMSE/MAE/R² on a held-out 20% test set, residual plots, feature importance.

## Results

| Metric | Value |
|---|---|
| Final model | **Random Forest** (150 trees, depth=24) |
| **Test RMSE** | **$0.124** |
| Test MAE | $0.087 |
| **Test R²** | **0.904** |

**Top features:** `type` (organic vs. conventional) and `region`, followed by total sales volume.

## Key Insights

- Organic avocados cost roughly 40–60% more than conventional ones, consistently across the whole period.
- Prices show clear seasonality, typically peaking in autumn and dipping in winter/early spring.
- Higher sales volume is associated with lower prices — classic supply/demand at work.

## Future Work

- Reframe explicitly as a time-series forecasting problem using lagged prices.
- Incorporate external data (Mexican harvest conditions/weather, since most US Hass avocados are imported from there).

## Data Source & License

Data originally sourced from the Hass Avocado Board, distributed via Kaggle for educational use.

## License

MIT
