# Market Risk Engine
A Python-based multi-asset market-risk project implementing Value at Risk
(VaR), Expected Shortfall (ES), statistical model validation and historical
stress testing.

The project develops progressively more advanced risk models and compares their
performance using rolling one-day-ahead out-of-sample forecasts.

## Project objective
The purpose of this project is to demonstrate the development, implementation
and validation of market-risk methodologies for a portfolio containing equity,
interest-rate, foreign-exchange and commodity exposures.

The project focuses on three practical questions:

1\. How much could the portfolio lose over a one-day horizon?
2\. Which risk model provides the most reliable forecasts?
3\. Which portfolio exposures drive losses during normal and stressed markets?

## Initial portfolio
| Risk factor | Market proxy | Exposure |
|---|---|---:|
| Equity index | SPY | EUR 250,000 |
| Government bond proxy | TLT | EUR 300,000 |
| EUR/USD | EURUSD=X | EUR 200,000 |
| Gold | GLD | EUR 150,000 |

****Total gross exposure: EUR 900,000****

The portfolio currently uses fixed linear EUR exposures. Daily position P&L is
approximated by multiplying each risk-factor return by its corresponding
notional exposure.

## Current functionality
### Market data and portfolio P&L
- Market-data download and validation
- Cross-market date alignment
- Daily risk-factor returns
- Daily position-level P&L
- Daily aggregate portfolio P&L
- Data-quality and missing-value checks

### VaR and Expected Shortfall models
- Historical Simulation VaR
- Parametric Normal VaR
- EWMA Parametric Normal VaR
- Weighted Historical Simulation
- Monte Carlo Normal VaR
- Student-t Parametric VaR
- EWMA Student-t VaR
- Filtered Historical Simulation
- Historical, parametric and simulated Expected Shortfall

### Model validation
- Rolling one-day-ahead VaR forecasts
- Kupiec unconditional-coverage test
- Christoffersen independence test
- Christoffersen conditional-coverage test
- Exception-frequency analysis
- Exception-clustering diagnostics
- Common-period model comparison
- Comparative model-performance ranking

### Distribution and parameter diagnostics
- P&L skewness and excess kurtosis
- Jarque–Bera normality testing
- Student-t degrees-of-freedom estimation
- EWMA-standardized residual analysis
- Normal versus heavy-tailed model comparison
- EWMA decay-factor sensitivity analysis
- Lookback-window sensitivity analysis

### Stress testing
- Worst one-day historical loss
- Worst five-day cumulative loss
- Worst twenty-day cumulative loss
- Risk-factor stress contributions
- Identification of offsetting and amplifying exposures

### Fixed-income risk

- Fixed-rate bond pricing
- Macaulay and modified duration
- Convexity
- DV01
- Key-rate DV01
- Parallel yield-curve stress testing
- Non-parallel curve stress scenarios
- Duration versus full-revaluation comparison
- Historical Treasury curve scenarios
- Historical full-revaluation VaR and Expected Shortfall
- Parametric key-rate VaR

## Model development
### Day 1 — Core risk engine
The first notebook implements:

- Historical Simulation VaR and ES
- Parametric Normal VaR and ES
- Rolling 99% VaR backtests
- Kupiec and Christoffersen tests
- Historical stress testing
- Risk-factor stress contributions

### Day 2 — Advanced VaR models
The second notebook adds:

- EWMA Parametric Normal VaR
- Weighted Historical Simulation
- Monte Carlo Normal VaR
- Model-performance comparison
- Comparative backtesting across five models

### Day 3 — Fat-tail and filtered models
The third notebook adds:

- Portfolio distribution diagnostics
- Student-t Parametric VaR
- EWMA Student-t VaR
- Filtered Historical Simulation
- Decay-factor sensitivity analysis
- Lookback-window sensitivity analysis
- Common-period comparison across eight models

### Day 4 — Risk decomposition

A risk-decomposition notebook has been started to extend portfolio analysis
toward marginal VaR, component VaR, incremental VaR and diversification
analysis. This module remains in progress.

### Day 5 — Fixed-income risk

The fifth notebook introduces an instrument-level interest-rate risk module:

- Fixed-rate bond pricing
- Duration and convexity
- DV01 and key-rate DV01
- Parallel and non-parallel yield shocks
- Exact full revaluation
- Historical Treasury curve scenarios
- Fixed-income VaR and Expected Shortfall

## Main findings
### Core and advanced models
The initial five models were evaluated using 2,404 rolling one-day-ahead
forecasts:

