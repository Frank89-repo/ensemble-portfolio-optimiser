# Institutional Ensemble Portfolio Optimiser (v4.3)

A quantitative portfolio construction and walk-forward backtesting framework in Python designed to mitigate estimation error and tail risk in multi-asset equity allocation.

## Methodology Overview
* **Data Pipeline:** Automated retrieval of daily price history (~5 years / 1,242 trading days across 21 US & European equities), FX-adjusted market capitalizations, live 13-week T-bill risk-free rate (`^IRX`), and fundamental valuation targets.
* **Sub-Models Evaluated:**
  1. **BL-MVO:** Black-Litterman Mean-Variance Optimization.
  2. **Bootstrap:** Resampled Mean-Variance Optimization to reduce sensitivity to point-estimate inputs.
  3. **HRP:** Hierarchical Risk Parity (clustering-based allocation without covariance matrix inversion).
  4. **Min-CVaR:** Minimum Conditional Value-at-Risk (95% tail-risk minimization).
* **Expanding-Window Walk-Forward Calibration:** Uses a 40% burn-in period followed by 5 expanding out-of-sample (OOS) drifting buy-and-hold folds net of transaction costs (bps) to prevent look-ahead bias.
* **Bayesian Meta-Weighting:** Combines prior model weights (80%) with walk-forward OOS Sharpe tilts (20%) damped by Lo (2002) Sharpe Ratio Standard Errors.

## Out-of-Sample Walk-Forward Results (5 Folds, Net of Transaction Costs)

| Model | Ann. Return (Net) | Ann. Volatility | Max Drawdown | CVaR-95 (Ann.) | OOS Sharpe | Calmar |
| :--- | :---: | :---: | :---: | :---: | :---: | :---: |
| **BL-MVO** | 51.00% | 24.48% | -24.13% | -54.03% | 1.921 | 2.114 |
| **Bootstrap** | 47.27% | 16.19% | -12.67% | -34.09% | 2.674 | 3.731 |
| **HRP** | 36.96% | 13.39% | -11.45% | -28.98% | 2.463 | 3.228 |
| **Min-CVaR** | 39.43% | 14.76% | -10.56% | -31.56% | 2.402 | 3.735 |
| **Ensemble** | **44.92%** | **16.22%** | **-12.92%** | **-34.90%** | **2.524** | **3.477** |
| **1/N Equal** | 49.91% | 17.75% | -13.24% | -37.82% | 2.588 | 3.770 |
| **S&P 500 (USD)** | 24.52% | 14.77% | -18.69% | -32.89% | 1.391 | 1.312 |
