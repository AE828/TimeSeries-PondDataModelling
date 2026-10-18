# Seasonal Decomposition and AR Modelling of Pond Water Levels

This  project applies classical time series decomposition and autoregressive modelling to monthly pond water level data from Hampshire, UK. All analysis was conducted in R.

## Objective

Decompose a monthly hydrological time series into its trend, seasonal, and stochastic components, then identify and fit the most parsimonious autoregressive model to the residual process.

## Data

- **Source**: Monthly pond water level measurements from Hampshire, UK
- **Frequency**: 12 observations per year (monthly)
- **Features**: Clear annual seasonality, a structural dip in the late 2000s, and strong short-term autocorrelation

## Methodology

- **Trend estimation**: Linear regression on the raw series; slope tested for significance (found negligible — near-flat trend)
- **Seasonal decomposition**: Additive model using 12 monthly dummy variables. 10 of 12 seasonal coefficients were statistically significant, confirming strong annual periodicity
- **Residual analysis**: After removing the fitted seasonal component, the de-seasonalised residual series was examined via ACF and PACF plots
- **AR(p) model selection**: Yule-Walker equations solved analytically for AR(1), AR(2), and AR(3). Each model's residuals were tested for white noise properties (zero mean via Z-test, constant variance, absence of serial correlation in residual ACF)
- **Model comparison**: AIC used to confirm optimal order. AR(3)'s third coefficient (~−0.03) was insignificant, indicating overfitting; AR(1) failed to remove serial dependence. AR(2) selected as the best-fitting model
- **Validation**: Final AR(2) re-fitted via `arima()` in R; estimated coefficients matched the Yule-Walker solutions, and the intercept was not significantly different from zero — consistent with a properly de-meaned residual series

## Key Findings

1. **Dominant seasonality**: The seasonal component accounts for the bulk of the variation, with peak water levels in winter (November–January) and troughs in summer (June–August)
2. **AR(2) stochastic structure**: The de-seasonalised residuals follow an AR(2) process — each month's deviation from the seasonal norm depends on the previous two months, capturing short-term hydrological memory
3. **No meaningful trend**: The linear trend slope was not statistically significant, suggesting stable long-run average water levels over the observation period
4. **Structural dip**: A notable deviation in the late 2000s was identified as an irregular shock rather than a regime change — the seasonal pattern and AR structure remain stable around it

## Final Model

```
X_t = S_t + 0.6643 · Z_{t−1} + 0.2649 · Z_{t−2} + N_t
```
Where:
- **S_t** is the estimated seasonal component (12 monthly coefficients)
- **Z_t** is the de-seasonalised residual at time t
- **N_t ~ N(0, σ² = 0.576)** is white noise

See Full Report (Including Code) : [TIME SERIES.pdf](https://github.com/AE828/Time-Series---Pond-Data-Modelling/blob/918e2f181fedbae4734c6a3c8470845e3e7e461f/TIME%20SERIES.pdf)
