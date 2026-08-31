Statistical Arbitrage Pairs Trading — Cointegration & Z-Score Backtest
## What is this?

This project builds and backtests a statistical arbitrage pairs-trading strategy in Python. It screens candidate pairs of economically similar stocks for cointegration, constructs a mean-reverting spread between the chosen pair, and trades that spread using a rolling z-score signal. The strategy is validated out-of-sample, accounting for transaction costs, and stress-tested with a parameter sensitivity analysis to check the result isn't a fluke of one specific parameter choice.
