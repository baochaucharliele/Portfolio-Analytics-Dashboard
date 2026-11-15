# 📈 Real-Time Portfolio Analytics Dashboard

[!\[Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/drive/1Abq7pjEuJ51KplV9TfgeIaWd6SaiFnXZ?usp=sharing)

A Python dashboard that tracks a stock portfolio against live market data. It calculates cost basis, profit and loss, and allocation, compares performance with the S\&P 500 using standard risk metrics, and refreshes automatically during market hours.

Built in Google Colab with **pandas**, **Plotly** and **yfinance**. Runs on a sample portfolio.

!\[Live dashboard: % change and dollar gain vs. average cost](images/live\_bars.png)
!\[Money spent per stock with profit / loss ring](images/pnl\_ring.png)
!\[Performance vs. S\&P 500](images/performance.png)

## Features

* **Live data pipeline:** pulls 1-minute and daily prices from Yahoo Finance, retries any ticker that fails through a second endpoint, and keeps the last known price if both fail
* **Position analytics:** cost basis, market value, unrealized P/L and return for each holding
* **Interactive charts:** average cost vs. current price, dollar gain by position, and a nested allocation chart (money spent on the inside, P/L ring on the outside)
* **Performance vs. benchmark:** time-weighted cumulative return against SPY
* **Risk metrics:** annualized return and volatility, Sharpe ratio, max drawdown, beta and correlation
* **Money-weighted return (XIRR):** annualized return that accounts for the timing of each purchase
* **Market-aware live mode:** refreshes every 60 seconds while the market is open and slows to every 15 minutes when it is closed, with timestamps in local time

## How it works

```
Yahoo Finance (yfinance)
   │  daily history + 1-minute bars, per-ticker fallback
   ▼
Clean and align  ──►  fill missing prices, split-adjusted closes, map buy dates to trading days
   │
   ▼
Analytics  ──►  cost basis, P/L, daily portfolio value, time-weighted returns,
   │            Sharpe, drawdown, beta, XIRR
   ▼
Plotly visuals + formatted tables, refreshed on a timer in live mode
```

|Cell|Purpose|
|-|-|
|1|Holdings input, price history, current prices, holdings table|
|2|Snapshot chart: average cost vs. current price|
|3|Performance analytics and risk metrics vs. S\&P 500|
|4|Live mode with auto-refreshing charts and table|

## Run it

1. Click **Open in Colab** above.
2. Choose **Runtime > Run all**.
3. Cell 4 keeps updating until you click its stop button.

To track a different portfolio, edit the `holdings` table in cell 1:

```python
holdings = pd.DataFrame({
    "ticker":   \["VOO", "AAPL"],
    "shares":   \[2, 3],
    "buy\_date": \["2024-01-08", "2024-03-04"],
    "avg\_cost": \[None, None],   # None uses the closing price on the buy date
})
```

## Metric definitions

|Metric|Meaning|
|-|-|
|Time-weighted return|Growth with the effect of deposits removed, so it reflects stock selection only|
|XIRR|Annualized return that includes the timing and size of each purchase|
|Sharpe ratio|Return above the risk-free rate (4% assumed) per unit of volatility|
|Max drawdown|Largest drop from a peak to a later low|
|Beta|Sensitivity of portfolio returns to the S\&P 500 (1.0 moves with the market)|

## Limitations

* Yahoo Finance prices can lag the market by a few minutes.
* Returns are price-only and do not include dividends.
* Each position is treated as a single purchase on its buy date.
* Market holidays are not detected.
* Live mode runs only while the Colab session is open.

## Tech stack

Python · pandas · NumPy · Plotly · yfinance · SciPy · Google Colab

\---

*Built for learning and demonstration. Not financial advice.*

