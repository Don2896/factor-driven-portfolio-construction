# Quantitative Investment Pipeline
### A Factor-Driven Portfolio Construction System

---

## Overview

This project builds a complete end-to-end quantitative investment pipeline using Python, applied to a real ten-asset portfolio spanning approximately 817 trading days (2020–2023). Every stage of the pipeline — from raw data to dynamic portfolio rebalancing — is implemented from first principles, with all model limitations documented explicitly alongside the outputs.

The portfolio is intentionally constructed with a **technology and AI growth tilt**, balanced by defensive and commodity assets to demonstrate genuine diversification benefits through the covariance matrix.

---

## Investment Universe

| Ticker | Asset | Role |
|--------|-------|------|
| NVDA | NVIDIA | AI infrastructure — primary growth driver |
| TSLA | Tesla | EV and autonomous driving exposure |
| PLTR | Palantir | Data analytics and AI software |
| GOOGL | Alphabet | Large-cap technology and AI research |
| MSFT | Microsoft | Mature technology with AI integration |
| JPM | JPMorgan Chase | Financial sector, interest rate sensitivity |
| JNJ | Johnson & Johnson | Healthcare defensive |
| XOM | ExxonMobil | Energy commodity exposure |
| SPY | S&P 500 ETF | Broad market benchmark and factor |
| GLD | Gold ETF | Safe haven, genuine diversifier |

---

## Pipeline Structure

The project is organised into six sequential blocks, each building directly on the previous.

```
Block 1 → Block 2 → Block 3 → Block 4 → Block 5 → Block 6
  Data    Factor   Portfolio   Monte     Value     Bayesian
  &       Model    Optimise    Carlo     at Risk   Dynamic
  Returns                     Sim                 Optimise
```

### Block 1 — Data Infrastructure and Return Analysis
- Pulled daily price data using `yfinance` for all ten assets
- Computed log returns and annualised mean returns and volatilities
- Built the annualised covariance and correlation matrices
- Ran comprehensive normality testing across all assets — skewness, kurtosis, QQ plots, and statistical tests
- **Key finding:** every asset rejected normality at p = 0.000. Fat tails are universal across the universe — a limitation that propagates through every subsequent block

### Block 2 — Factor Model Construction
- Built a three-factor regression model: market returns (SPY), value/growth spread (IWD minus IWF), oil prices (XOM)
- Used `np.linalg.lstsq` — matrix regression from first principles — to find factor loadings for each asset individually
- Produced a factor exposure table: alpha, market beta, value/growth beta, oil beta, and R-squared per asset
- **Key finding:** NVDA generated 21.46% annual alpha — genuine outperformance above factor predictions. PLTR generated -3.14% negative alpha despite its AI narrative. GLD showed R-squared of 0.027 — operating in an almost completely separate return universe from equities

### Block 3 — Portfolio Optimisation (Modern Portfolio Theory)
- Simulated 5,000 random portfolio weight combinations to visualise the opportunity set
- Found the maximum Sharpe ratio portfolio using constrained optimisation (`sco.minimize`)
- Found the minimum variance portfolio
- Derived the complete efficient frontier across 50 target return levels
- **Key finding:** Maximum Sharpe portfolio achieved 34.17% return, 24.73% volatility, Sharpe 1.382 — dominated by XOM (60%) and NVDA (18%). Minimum Variance portfolio achieved 5.70% return, 10.47% volatility — dominated by GLD (49%) and JNJ (30%). Equal weight Sharpe of 0.846 beaten by both optimised strategies

### Block 4 — Monte Carlo Simulation
- Implemented Geometric Brownian Motion simulation using portfolio-level parameters from Block 3
- Generated 10,000 independent price paths over 252 trading days with moment matching variance reduction
- Produced full distribution of possible future outcomes for both optimal portfolios
- **Key finding:** Maximum Sharpe portfolio showed 10.5% probability of loss despite 34.17% expected return. Minimum Variance portfolio showed 30.9% probability of loss — demonstrating that minimum variance does not mean minimum probability of loss

