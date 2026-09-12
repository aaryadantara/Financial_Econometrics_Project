# Ireland 10-Year Sovereign Bond Yield Forecasting

Group project (Group 2) for Financial Econometrics, MSc Finance, Dublin City University.

## Research question

Can macroeconomic variables — especially CPI — improve forecasts of Ireland's 10-year government bond yield beyond a univariate baseline, and how much of the yield's variance is driven by domestic factors versus global factors (US rates, risk sentiment)?

## Data

Monthly data, January 2003 – January 2024, from FRED and Yahoo Finance. Nine variables: Ireland 10Y yield (target), ECB policy rate, euro area CPI, euro area industrial production, VIX, US 10Y Treasury yield, EUR/USD, Euro Stoxx 50, and Brent crude.

## Literature review

A full literature review (`literature_review.docx`) grounds the study in seven papers across three themes:

- **Macro-finance foundations** — the Fisher effect (Mishkin, 1992) motivates CPI as the primary exogenous driver and justifies I(1) differencing; fiscal and global determinants of sovereign yields (Baldacci & Kumar, 2010) justify including US 10Y, VIX, and ECB rate; long-run vs. short-run dynamics in small open economies (Poghosyan, 2014) inform the VAR's Cholesky ordering
- **Time-series modelling of CPI/inflation** — ARIMAX vs. ARIMA on Malaysian CPI (Huei & Kamisan, 2025) warns that exogenous regressors can improve in-sample fit while hurting out-of-sample accuracy; CPI seasonality in Ukraine (Shinkarenko et al., 2021) supports the ACF/STL approach used here and suggests seasonality matters less for financial yields than for price indices; SARIMAX on Nigerian inflation (Are, Aako & Ojo, 2023) validates the SARIMAX framework and flags that not every exogenous regressor turns out significant
- **Applied bond yield forecasting** — ARIMA(1,1,1) on US and Singapore 10Y yields (Zhou, 2025) is the closest direct comparator, and is purely univariate; this project's key extension is adding the macro-financial exogenous block Zhou's study lacks

The review's three research gaps this project addresses: no existing study places CPI inside an ARIMAX/SARIMAX exogenous block specifically to forecast sovereign yields; small eurozone open economies like Ireland are underrepresented relative to US/Singapore/Nigeria/Ukraine; and no study compares seasonal encoding methods (Fourier terms vs. seasonal ARIMA vs. month dummies) in this context.

## Methodology

1. **Stationarity & seasonality** — ADF tests on all series; STL decomposition (seasonal strength = 0.254, moderate); ACF/PACF and Ljung-Box tests for autocorrelation
2. **ARIMAX(1,1,1)** — univariate ARIMA structure plus the macro exogenous block
3. **SARIMAX(1,1,1)(1,0,1,12)** — adds a seasonal component and Fourier terms to capture smooth annual patterns
4. **Out-of-sample evaluation** — 24-month holdout, chronological train/test split, benchmarked against a naive random-walk forecast (evaluating both in-sample AIC/BIC and out-of-sample RMSE/MAE, per the in-sample/OOS divergence flagged in the literature)
5. **VAR model** — 6-equation system with Granger causality tests, impulse response functions, and forecast error variance decomposition (FEVD)

## Key findings

- **Forecast accuracy**: ARIMAX cut out-of-sample RMSE by 56% versus the naive random-walk benchmark (1.0313 vs. 2.3395); SARIMAX came in close behind (RMSE 1.1123)
- **Model selection**: ARIMAX outperforms SARIMAX on BIC (−100.30 vs. −85.57), while SARIMAX wins on AIC (−138.47 vs. −134.53) — BIC is preferred here for its parsimony penalty, favouring ARIMAX as the better-generalising model
- **Granger causality**: of the five candidate predictors, only CPI significantly Granger-causes the bond yield (p = 0.0202); US 10Y, VIX, ECB rate, and industrial production do not
- **Variance decomposition**: at a 12-month horizon, 54.4% of yield variance is explained by the yield's own history and 36.8% by US 10Y — global rate dynamics dominate over domestic monetary and inflation variables, consistent with the small-open-economy literature (Poghosyan, 2014; Baldacci & Kumar, 2010) motivating the project

## Tools

Python (`pandas`, `numpy`, `statsmodels`, `fredapi`, `yfinance`, `scikit-learn`, `matplotlib`)

## Files

- `FE_Group2_Final.ipynb` — full analysis notebook (data pipeline, diagnostics, ARIMAX/SARIMAX/VAR models, results summary)
- `FE_PPT_Group2_Final.pdf` — presentation deck (theoretical framework, literature review, results)
- `literature_review.docx` — full literature review (7 papers, synthesis table, research gap analysis)

## Team

Aarya Dantara, Nitish, Mohit, Dhruv
