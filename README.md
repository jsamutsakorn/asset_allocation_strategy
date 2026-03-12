# Asset Allocation Strategy — Markowitz Portfolio Optimization

A Python project that constructs, optimizes, and back-tests equity portfolios
using Mean-Variance Optimization (MVO), comparing three optimized strategies
against an S&P 500 benchmark over a 10-year period (2015–2025).

---

## Overview

This project demonstrates a full quantitative portfolio management workflow:
data collection, statistical modelling, portfolio optimization, and
performance evaluation — built entirely in Python using real market data.

---

## Universe

Nine U.S. equities spanning defensive and growth sectors:

| Ticker | Sector              | Role                          | Beta vs SPY |
|--------|---------------------|-------------------------------|-------------|
| JNJ    | Healthcare          | Defensive, stable earnings    | 1.29        |
| KO     | Consumer Staples    | Resilient cash flows          | 0.83        |
| EOG    | Energy              | Commodity / inflation hedge   | 1.61        |
| NEM    | Gold Mining         | Safe-haven diversifier        | 1.09        |
| ADM    | Agriculture         | Food/crop price exposure      | 0.54        |
| MSFT   | Large-Cap Tech      | Cloud & AI growth platform    | 0.60        |
| ADBE   | Software            | High-margin subscription SaaS | 1.23        |
| AMAT   | Semiconductors      | Chip equipment ("picks & shovels") | 0.39   |
| NVDA   | AI Chips            | High-beta AI growth engine    | 1.75        |

---

## Methodology

### Data Collection
- Downloaded 10 years of daily adjusted close prices (2015–2025) via `yfinance`
- Computed daily **log returns** for statistical modelling

### Portfolio Statistics
- Calculated annualized expected returns and a **9×9 covariance matrix**
- Implemented a custom `portfolio_stats()` function (return, volatility, Sharpe)
- Simulated **10,000 random portfolios** to approximate the efficient frontier

### Mean-Variance Optimization (MVO)
- Used `PyPortfolioOpt` with **Ledoit-Wolf shrinkage** covariance for robustness
- Solved three optimization problems:
  - **Max Sharpe** — maximises risk-adjusted return (5% risk-free rate)
  - **Min Volatility** — minimises total portfolio variance
  - **20% Target Return** — minimises risk subject to ≥20% annual return
- Also implemented MVO from scratch using `scipy.optimize`

### Back-test & Performance Evaluation
- Applied static weights to daily returns (2015–2025)
- Computed key metrics vs. SPY benchmark:

| Portfolio      | Ann. Return | Ann. Volatility | Sharpe | Max Drawdown | Beta |
|----------------|-------------|-----------------|--------|--------------|------|
| Max Sharpe     | 67.62%      | 48.17%          | 1.30   | -65.99%      | 1.74 |
| Min Volatility | 10.33%      | 14.89%          | 0.36   | -29.23%      | 0.63 |
| 20% Target     | 20.27%      | 16.77%          | 0.91   | -28.51%      | 0.79 |
| SPY Benchmark  | 13.81%      | 17.62%          | 0.50   | -33.72%      | 1.00 |

### Visualisation
- Efficient frontier with optimal portfolio overlays
- Cumulative return curves (2015–2025)
- Annual return bar charts by year
- Individual asset volatility vs. portfolio volatility (diversification effect)
- Portfolio weight allocation heatmaps

---

## Key Finding

The **20% Target Return portfolio** is the most practical choice for a
risk-aware investor:
- Sits on the efficient frontier
- Delivers **~47% more return than SPY** (20.3% vs 13.8%)
- Maintains **lower volatility and drawdown** than SPY
- Keeps **beta below 1.0** (0.79), reducing market sensitivity
- Avoids the extreme NVDA concentration of the Max Sharpe portfolio

---

## Tech Stack

| Library        | Purpose                              |
|----------------|--------------------------------------|
| `yfinance`     | Market data download                 |
| `pandas`       | Data manipulation                    |
| `numpy`        | Numerical computation                |
| `scipy`        | Custom MVO solver                    |
| `PyPortfolioOpt` | Efficient frontier & shrinkage     |
| `matplotlib`   | Visualisation                        |
| `seaborn`      | Heatmaps & styled charts             |

---

## Project Structure
```
├── Asset_allocation.ipynb # Full analysis notebook
├── Asset_allocation_overview # Report of the
├── figures # ALL figures generated in the workflow
└── csv # All csv files generated
```


---

## How to Run

```bash
pip install yfinance PyPortfolioOpt pandas numpy scipy matplotlib seaborn
jupyter notebook Asset_allocation.ipynb
```

## Author: Jiraphat Samutsakorn
This project was built as a quantitative finance portfolio project demonstrating skills in financial data analysis, portfolio theory, and Python-based modelling.