---
title: "Crop Revenue Prediction and Recommendation for Uttar Pradesh (WIP)"
date: 2026-01-11
excerpt: "This project builds a district-level dataset for Uttar Pradesh and trains a regression model to predict revenue per hectare (yield × price), then ranks crops to suggest top options per district-season."
---

## Crop Revenue Predictor and Crop Ranking System (Work in Progress)

### What I’m doing
- Collecting and merging public datasets: crop area/production (to compute yield), seasonal weather (rainfall/temperature), and crop prices.
- Creating the target:
  - `yield_kg_per_hectare = production_kg / area_hectare`
  - `revenue_per_hectare = yield_kg_per_hectare × price_inr_per_kg`
- Feature engineering: seasonal aggregates (rainfall totals/means, temperature summaries) + simple lag/rolling features where possible.
- Training a **Random Forest regressor** with time-based splits (train on older years, test on newer years).
- Output: rank crops by predicted `revenue_per_ha` and return the **top 3** options per district-season.

### Tech Stack Used
   *Python, pandas, NumPy, scikit-learn (RandomForestRegressor), matplotlib*