### Block 5 — Risk Measurement (Value at Risk)
- Computed VaR using three distinct methodologies: simulation-based, historical, and analytical
- Compared all three approaches across 90%, 95%, and 99% confidence levels
- Applied 2022 rate shock stress test to both optimal portfolios
- **Key finding:** Historical 99% VaR of 57.70% versus analytical VaR of 23.35% for the maximum Sharpe portfolio — a 34 percentage point gap quantifying the direct cost of the normality assumption. The 2022 stress test showed minimum variance losing more than maximum Sharpe, as XOM's energy exposure cushioned the maximum Sharpe portfolio during the rate shock

### Block 6 — Bayesian Dynamic Optimisation
- Replaced static historical mean returns with 60-day rolling window estimates
- Ran maximum Sharpe optimisation at each of 758 trading days using current market conditions
- Compared static versus dynamic portfolio weights and tracked regime shifts through time
- **Key finding:** Complete regime shift between static and dynamic portfolios — XOM allocation fell from 60.3% to 0% as the energy supercycle ended; GLD allocation rose from 0% to 61.4% as gold surged on shifting rate expectations. Dynamic mean Sharpe of 3.380 versus static 1.382 — though inflated by in-sample evaluation bias

---

## Key Results Summary

| Block | Key Metric | Value |
|-------|-----------|-------|
| Normality | p-value across all assets | 0.000 (universal rejection) |
| Factor Model | NVDA annual alpha | 21.46% |
| Factor Model | PLTR annual alpha | -3.14% |
| Factor Model | GLD R-squared | 0.027 |
| Optimisation | Max Sharpe ratio | 1.382 |
| Optimisation | Min Variance volatility | 10.47% |
| Simulation | Max Sharpe probability of loss | 10.5% |
| Simulation | Min Variance probability of loss | 30.9% |
| VaR | Normality assumption cost at 99% | 34 percentage points |
| Stress Test | Max Sharpe 2022 drawdown | -28.87% |
| Stress Test | Min Variance 2022 drawdown | -32.15% |
| Dynamic | XOM static → dynamic weight | 60.3% → 0.0% |
| Dynamic | GLD static → dynamic weight | 0.0% → 61.4% |

---

## Technical Stack

| Library | Purpose |
|---------|---------|
| `numpy` | Matrix operations, lstsq regression, simulation |
| `pandas` | Data handling, time series alignment, rolling windows |
| `scipy.optimize` | Constrained portfolio optimisation (SLSQP) |
| `scipy.stats` | Normality tests, VaR percentile computation |
| `statsmodels` | QQ plots for normality visualisation |
| `yfinance` | Real market data retrieval |
| `matplotlib` | All visualisations |

---

## Key Concepts Demonstrated

- **Matrix regression from first principles** — factor model built using `np.linalg.lstsq` with manually constructed basis function matrix
- **Constrained optimisation** — Sharpe ratio maximisation and variance minimisation with equality and inequality constraints
- **Monte Carlo simulation** — GBM with Itô correction and moment matching variance reduction
- **Value at Risk** — three methodologies compared with explicit limitation documentation
- **Bayesian dynamic estimation** — rolling window return estimation replacing static historical means
- **Data alignment** — covariance matrix indexing bug identified and fixed through systematic diagnostic approach

---

## Honest Limitations

This project explicitly documents where models break down:

- All assets reject normality — GBM and analytical VaR underestimate tail risk
- Static optimisation anchors to historical regimes — XOM's 60% weight reflects the 2021-2022 energy supercycle, not forward-looking conditions
- Dynamic Sharpe of 3.380 is inflated by in-sample evaluation — a proper out-of-sample test would apply each day's weights to the following day's unseen returns
- The 60-day rolling mean is a momentum signal — it cannot distinguish sustainable regime shifts from temporary surges, requiring human judgment alongside the quantitative output

---

## How to Run

```bash
# Clone the repository
git clone https://github.com/yourusername/quantitative-investment-pipeline

# Install dependencies
pip install -r requirements.txt

# Open the notebook
jupyter notebook data_and_return_analysis.ipynb
```

**Note:** the notebook pulls live data from Yahoo Finance on each run. Results may differ slightly from those documented here as the dataset extends beyond December 2023.

---

## Background

Built as part of a self-directed transition into investment analysis, working through *Python for Finance* by Yves Hilpisch. The project implements and extends the quantitative techniques from Chapters 11–13 — mathematical tools, stochastic processes, and statistics — applied to a real portfolio construction problem rather than textbook examples.

---

*Donald | 2024*
