Statistical Arbitrage Pairs Trading — Cointegration & Z-Score Backtest
## What is this?

This project builds and backtests a statistical arbitrage pairs-trading strategy in Python. It screens candidate pairs of economically similar stocks for cointegration, constructs a mean-reverting spread between the chosen pair, and trades that spread using a rolling z-score signal. The strategy is validated out-of-sample, accounting for transaction costs, and stress-tested with a parameter sensitivity analysis to check the result isn't a fluke of one specific parameter choice.

## How it works
- Screen candidate pairs — five pairs of companies in similar sectors (e.g. Coca-Cola/Pepsi, Visa/Mastercard, ExxonMobil/Chevron) are tested for cointegration using the Engle-Granger test. Correlation alone isn't enough — cointegration specifically means a combination of the two prices behaves as stationary, giving a genuine, stable relationship to trade around.
- Split the data — the price history is split into an in-sample period (used to find the relationship) and an out-of-sample period (used only to evaluate performance, never to recalibrate). This discipline is what separates a genuine result from one that's simply curve-fitted to the past.
- Estimate the hedge ratio — using in-sample data only, a regression finds the ratio in which to combine the two stocks so their difference (the "spread") is as stationary as possible.
- Build the z-score signal — the spread's distance from its own rolling 60-day average, measured in standard deviations, tells us how unusually wide or narrow it currently is.
- Generate trades — the strategy enters a position when the z-score crosses ±2 (betting the spread reverts), and exits once it moves back within ±0.5 of zero, avoiding excessive trading right at the mean.
- Avoid look-ahead bias — every trade executes one day after the signal that generated it, since in reality you can't act on a closing price before it's actually known.
- Account for transaction costs — a cost is applied every time the position changes, so the result reflects something closer to real trading conditions rather than a frictionless ideal.

## The results
Out of five candidate pairs tested, Visa and Mastercard showed by far the strongest cointegration evidence (Engle-Granger p-value = 0.0068), consistent with them being close competitors in the same business (payment networks) with similar exposure to consumer spending trends. The estimated hedge ratio was 0.52, and the resulting spread passed an Augmented Dickey-Fuller stationarity test with high confidence (p = 0.0013).
On out-of-sample data the strategy achieved an annualised Sharpe ratio of 1.17, a max drawdown of roughly 10 (in spread units), and executed 27 trades over roughly two years — a sensible trading frequency for the chosen parameters, not excessively active.
! [graph](backtesting_graph.png)
