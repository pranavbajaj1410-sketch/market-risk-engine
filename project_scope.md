# Market Risk Engine

## Objective

Develop and validate a Python-based market-risk engine for a multi-asset
portfolio.

The initial version will calculate:

- Daily portfolio P&L
- Historical Simulation VaR
- Parametric VaR
- Expected Shortfall
- Rolling VaR forecasts
- VaR exceptions
- Kupiec backtesting statistics
- Historical stress losses

## Initial Portfolio

| Risk factor | Asset class | Exposure |
|---|---|---:|
| Equity index | Equity | EUR 250,000 |
| Government bond proxy | Interest rates | EUR 300,000 |
| EUR/USD | Foreign exchange | EUR 200,000 |
| Gold | Commodity | EUR 150,000 |

Total gross exposure: EUR 900,000.

## Risk Parameters

- One-day holding period
- 95% and 99% confidence levels
- 250-day primary estimation window
- 500-day comparison window
- Daily market data
- At least five years of historical observations

## Planned Outputs

1. Clean market-price and return data
2. Daily position and portfolio P&L
3. VaR and Expected Shortfall estimates
4. Rolling backtesting results
5. Model-performance statistics
6. Historical stress tests
7. Charts and risk commentary
8. Model-validation report

## Initial Limitations

- Positions are represented as linear market exposures.
- Derivative nonlinearities are initially excluded.
- Transaction and funding costs are excluded.
- Currency conversion is simplified.
- Historical market relationships are assumed to remain informative.