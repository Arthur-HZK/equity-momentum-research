# Equity Momentum Research

An end-to-end cross-sectional momentum study on US equities. The goal is not a high backtest return but a clean research process: a testable hypothesis, point-in-time signals with no look-ahead, realistic transaction costs, and an alpha/beta decomposition against the market.

## Question

Does a simple cross-sectional momentum signal produce alpha that is decorrelated from the market and survives transaction costs?

## Approach

Long past winners, short past losers, dollar-neutral, so market direction cancels out and only relative selection drives the P&L.

- Universe: 30 US large caps across sectors, 2010 to 2026 (about 15 years, yfinance)
- Signal: 12-1 momentum, point-in-time using `.shift` to prevent look-ahead
- Book: long top quintile, short bottom quintile, dollar-neutral
- Backtest: positions from day t applied to t+1 returns
- Costs: 10 bps per unit of turnover (conservative for large caps)
- Rebalancing: monthly, since a 12-month signal should not be traded daily
- Alpha: OLS of strategy returns on SPY returns

## Results

| Strategy | Net Sharpe | Net annual return |
|---|---|---|
| Momentum (daily rebal) | 0.01 | -1.97% |
| Momentum (monthly rebal) | 0.17 | +1.36% |
| Reversion 5d (monthly) | -0.15 | -4.53% |

Market regression on the monthly momentum strategy:

- Beta to market: 0.07 (decorrelated)
- Annualized alpha: 2.65%

![Equity curve](equity_curve.png)

## What I take from it

- Turnover, not the signal itself, was killing the edge. The same signal goes from -1.97% (daily) to +1.36% net (monthly) just by trading at the right frequency.
- The monthly strategy is genuine alpha rather than hidden market exposure (beta around 0.07).
- A naive combination with short-term reversion fails despite near-zero correlation (0.05). A short-horizon signal cannot share a monthly rebalance with a long-horizon one. Each signal has its own natural frequency.

## Limitations

- Small universe (30 names) limits cross-sectional dispersion.
- Dollar-neutral only, no explicit sector or beta neutralization.
- Fixed cost assumption. Real costs vary with liquidity and regime.
- Single factor. Institutional alpha comes from combining many weak, decorrelated signals.

## Stack

Python, pandas, numpy, yfinance, matplotlib.