- Historical Simulation
- Parametric Normal
- EWMA Parametric Normal
- Weighted Historical Simulation
- Monte Carlo Normal

Weighted Historical Simulation produced the fewest exceptions among these
models, with 35 exceptions compared with approximately 24 expected. However,
it still failed unconditional coverage, independence and combined conditional
coverage.

Parametric Normal and Monte Carlo Normal produced nearly identical risk
estimates and backtesting results because both models use:

- Normally distributed returns
- Rolling covariance estimates
- Fixed linear portfolio exposures

All five initial models failed conditional coverage. This indicated that they
did not adequately represent fat tails, volatility-regime changes and
stressed-period dependence.

### Evidence of non-normality
The portfolio P&L distribution is materially non-normal.

The full sample has:

- Negative skewness
- Substantial excess kurtosis
- A Jarque–Bera p-value close to zero

Normality is also rejected for the most recent 250-day period.

These findings help explain why the normal-distribution models produce too many
VaR exceptions.

### Student-t models
The Student-t Parametric model substantially improves exception frequency.

The fitted Student-t degrees of freedom vary through time, indicating that the
severity of tail risk is not constant across market regimes.

Both Student-t Parametric and EWMA Student-t pass unconditional coverage over
the final common evaluation period. However, their exceptions remain clustered,
so both models fail the independence and conditional-coverage tests.

The results suggest that heavy-tailed distributions improve the overall number
of VaR exceptions but do not fully capture the timing of stress events.

### Preferred Filtered Historical Simulation model
Sensitivity analysis was conducted using:

- Lookback windows of 250 and 500 days
- EWMA decay factors of 0.94, 0.97 and 0.99

The strongest specification was:

```text
Filtered Historical Simulation
Lookback window: 500 days
EWMA decay factor: 0.94
```

Over the common 2,154-day evaluation period, this model produced:

| Metric | Result |
|---|---:|
| Expected exceptions | 21.54 |
| Actual exceptions | 23 |
| Observed exception rate | 1.07% |
| Kupiec p-value | 0.7545 |
| Independence p-value | 0.0231 |
| Conditional-coverage p-value | 0.0722 |

The preferred model:

- Passes unconditional coverage at the 5% level
- Passes combined conditional coverage at the 5% level
- Fails the standalone independence test
- Ranks first in the common-period model comparison

It is therefore treated as the strongest candidate model rather than a
flawless or definitively validated model.

The longer window also increases the number of standardized residual scenarios
from 230 to 480. At a 99% confidence level, this increases the effective
Expected Shortfall tail sample from 2.3 to 4.8 scenarios.

### Common-period model ranking
The final eight models were compared over the same 2,154 forecast dates.

The overall diagnostic ranking was:

1\. Filtered Historical Simulation — 500 days, lambda 0.94
2\. EWMA Student-t
3\. Student-t Parametric
4\. Weighted Historical Simulation
5\. Historical Simulation
6\. EWMA Parametric Normal
7\. Monte Carlo Normal
8\. Parametric Normal

The comparison shows that models incorporating heavy tails and time-varying
volatility perform materially better than static normal-distribution models.

### Historical stress result
The worst twenty-day historical portfolio loss was approximately:

```text
EUR 84,179
```

The loss was driven primarily by the equity exposure. Gold also contributed
to the loss, while rates and foreign exchange partially offset the decline.

### Fixed-income risk findings

The fixed-income module replaces the original government-bond return proxy
with an instrument-level interest-rate risk framework based on illustrative
2-year, 5-year and 10-year fixed-rate bonds.

Interest-rate risk is concentrated in the long end of the curve. The 10-year
node contributes approximately 55% of total portfolio DV01, while the 5-year
and 2-year nodes contribute approximately 31% and 13%, respectively.

Under a +100 basis-point parallel increase in Treasury yields, exact full
revaluation produces a portfolio loss of approximately EUR 13,726, equivalent
to about 4.6% of fixed-income market value.

Duration alone becomes increasingly inaccurate as rate shocks grow. Across the
parallel-shock scenarios, adding convexity reduces the mean absolute pricing
error from approximately EUR 685 to approximately EUR 35.

Using 500 historical Treasury curve scenarios, the fixed-income portfolio has
a one-day 99% Historical Full-Revaluation VaR of approximately EUR 1,702 and
Expected Shortfall of approximately EUR 1,908.

The Parametric Key-Rate Normal model produces a lower 99% VaR of approximately
EUR 1,597 and Expected Shortfall of approximately EUR 1,829.

