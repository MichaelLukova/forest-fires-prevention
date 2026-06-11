Forest Fire Prevention and Prediction Analysis

A data science project that models wildfire burn area and classifies fire risk using the UCI Forest Fires Dataset — meteorological and fire-weather data from Montesinho Natural Park, Portugal.


Table of Contents
Project Overview
Dataset
Exploratory Data Analysis
Regression Modeling
Regularization & Multicollinearity
Classification Pipeline
Findings & Recommendations
Getting Started

Project Overview
Predicting wildfire burn area is inherently difficult due to localized microclimates, wind dynamics, and fuel availability all introducing noise. This project approaches the problem from two angles:

Regression: A progressive series of OLS models predicts log-transformed burned area, building from a baseline to quadratic, interaction, and log-transformed feature variants.
Classification: A logistic regression model maps meteorological conditions to a binary risk label (Large Fire vs. Small/No Fire), supporting faster resource allocation decisions.

Dataset
517 observations, 13 columns, no missing values. Target variable: area (hectares burned).

Exploratory Data Analysis
area is heavily right-skewed (median: 0.52 ha, max: 1090.84 ha). A log transformation log_area = log(area + 1) was applied to stabilize variance before modeling.

Key correlations with log_area:

DMC, wind, and DC show the strongest positive linear relationships.
RH shows a negative correlation (−0.054), consistent with the expectation that lower moisture increases fire risk.
temp carries a positive correlation of 0.054.

Regression Modeling
Four OLS regression models were built iteratively to balance parsimony, interpretability, and fit.

Model 4 achieves the highest variance explanation at R² = 9.93%.
Residuals approximate normality but show mild heteroscedasticity.
Cook's Distance flagged 29 influential observations above the threshold of 4/N = 0.0077.
Note: Low overall R² values reflect the inherent unpredictability of wildfire spread — a known challenge in this domain and consistent with published results on this dataset.

Regularization & Multicollinearity
FWI components are structurally correlated, introducing multicollinearity. VIF analysis confirmed:

Regularized models were trained on Model 4's feature set using an 80/20 train-test split:

Classification Pipeline
Fire risk was framed as a binary label: Large Fire (area > 0.52 ha) vs. Small/No Fire, using the median as the threshold.

Findings & Recommendations
Model Selection Guide
Use Case
Recommended Model
Rationale
Burn area estimation - Lasso Regression - Parsimonious; auto-selects relevant features; good generalization
Risk classification - Logistic Regression - Fast probability scoring for field deployment
Correlated feature sets - Ridge Regression - Stabilizes coefficients under multicollinearity

Key Operational Insight
temp and RH are the most reliable early warning signals. Forestry management should escalate to high-alert status when:

Temperature exceeds 25°C, AND
Relative humidity drops below 30%
This combination is most dangerous during the dry summer months (July–August) when deep soil dryness (DC) is at its seasonal peak.
