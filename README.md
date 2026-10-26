Stock Return Prediction with Machine Learning: the S&P 500 Magnificent 7

Master's thesis (MSc in Business Analytics, Universidad Pontificia Comillas ICADE-ICAI, 2026).

Can past daily returns alone predict the 22-trading-day cumulative return of Apple, Amazon, Alphabet, Meta, Microsoft, NVIDIA and Tesla, and would those predictions beat a simple buy & hold strategy?

Notebook written in Spanish.

Data

Daily price data for S&P 500 constituents from FactSet (1,391,801 rows, 503 companies, Dec 2014 – Dec 2025), filtered to the Magnificent 7 (19,369 rows). Daily returns are adjusted for dividends.

The data is licensed and not included in this repository.

Approach
Target and features: cumulative return over the next 22 trading days, predicted from the previous 60 daily returns (lags 1–60).
Leakage-free split: chronological 80/20 split (train up to Sep 2023, test from Oct 2023). Training rows whose 22-day target overlaps the test period are excluded, and the scaler is fitted on train only (StandardScaler, chosen for its robustness to extreme returns).
Models: naive baseline (predicts 0), linear regression, LightGBM and LSTM, with randomised hyperparameter search and time-series cross-validation (LightGBM) / Keras Tuner (LSTM).
Rolling backtest: LightGBM retrained monthly on a 365-day window (Oct 2023 – Nov 2025).
Investment simulation: model-driven strategy vs. buy & hold, with a 0.1% transaction cost per trade.
Results

Test set (Oct 2023 onwards):

Model	RMSE	MAE	R²
Naive	0.1180	0.0856	−0.1065
Linear regression	0.1137	0.0822	−0.0281
LightGBM	0.1120	0.0810	0.0032
LSTM	0.1235	0.0959	−0.2123
LightGBM is the only model with a positive test R², but it explains a negligible share of the variance. Its MAE of 0.081 means an average error of about 8 percentage points on the 22-day return.
LSTM clearly overfits: validation loss worsens while training loss falls.
In the rolling backtest, LightGBM's R² drops to −0.025: no predictive power under realistic conditions.
The model predicts a positive return in every period, so the strategy stays fully invested and is equivalent to buy & hold. The portfolio grows from €70,000 to about €150,000, slightly below buy & hold. No model anticipated the March 2025 drawdown.
Conclusion

Historical returns alone do not provide a usable signal to beat a passive strategy on these stocks, a result consistent with market efficiency. The value of the project lies in the evaluation framework: a leakage-free split, a naive baseline, a rolling backtest and transaction costs.

Next steps: add fundamental, macroeconomic or sentiment features, and evaluate classification of return direction instead of regression.

Tools

Python · pandas · NumPy · scikit-learn · LightGBM · TensorFlow/Keras · Keras Tuner · Matplotlib · Seaborn