The worst historical rate scenario in the 500-day window occurred on
4 October 2024, when the 2-year, 5-year and 10-year Treasury yields increased
by approximately 23, 19 and 13 basis points, respectively. The resulting
portfolio loss was approximately EUR 2,294.

## Repository structure
```text
market-risk-engine/
├── data/
│   ├── raw/
│   └── processed/
├── notebooks/
│   ├── 01_core_var_engine.ipynb
│   ├── 02_advanced_var_models.ipynb
│   ├── 03_filtered_historical_and_t_var.ipynb
│   ├── 04_risk_decomposition.ipynb
│   └── 05_fixed_income_risk.ipynb
├── outputs/
│   ├── charts/
│   └── tables/
├── reports/
├── src/
├── tests/
├── .gitignore
├── project_scope.md
├── requirements.txt
└── README.md
```

## Installation
Clone the repository and move into the project directory:

```bash
git clone <repository-url>
cd market-risk-engine
```

Create a Python virtual environment:

```bash
python -m venv .venv
```

Activate the virtual environment.

On Windows:

```bash
.venv\Scripts\activate
```

On macOS or Linux:

```bash
source .venv/bin/activate
```

Install the required packages:

```bash
pip install -r requirements.txt
```

Open the project in VS Code or Jupyter and run the completed notebooks in order:

```text
notebooks/01_core_var_engine.ipynb
notebooks/02_advanced_var_models.ipynb
notebooks/03_filtered_historical_and_t_var.ipynb
notebooks/05_fixed_income_risk.ipynb
```

The first notebook downloads and processes the required market data. Later
notebooks use the processed data and saved outputs generated by the earlier
notebooks.

## Key output files
Generated results are stored in:

```text
outputs/tables/
```

Key tables include:

- Current VaR and Expected Shortfall estimates
- Rolling VaR forecasts
- VaR exception summaries
- Kupiec test results
- Christoffersen independence results
- Conditional-coverage results
- Student-t parameter diagnostics
- Filtered Historical Simulation sensitivity results
- Common-period model-validation comparison
- Bond pricing and risk metrics
- DV01 and key-rate DV01 contributions
- Parallel and non-parallel yield-curve stress results
- Historical fixed-income VaR and Expected Shortfall
- Parametric key-rate VaR comparison

Generated charts are stored in:

```text
outputs/charts/
```

Key charts include:

- Historical portfolio P&L distribution
- Rolling VaR backtests
- Monte Carlo simulated P&L distribution
- EWMA-standardized residual distribution
- Filtered Historical scenario distribution
- Model exception-rate comparison
- Conditional-coverage comparison
- Historical stress contributions
- Current Treasury yield curve
- DV01 and key-rate DV01 profiles
- Parallel yield-shock P&L comparison
- Yield-curve stress scenarios
- Historical Treasury yield changes
- Historical fixed-income VaR distribution

## Important limitations
The current project uses simplified fixed linear exposures and liquid market
proxies.

It does not yet include:

- Actual Treasury security-level cash-flow schedules and CUSIPs
- Multi-node cash-flow mapping for key-rate duration
- Credit-spread and liquidity-spread risk
- Derivative nonlinearities
- Option Greeks
- Full portfolio revaluation
- Transaction costs
- Funding and liquidity costs
- Separate currency translation for USD-denominated instruments
- Time-varying portfolio holdings
- Formal Expected Shortfall backtesting
- An untouched holdout period for final parameter validation

The preferred Filtered Historical Simulation parameters were selected using the
same broad historical period used for model comparison. Future work should test
the selected model on a separate holdout sample or use nested rolling model
selection.

The fixed-income module uses illustrative bonds rather than specific outstanding
Treasury securities. Each bond is mapped directly to a single Treasury maturity
node, credit and liquidity spreads are excluded, and the EUR reporting values
do not separately model USD/EUR translation.

## Planned extensions
- Component VaR
- Marginal VaR
- Incremental VaR
- Diversification-benefit analysis
- GARCH volatility modelling
- Option Greeks and nonlinear P&L
- Delta-normal and delta-gamma VaR
- Full-revaluation Monte Carlo
- P&L attribution
- Expected Shortfall backtesting
- FRTB methodology
- Model-validation report
- Automated testing and reusable Python modules

## Disclaimer
This project is intended for educational and portfolio-demonstration purposes.
The risk estimates should not be interpreted as production-ready trading or
capital-management measures.
