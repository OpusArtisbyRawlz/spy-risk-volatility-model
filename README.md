# Project 2 — Volatility Regime Classification

## Objective

The goal of this project is to determine whether **future volatility regimes can be predicted using observable market features**.

Specifically, we test whether the relationship between **short-term realized volatility and longer-term volatility** contains predictive information about future volatility conditions.

Understanding volatility regimes is important because volatility strongly influences:

* position sizing
* leverage
* drawdown risk
* trading system performance

This project focuses on building a **probabilistic model of future volatility regime** rather than predicting exact volatility values.

---

## Target Definition

The target variable is based on the ratio between short-term and medium-term realized volatility.

[
VolRatio_t = \frac{RV5_{trail}}{RV20_{trail}}
]

Where:

* **RV5_trail** = trailing 5-day realized volatility
* **RV20_trail** = trailing 20-day realized volatility

The classification target is defined as:

```
target = 1 if VolRatio > threshold
target = 0 otherwise
```

This creates a binary classification problem where the model predicts whether the market is entering a **higher volatility regime** relative to its recent baseline.

---

## Data

* Asset: SPY
* Frequency: Daily
* Time range: 2000 – 2026
* Observations: ~6500

All features are constructed using **only past information** to avoid look-ahead bias.

---

## Modeling Approach

The modeling framework consists of two stages:

### Stage 1 — Regression

A regression model predicts the expected value of the volatility ratio.

```
RV5_trail / RV20_trail
```

This captures the continuous relationship between short- and medium-term volatility.

### Stage 2 — Classification

The predicted volatility ratio is passed into a logistic classifier which outputs:

```
P(high volatility regime)
```

This produces a **probability of entering a high volatility regime**.

---

## Evaluation Framework

Evaluation is performed using **walk-forward out-of-sample testing**.

For each window:

1. Train models on historical data
2. Predict on unseen future data
3. Store out-of-sample probabilities

This process simulates **real-time forecasting** and prevents look-ahead bias.

---

## Calibration Analysis

The predicted probabilities were evaluated using **probability binning**.

Observations were grouped into probability bins, and the realized frequency of high volatility regimes was compared against predicted probabilities.

Key observation:

* The model probabilities show **some alignment with realized frequencies**, but the signal strength is modest.

---

## Key Results

Summary statistics:

* Mean predicted probability: **0.154**
* Base prevalence of high volatility regime: **~0.163**
* Sample size: **6557 observations**
* Time coverage: **2000–2026**

The model shows **limited predictive power** for volatility regime classification.

While the model captures some information about volatility clustering, the edge is relatively weak.

---

## Interpretation

Volatility is known to exhibit **strong clustering behavior**, but predicting regime transitions remains difficult.

Possible explanations for weak signal:

* volatility regimes may depend on **exogenous macro shocks**
* limited feature set
* nonlinear regime dynamics

Despite limited predictive performance, the project provides a useful framework for:

* volatility feature engineering
* probabilistic forecasting
* calibration analysis
* walk-forward evaluation

---

## Practical Implications

Although the predictive edge is small, volatility regime estimates may still be useful for:

* risk management
* position sizing
* filtering directional signals

Future work may combine volatility regime predictions with **directional alpha models**.

---

## Next Steps

Future improvements may include:

* richer volatility features
* nonlinear models (tree ensembles)
* macro regime variables
* integration with directional models

The next project explores **directional alpha using multi-day forward returns**.
