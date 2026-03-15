# Asset Allocation Strategy — Markowitz Portfolio Optimization

A Python project that constructs, optimizes, and back-tests equity portfolios
using Mean-Variance Optimization (MVO), comparing three optimized strategies
against an S&P 500 benchmark over a 10-year period (2015–2025).

---

## Overview

This project demonstrates a full quantitative portfolio management workflow:
data collection, statistical modelling, portfolio optimization, and
performance evaluation — built entirely in Python using real market data.

The notebook:
- Builds an equity universe of 9 U.S. stocks plus SPY.
- Estimates expected returns and covariance using daily data.
- Solves three Markowitz-style optimization problems (Max Sharpe, Min Vol, 20% Target).
- Back-tests each portfolio vs. SPY from 2015–2025.
- Produces tables and charts for inclusion in a report or slide deck.

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
| AMAT   | Semiconductors      | Chip equipment (“picks & shovels”) | 0.39   |
| NVDA   | AI Chips            | High-beta AI growth engine    | 1.75        |

(Betas vs. SPY are estimated over the 2015–2025 sample.) [file:218]

---

## Methodology

### Data Collection

- Download 10 years of daily adjusted close prices (2015–2025) via `yfinance`.
- Compute daily **log returns** for all assets and the SPY benchmark. [file:218]

### Portfolio Statistics

- Compute annualized expected returns from daily mean returns. [file:218]
- Compute the **9×9 covariance matrix** of daily returns and annualize it. [file:218]
- Implement a reusable `portfolio_stats()` helper to return:
  - Annualized return
  - Annualized volatility
  - Sharpe ratio (using a 5% risk-free rate) [file:218]
- Simulate **10,000 random portfolios** to approximate the efficient frontier. [file:218]

### Mean-Variance Optimization (MVO)

- Use `PyPortfolioOpt` with **Ledoit–Wolf shrinkage** covariance for robustness. [file:218]
- Solve three optimization problems:

  1. **Max Sharpe**
     - Maximizes risk-adjusted return (Sharpe ratio) subject to long-only, fully invested weights. [file:218]
  2. **Min Volatility**
     - Minimizes total portfolio variance under the same long-only constraint. [file:218]
  3. **20% Target Return**
     - Minimizes volatility subject to a 20% annual target return; falls back gracefully if target is infeasible. [file:218]

- Re-implement the efficient frontier from scratch with `scipy.optimize.minimize`
  (SLSQP) as a validation of the PyPortfolioOpt solutions. [file:218]

### Back-test & Performance Evaluation

- Apply static optimal weights to daily returns over 2015–2025. [file:218]
- Compute:

  - Annualized return
  - Annualized volatility
  - Sharpe ratio (5% risk-free)
  - Maximum drawdown
  - Total cumulative return
  - Portfolio beta vs. SPY [file:218]

- Compare all three optimized portfolios vs. SPY:

| Portfolio      | Ann. Return | Ann. Volatility | Sharpe | Max Drawdown | Beta |
|----------------|-------------|-----------------|--------|--------------|------|
| Max Sharpe     | 43.07%      | 31.28%          | 1.22   | -41.83%      | 1.35 |
| Min Volatility | 15.34%      | 21.96%          | 0.47   | -29.86%      | 1.05 |
| 20% Target     | 26.06%      | 23.76%          | 0.89   | -32.19%      | 1.17 |
| SPY Benchmark  | 13.81%      | 17.62%          | 0.50   | -33.72%      | 1.00 |

(Values shown above are rounded to two decimals for readability.) [file:218]

---

## Visualisation

The notebook generates several publication-ready figures: [file:218]

- **Efficient frontier cloud** of 10,000 random portfolios, colored by Sharpe ratio.
- **Optimal portfolio overlay** highlighting:
  - Max Sharpe (annotated off-chart if necessary)
  - Min Volatility
  - 20% Target Return [file:218]
- **Cumulative performance curves** (2015–2025) for all three portfolios vs. SPY.
- **Annual returns bar chart** by year to show path dependency. [file:218]
- **Portfolio weights** table and/or heatmap showing concentration risk
  (e.g., NVDA in the Max Sharpe solution). [file:218]

All figures are saved into a `figures/` directory for easy export to reports or slide decks. [file:218]

---

## Key Finding

The **20% Target Return portfolio** emerges as the most practical choice for a
risk-aware investor: [file:218]

- Sits cleanly on the efficient frontier. [file:218]
- Delivers **~89% more return than SPY** (26.1% vs 13.8% annualized). [file:218]
- Accepts **higher volatility and drawdown than SPY**, but with a much stronger
  risk‑adjusted profile (Sharpe ≈ 0.89 vs 0.50). [file:218]
- Maintains **beta modestly above 1.0** (~1.17), keeping market sensitivity
  reasonable. [file:218]
- Avoids the extreme NVDA concentration and higher tail risk of the Max Sharpe
  portfolio. [file:218]

---

## Tech Stack

| Library         | Purpose                                 |
|-----------------|-----------------------------------------|
| `yfinance`      | Market data download                    |
| `pandas`        | Data manipulation and time series       |
| `numpy`         | Numerical computation                   |
| `scipy`         | Custom MVO solver (SLSQP)               |
| `PyPortfolioOpt`| Efficient frontier & robust covariances |
| `matplotlib`    | Core visualizations                     |
| `seaborn`       | Heatmaps & styled charts                |

---

## Project Structure

```text
├── Asset_allocation.ipynb        # Main analysis and optimization workflow
├── Asset_allocation_overview.pdf # Slide/report-style overview
├── figures/                      # All generated charts (PNG)
└── csv/                          # Exported CSV data (returns, covariances, etc.)

--- 

Author
Jiraphat Samutsakorn
This project serves as a quantitative finance portfolio project, demonstrating
skills in data handling, portfolio theory, optimization, and Python-based
back-testing.