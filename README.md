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
- Historical and Parametric Expected Shortfall
- Rolling one-day VaR forecasts
- Kupiec unconditional-coverage test
- Christoffersen independence test
- Conditional-coverage test
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

Using a rolling 250-day window, both the 99% Historical Simulation and
Parametric Normal VaR models produce significantly more exceptions than
expected.

The Kupiec and Christoffersen tests reject correct conditional coverage for
both models. The Parametric Normal model performs worse and understates
empirical tail risk.

The worst twenty-day historical portfolio loss is approximately EUR 84,179,
driven primarily by the equity exposure.

## Repository structure

```text
market-risk-engine/
├── data/
│   ├── raw/
│   └── processed/
├── notebooks/
│   └── 01_core_var_engine.ipynb
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

- EWMA volatility
- Weighted Historical Simulation
- Monte Carlo VaR
- Component and marginal VaR
- Fixed-income sensitivities
- Option Greeks
- P&L attribution
- FRTB methodology
- Model-validation report