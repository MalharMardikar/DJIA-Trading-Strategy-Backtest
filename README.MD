# DJIA Trading Strategy Backtest

A Python-based backtesting framework comparing active vs. passive trading
strategies on DJIA constituent stocks and the DIA ETF, using 52-week
High/Low signals, volume filters, and Fama-French factor analysis.

---

## Key Results

| Strategy       | Portfolio Growth (26 yrs) | Sharpe Ratio | Max Drawdown |
|----------------|--------------------------|--------------|--------------|
| DIA Passive    | 10x                      | 0.0315       | -52.33%      |
| Stock Active   | 5x                       | 0.0184       | -62.78%      |
| DIA Active     | Net Loss                 | -0.0145      | -59.34%      |

Adding momentum + volatility filters improved Stock Active win rate
from **49.10% → 62.64%** (+13.54%).

---

## Architecture

Kenneth French Data Library → Python (pandas, numpy) →
Strategy Backtesting → Performance Analysis

---

## Tech Stack

| Layer                | Tool                                      |
|----------------------|-------------------------------------------|
| Data Source          | Kenneth French Data Library + yfinance    |
| Data Processing      | Python (pandas, numpy)                    |
| Statistical Analysis | statsmodels (OLS, Fama-French 3-Factor)   |
| Visualization        | matplotlib                                |
| Environment          | Jupyter Notebook                          |

---

## Strategies Compared

1. **DIA Passive** — Buy and hold DIA ETF for 26 years
2. **DIA Active** — Trade DIA ETF using 52-week High/Low + volume signals
3. **Stock Active** — Trade DJIA constituent stocks using same signals
4. **Enhanced Active** — Stock Active with added momentum & volatility filters

## Trading Signal Logic

- **Long entry:** Price hits 52-week high AND volume > 1.5x 20-day average
- **Short entry:** Price hits 52-week low AND volume > 1.5x 20-day average
- **Exit:** After 20-day holding period OR 5% stop-loss triggered

---

## Data Sources

**Fama/French 3 Factors (Monthly)**
Download from [Kenneth French's Data Library](https://mba.tuck.dartmouth.edu/pages/faculty/ken.french/data_library.html)
Save as `data/data_ff3.csv`

**DJIA Constituent Stocks + DIA ETF (Daily, 1998–2024)**
This dataset is not publicly hosted. You can recreate it using the
yfinance script below — note that results may differ slightly from
the analysis in this notebook due to constituent changes over time.

```python
import yfinance as yf
import pandas as pd

tickers = [
    'DIA', 'AAPL', 'AMGN', 'AXP', 'BA', 'CAT', 'CRM', 'CSCO', 'CVX',
    'DIS', 'GS', 'HD', 'HON', 'IBM', 'INTC', 'JNJ', 'JPM', 'KO',
    'MCD', 'MMM', 'MRK', 'MSFT', 'NKE', 'PG', 'TRV', 'UNH', 'V',
    'VZ', 'WBA', 'WMT'
]

data = yf.download(tickers, start="1998-01-01", end="2024-01-01", auto_adjust=False)
data = data.stack(level=1).reset_index()
data.columns.name = None
data = data.rename(columns={'level_1': 'Ticker'})
data.to_csv('data/data_djia.csv', index=False)
```

---

## Setup

1. Clone the repository
2. Download both data files following the instructions above and place them in the data/ folder
3. Install dependencies:
   - pip install -r requirements.txt
4. Run djia_trading_strategy.ipynb in Jupyter Notebook

Author
Malhar Mardikar
https://www.linkedin.com/in/malhar-mardikar-768892287/

