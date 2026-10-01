# Real Estate Valuation: Regression Modeling & Diagnostics

**Statistical modeling case study in R focused on model building, validation, and regression diagnostics**

## Executive Summary

This project examines whether real-estate price per square foot can be modeled from property age, transit accessibility, nearby convenience stores, and geographic variables.

The value of the project is not simply fitting a regression equation. The analysis demonstrates a broader modeling workflow:

**exploration → polynomial modeling → train/test split → model reduction → RMSE/AIC/ANOVA comparison → assumption diagnostics → multicollinearity analysis → outlier analysis → all-subsets regression → held-out evaluation**

That makes the project useful evidence of statistical reasoning rather than just familiarity with `lm()`.

## Business Question

Which measurable property and location characteristics help explain `PricePerSQFT`, and how can a regression model be simplified without materially sacrificing fit?

A secondary question is equally important:

**Can the model's statistical assumptions and limitations be identified before its predictions are used for decision-making?**

## Analytical Approach

### 1. Train/test design

The source coursework used a reproducible 70/30 split so model development occurred on training observations and final predictive evaluation could be performed on held-out data.

### 2. Exploratory analysis

Predictors were plotted against `PricePerSQFT`. `HouseAge` showed evidence of curvature, motivating a quadratic term (`HouseAge²`).

### 3. Correlation screening

The original analysis reported notable relationships among predictors, including approximately:

- `ToTransitStation` ↔ `Longitude`: **r = -0.79**
- `ToTransitStation` ↔ `NumConvenienceStores`: **r = -0.59**

Correlation was treated as a diagnostic signal rather than an automatic variable-removal rule.

### 4. Full regression model

The original full specification was:

```text
PricePerSQFT ~ HouseAge + HouseAge² + ToTransitStation
             + NumConvenienceStores + Latitude + Longitude
```

### 5. Model reduction

A reduced model removed `Longitude`.

The saved coursework reported:

| Model | Adjusted R² | Training RMSE | Nested-model ANOVA | AIC |
|---|---:|---:|---:|---:|
| Full | 0.5863 | 76.91 | — | ~3371.7 |
| Reduced (-Longitude) | 0.5877 | 75.99 | p = 0.8104 | ~3371.7 |

The non-significant nested-model comparison suggested that removing `Longitude` did not produce evidence of a meaningful loss of fit in that comparison.

**Important:** these are coursework-reported results. The original Excel dataset is not distributed in this repository, so the metrics are not presented as independently rerun portfolio results.

## Model Diagnostics

The analysis tests several assumptions and risks:

- Residual normality / Q-Q behavior
- Independence of errors with Durbin-Watson and ACF
- Linearity using residual and component-plus-residual plots
- Homoscedasticity using `ncvTest`
- Multicollinearity using VIF
- Outliers using Bonferroni-adjusted outlier testing

The original coursework reported evidence of **heteroscedasticity**, high VIF values for `HouseAge` and `HouseAge²`, and a flagged outlier.

Recognizing those issues is important: a model can have reasonable fit statistics and still have diagnostic limitations.

## A More Careful Model-Selection Principle

The original coursework statement that the model with the lowest RMSE is "always" the best model was too absolute.

A better analytical principle is:

> Model selection should consider held-out predictive performance together with model complexity, assumptions, interpretability, information criteria, influential observations, and the business objective.

The updated analysis preserves the original work while making this distinction explicit.

## Polynomial Terms and VIF

A predictor and its squared term can be strongly correlated. The updated notebook therefore demonstrates **mean-centering `HouseAge` before squaring it** as a methodological extension that can reduce nonessential multicollinearity.

This extension is clearly separated from the original exam analysis.

## Skills Demonstrated

- R
- Multiple linear regression
- Polynomial regression
- 70/30 train-test validation
- RMSE
- AIC
- Nested-model ANOVA
- Backward model reduction
- Correlation analysis
- Residual diagnostics
- Durbin-Watson testing
- Heteroscedasticity testing
- VIF / multicollinearity analysis
- Outlier detection
- All-subsets regression
- Model interpretation
- Statistical communication

## Repository Structure

```text
real-estate-price-prediction/
├── README.md
└── analysis/
    └── real_estate_regression_diagnostics.Rmd
```

## Data & Reproducibility Note

The original analysis referenced `RealEstateValue.xlsx`, but that dataset was not included in the uploaded repository. It is therefore **not appropriate to claim that this public repository is currently fully reproducible**.

The notebook has been cleaned so it expects the dataset at:

```text
data/RealEstateValue.xlsx
```

If a distributable copy of the source dataset is later located, it can be added and the numerical outputs can be rerun and preserved.

## Interview Talking Point

> I used multiple regression to model price per square foot and added a quadratic house-age term after exploratory plots suggested curvature. I compared a full and reduced model using RMSE, AIC and a nested-model ANOVA, then tested the selected model for normality, independence, linearity, heteroscedasticity, multicollinearity and outliers. One of the biggest lessons was that selecting a model isn't just about the lowest RMSE. The diagnostics showed issues such as heteroscedasticity and high VIF around the polynomial terms, so I would address those before treating the model as production-ready.

## Why This Project Matters

This project demonstrates the ability to move beyond generating a prediction and ask whether the model is **statistically defensible, generalizable, interpretable, and appropriate for the business decision**.
