# Market Risk Engine

A Python-based multi-asset market-risk project implementing VaR, Expected
Shortfall, model backtesting and historical stress testing.

## Project objective

The project demonstrates the development and validation of market-risk
methodologies for a portfolio containing equity, interest-rate,
foreign-exchange and commodity exposures.

## Current functionality

- Market-data download and validation
- Daily position and portfolio P&L
- Historical Simulation VaR
- Parametric Normal VaR
- EWMA Parametric VaR
- Weighted Historical Simulation
- Monte Carlo Normal VaR
- Historical, Parametric and simulated Expected Shortfall
- Rolling one-day VaR forecasts
- Kupiec unconditional-coverage test
- Christoffersen independence test
- Conditional-coverage test
- Comparative model-performance ranking
- Historical stress testing
- Risk-factor stress contributions

## Initial portfolio

| Risk factor | Exposure |
|---|---:|
| Equity index | EUR 250,000 |
| Government bond proxy | EUR 300,000 |
| EUR/USD | EUR 200,000 |
| Gold | EUR 150,000 |

Total gross exposure: EUR 900,000.

## Main findings

Five one-day 99% VaR models were compared using 2,404 rolling out-of-sample
forecasts:

- Historical Simulation
- Parametric Normal
- EWMA Parametric Normal
- Weighted Historical Simulation
- Monte Carlo Normal

Weighted Historical Simulation produced the fewest exceptions, with 35
exceptions compared with approximately 24 expected. However, it still failed
the Kupiec, Christoffersen independence and conditional-coverage tests.

EWMA Parametric Normal was the only model for which exception independence was
not rejected at the 5% level. However, it produced 47 exceptions and failed
overall conditional coverage.

Parametric Normal and Monte Carlo Normal produced nearly identical risk
estimates and backtesting results because both use normal returns, rolling
covariance estimates and fixed linear portfolio exposures.

All five models failed conditional coverage, indicating that the current
methods do not fully capture fat tails, volatility-regime changes and stressed
market dependence.

The worst twenty-day historical portfolio loss was approximately EUR 84,179,
driven primarily by the equity exposure.

## Repository structure

```text
market-risk-engine/
├── data/
│   ├── raw/
│   └── processed/
├── notebooks/
│   ├── 01_core_var_engine.ipynb
│   └── 02_advanced_var_models.ipynb
├── outputs/
│   ├── charts/
│   └── tables/
├── reports/
├── src/
├── tests/
├── project_scope.md
├── requirements.txt
└── README.md

```

## Installation

Create and activate a Python virtual environment.

Install the required packages:

```bash
pip install -r requirements.txt
```

Then open and run:

```text
notebooks/01_core_var_engine.ipynb
```

## Important limitations

The current version uses fixed linear exposures and liquid market proxies.

It does not yet include:

- Direct bond pricing
- Yield-curve sensitivities
- Derivative nonlinearities
- Transaction and funding costs
- Full portfolio revaluation
- Separate currency translation for USD-denominated instruments

## Planned extensions

- Filtered Historical Simulation
- Student-t VaR
- GARCH volatility modelling
- Component and marginal VaR
- Fixed-income DV01 and key-rate sensitivities
- Option Greeks and nonlinear P&L
- P&L attribution
- FRTB methodology
- Model-validation report