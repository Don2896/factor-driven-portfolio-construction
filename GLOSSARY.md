# Project Glossary
## Quantitative Investment Pipeline — Key Terms

All terms are defined in the context of this project. Where relevant, connections to specific blocks are noted.

---

## A

**Alpha (α)**
The portion of an asset's return that cannot be explained by its factor exposures. A positive alpha means the asset consistently outperforms what its systematic factor loadings predict — genuine stock-specific value generation. A negative alpha means consistent underperformance. In this project NVDA generated 21.46% annual alpha; PLTR generated -3.14%. Alpha is the intercept term in the factor model regression. *Block 2*

**Analytical VaR**
Value at Risk computed using the normal distribution formula directly: VaR = -(μT + zσ√T), where z is the confidence level z-score. Fast to compute but assumes normally distributed returns — systematically underestimates tail risk when fat tails are present. *Block 5*

**Annualisation**
Converting daily statistics to annual equivalents. Returns scale linearly — multiply by 252 trading days. Volatility scales with the square root of time — multiply by √252. The two scale differently because of the mathematics of compounding. *Blocks 1, 3*

---

## B

**Basis Function**
A fundamental shape or ingredient used in regression. In Chapter 11 these were mathematical functions (1, x, x², sin(x)). In the factor model they are economic forces (market returns, value/growth spread, oil returns). The matrix regression finds optimal coefficients for each basis function. *Blocks 2*

**Bayesian Updating**
A statistical framework where prior beliefs are updated as new evidence arrives, producing a posterior distribution. In this project implemented as a 60-day rolling window that continuously updates return estimates — recent data replaces stale historical averages as the basis for portfolio allocation decisions. *Block 6*

**Beta (β)**
A factor loading — measures how sensitive an asset's returns are to a specific factor. Market beta of 1.8 means the asset moves 1.8 times the market in either direction. Beta is directional — positive market beta means the asset follows the market; negative would mean it moves opposite. *Block 2*

**Bounds**
Hard limits on individual variables in an optimisation problem. In portfolio optimisation, bounds of (0, 1) per asset enforce the no-short-selling constraint — no weight can be negative and none can exceed 100%. *Block 3*

---

## C

**Capital Market Line (CML)**
The straight line from the risk-free rate tangent to the efficient frontier. Represents the optimal combination of a risk-free asset and the risky portfolio at the tangency point. Every investor should hold the tangency portfolio as their risky allocation, mixing it with cash according to their risk tolerance. *Block 3*

**Coefficient**
A multiplier found by regression that scales a basis function or factor. In the factor model, four coefficients are found per asset: alpha, market beta, value/growth beta, oil beta. These are fixed across the full dataset — they describe the asset's average sensitivity over the entire estimation period. *Blocks 2*

**Confidence Level**
The probability threshold used in VaR. A 95% confidence level means 95% of scenarios produce losses no worse than the stated VaR — and 5% produce losses exceeding it. Higher confidence levels produce larger VaR figures. *Block 5*

**Constrained Optimisation**
Finding the minimum or maximum of a function subject to rules the solution must obey. In portfolio optimisation, the objective function (Sharpe ratio or volatility) is optimised subject to equality constraints (weights sum to 1) and bounds (no short selling). Uses `sco.minimize` with `method='SLSQP'`. *Blocks 3, 6*

**Covariance Matrix**
A symmetric matrix capturing how every pair of assets moves in relation to each other. Diagonal entries are each asset's own variance. Off-diagonal entries are pairwise covariances — near zero means assets move independently, high positive means they move together. The central mathematical object in portfolio volatility calculation. *Blocks 1, 3*

**Covariance vs Correlation**
Both measure pairwise asset relationships. Covariance is the raw measure — values depend on the scale of returns, hard to interpret directly. Correlation normalises to a -1 to +1 scale by dividing by the product of the two assets' standard deviations — immediately readable. The covariance matrix is used in the maths; the correlation matrix is used for human interpretation. *Block 1*

---

## D

**Data Alignment**
Ensuring that return series and factor series share identical date indices before any computation. Misalignment produces silently wrong results — the covariance matrix applied to incorrectly ordered weights assigns volatilities to the wrong assets. Fixed in this project by creating indexed pandas Series for weights and explicitly reindexing the covariance matrix to match the tickers list. *Blocks 2, 3*

**Diversification**
The mathematical reduction in portfolio volatility achieved by combining assets with low or negative covariance. When assets move independently, their bad days do not coincide — the portfolio's worst days are less severe than any individual asset's worst days. Captured precisely by the off-diagonal terms in the covariance matrix. *Blocks 1, 3*

**Domain Knowledge**
Prior understanding of the structure or drivers of the data being modelled, used to make better modelling decisions. In Chapter 11, knowing f(x) contained sin(x) led to including it as a basis function — producing a perfect fit. In the factor model, knowing equities are driven by market, style, and commodity forces informed factor selection. *Blocks 2*

---

## E

**Efficient Frontier**
The boundary of the achievable portfolio opportunity set. Every portfolio on the frontier is optimal — for its level of risk, no other portfolio delivers more return, and for its level of return, no other portfolio accepts less risk. Portfolios inside the frontier are suboptimal. Derived in this project by running 50 separate minimum variance optimisations across target return levels. *Block 3*

**Excess Return**
Portfolio return minus the risk-free rate. The portion of return earned specifically by taking risk — the first few percent earned by cash would have been available without any risk at all. The numerator of the Sharpe ratio. *Block 3*

**Expected Return**
The probability-weighted average outcome of a distribution of possible returns. In this project computed as the historical mean return — an implicit assumption that the recent past is representative of the future. The central limitation of backward-looking optimisation. *Blocks 3, 4*

---

## F

**Factor Exposure**
How sensitive an asset's returns are to a specific factor, measured as a coefficient. Also called factor loading. A market beta of 1.4 means 40% more sensitive to market moves than the market itself. The complete set of factor exposures for all assets is the factor exposure table produced in Block 2. *Block 2*

**Factor Model**
A regression framework that decomposes asset returns into systematic components (explained by factors) and an idiosyncratic component (unexplained). The factor model equation: R_i = α_i + β₁F₁ + β₂F₂ + β₃F₃ + ε_i. Implemented using `np.linalg.lstsq` — the same matrix regression from Chapter 11 applied to financial data. *Block 2*

**Fat Tails**
A property of return distributions where extreme events occur more frequently than a normal distribution predicts. Measured by positive excess kurtosis. All ten assets in this project showed positive kurtosis and rejected normality — meaning standard models systematically underestimate the probability and magnitude of extreme losses. *Blocks 1, 5*

---

## G

**Geometric Brownian Motion (GBM)**
The standard mathematical model for stock price evolution. Price moves continuously with a drift term (expected return) and a random diffusion term (volatility × random shock). The model assumes log returns are normally distributed — an assumption violated by real data as shown in Block 1. Used in Block 4 for simulation and Block 5 for simulation-based VaR. *Blocks 4, 5*

---

## H

**Historical VaR**
Value at Risk computed directly from observed daily returns with no distribution assumption. The most honest VaR estimate — captures real fat tails, negative skew, and all non-normality in the data. In this project historical 99% VaR of 57.70% for the maximum Sharpe portfolio versus analytical VaR of 23.35% — a 34 percentage point gap quantifying the fat tail cost. *Block 5*

---

## I

**Idiosyncratic Return**
The portion of an asset's return not explained by any factor — company-specific news, earnings surprises, management decisions. Captured by the residual (ε) in the factor model. Distinct from alpha: the residual fluctuates randomly day to day; alpha is the consistent directional component of what factors cannot explain. *Block 2*

**In-Sample Evaluation**
Measuring model performance on the same data used to build the model. Produces optimistic results because the model was tuned to that data. The dynamic optimiser's Sharpe of 3.380 suffers from in-sample bias — weights were evaluated against the same 60-day windows that generated them. A proper out-of-sample test applies weights to the following day's unseen returns. *Block 6*

**Itô Correction**
The term -½σ²dt in the GBM formula that adjusts for the mathematical asymmetry of log-normal compounding. Without it, the mean of simulated final values would exceed the true expected return because the right tail of the log-normal distribution inflates the arithmetic mean. The correction ensures simulated paths are centred on the true expected portfolio return. *Block 4*

---

## K

**Kurtosis**
A measure of the weight of a distribution's tails relative to a normal distribution. Normal distribution has excess kurtosis of zero. Positive excess kurtosis means fat tails — extreme events occur more frequently than normal theory predicts. In this project JPM showed the highest kurtosis at 5.523, reflecting banking sector tail risk from crisis events. *Block 1*

---

## L

**Log Return**
The natural logarithm of the price ratio: log(P_t / P_{t-1}). Used in preference to simple percentage returns because: (1) additive over time — log returns sum across periods; (2) symmetric — gains and losses are mathematically equal in magnitude; (3) directly connected to the GBM framework and normal distribution assumption. *Block 1*

**Log-Normal Distribution**
The distribution of a variable whose logarithm is normally distributed. When normally distributed log returns are compounded, the resulting price levels are log-normally distributed. Characteristics: hard floor at zero (prices cannot go negative), long right tail, positively skewed — mean exceeds median. *Block 4*

**lstsq (np.linalg.lstsq)**
NumPy's least squares solver. Finds the set of coefficients that minimises the sum of squared residuals between predicted and actual values. In Chapter 11 used for curve fitting to mathematical functions. In Block 2 used for factor model regression — same mathematics, financial application. Returns coefficients, residuals, matrix rank, and singular values. *Block 2*

---

## M

**Market Beta**
The factor loading on the market return factor. Measures how much an asset amplifies or dampens market moves. Beta above 1 amplifies — NVDA at 1.396 moves 39.6% more than the market. Beta below 1 dampens — JNJ at 0.611 moves only 61.1% as much as the market. Beta is directional — positive market moves produce positive stock moves, scaled by beta. *Block 2*

**Maximum Drawdown**
The largest peak-to-trough decline in portfolio value during a specific period. Always more severe than the start-to-end return because it captures the worst moment an investor experienced, not just the final outcome. In the 2022 stress test, maximum Sharpe portfolio maximum drawdown was -28.87% despite a -22.48% total year return. *Block 5*

**Maximum Sharpe Ratio Portfolio**
The portfolio allocation that maximises the Sharpe ratio — the best achievable return per unit of risk. Found by minimising negative Sharpe ratio using `sco.minimize`. In this project: 60.27% XOM, 17.77% NVDA, 13.61% MSFT, 8.34% JPM — Sharpe of 1.382. *Block 3*

**Mean Reversion**
The tendency of a variable to return toward its long-run average after deviating from it. Used in the Square Root Diffusion (CIR) model for interest rates and volatility — captured by the kappa parameter (speed of reversion) and theta (long-run mean). *Chapter 12*

**Minimum Variance Portfolio**
The portfolio allocation with the lowest achievable volatility regardless of return. Found by minimising `port_vol` directly. In this project: 49.37% GLD, 30.17% JNJ, 13.18% SPY — volatility of 10.47%. Demonstrates that minimum variance does not mean minimum probability of loss. *Block 3*

**Modern Portfolio Theory (MPT)**
Harry Markowitz's framework (Nobel Prize 1990) for constructing portfolios that maximise return for a given level of risk. Relies on mean returns and the covariance matrix as the complete description of portfolio properties — valid under normality assumptions. The mathematical foundation of Block 3. *Block 3*

**Moment Matching**
A variance reduction technique for Monte Carlo simulation. Standardises random draws to have exactly mean zero and standard deviation one before use: rand = (rand - rand.mean()) / rand.std(). Eliminates bias introduced by pseudo-random number generation where sample statistics imperfectly match theoretical values. *Block 4*

**Monte Carlo Simulation**
A computational method that generates thousands of random scenarios to estimate the distribution of possible outcomes. In Block 4, 10,000 GBM price paths are simulated over 252 trading days to produce the full distribution of possible portfolio values — revealing information that a single expected return estimate cannot. *Block 4*

---

## N

**Normality Test**
Statistical tests assessing whether a return distribution is consistent with a normal distribution. Three tests used: skewtest (measures skewness deviation from zero), kurtosistest (measures kurtosis deviation from zero), normaltest (combined omnibus test). P-value below 0.05 rejects normality. All ten assets returned p = 0.000. *Block 1*

**np.dot**
NumPy's dot product function. Used in portfolio volatility calculation: np.dot(weights.T, np.dot(cov_matrix, weights)) implements w^T Σ w. Also used in the factor model to apply coefficients to the factor matrix to generate predicted returns. Equivalent to the @ matrix multiplication operator. *Blocks 2, 3*

---

## O

**Overdetermined System**
A system with more equations than unknowns — no exact solution satisfies all equations simultaneously. The factor model has 817 equations (one per trading day) and four unknowns (alpha and three betas). lstsq finds the best approximate solution by minimising the total squared error across all equations. *Block 2*

---

## P

**p-value**
The probability of observing data as extreme as what was measured, assuming the null hypothesis is true. In normality testing: p ≥ 0.05 means cannot reject normality; p < 0.05 means reject normality; p = 0.000 means overwhelming evidence against normality. *Block 1*

**Path Matrix**
The (M+1) × I matrix storing all Monte Carlo simulation paths. Rows are time steps (0 to M), columns are individual simulation paths (1 to I). Row 0 contains the starting value for all paths. S[-1] gives all final values at maturity. Shape in this project: (253, 10000). *Block 4*

**Port_ret**
The portfolio return function: np.sum(mean_returns × weights). Computes the expected annual portfolio return as the weighted average of individual asset mean returns. One of two core functions called repeatedly by the optimiser. *Block 3*

**Port_vol**
The portfolio volatility function: np.sqrt(np.dot(weights.T, np.dot(cov_matrix, weights))). Computes expected annual portfolio volatility using the full covariance matrix. Not a weighted average — captures diversification through off-diagonal covariance terms. One of two core functions called repeatedly by the optimiser. *Block 3*

**Prior (Bayesian)**
The initial belief about a parameter before observing data. In Bayesian regression, a wide prior (large standard deviation) means little prior knowledge — the data does most of the work. A narrow informative prior encodes domain knowledge that constrains the estimate. *Chapter 13, Block 6*

---

## Q

**QQ Plot (Quantile-Quantile Plot)**
A visual normality test plotting sample quantiles against theoretical normal distribution quantiles. If data is normally distributed, points fall on a straight diagonal line. Deviations — particularly S-shaped curves at the ends — indicate fat tails. All assets showed S-shaped deviations in this project. *Block 1*

---

## R

**R-squared (R²)**
The proportion of an asset's return variance explained by the factor model. Ranges from 0 to 1. SPY and XOM showed R² = 1.0 (tautology — used as their own factors). GLD showed R² = 0.027 — factors explain almost nothing about gold's movements. MSFT showed R² = 0.761 — well explained by market and growth factors. *Block 2*

**Regime**
A period characterised by a consistent set of market conditions and return drivers. The 2021-2022 energy supercycle was one regime — favouring XOM and commodity assets. The 2023 AI buildout and rate cycle peak was another — favouring NVDA, MSFT, JPM, and GLD. Static optimisation anchors to historical regimes; Bayesian dynamic optimisation responds to regime shifts. *Block 6*

**Residual (ε)**
The difference between the factor model's predicted return and the actual observed return on any given day. Random, fluctuating daily, averaging toward zero over time. Distinct from alpha — residuals are noise; alpha is the consistent directional component of unexplained returns. *Block 2*

**Risk-Free Rate**
The return available to any investor with zero risk — typically short-term government bonds. Used as the baseline in Sharpe ratio calculation: excess return = portfolio return minus risk-free rate. Set to zero in this project for simplicity. *Block 3*

**Rolling Window**
A fixed-length window of observations that moves forward through time one period at a time. In Block 6, a 60-day rolling window computes mean returns using only the most recent 60 trading days — continuously dropping the oldest observation and adding the newest. Produces time-varying estimates that reflect current market conditions. *Block 6*

---

## S

**Sharpe Ratio**
The primary measure of risk-adjusted portfolio quality: (portfolio return - risk-free rate) / portfolio volatility. Measures excess return earned per unit of risk taken. Higher is always better regardless of absolute return or volatility levels. In this project the maximum Sharpe portfolio achieved 1.382 — meaning 138.2 cents of excess return per 1% of volatility accepted. *Blocks 3, 6*

**Sign Flip**
The technique of negating an objective function to convert a maximisation problem into a minimisation problem, because scipy only minimises. Used for Sharpe ratio maximisation (minimise negative Sharpe) and investor utility maximisation (minimise negative utility) — first encountered in Chapter 11, reappearing in Block 3. *Blocks 3*

**Simulation VaR**
Value at Risk computed by reading percentiles directly from the Monte Carlo simulation output. The 5th percentile of simulated annual returns gives the 95% VaR. Better than analytical VaR because it captures log-normal compounding effects, but still based on GBM which assumes normally distributed daily returns. *Block 5*

**Skewness**
A measure of the asymmetry of a return distribution. Normal distribution has skew of zero. Negative skew means the left tail is longer — large negative returns are more extreme than large positive returns. Positive skew means the right tail is longer — large positive returns are more extreme. NVDA showed positive skew (0.436); SPY showed negative skew (-0.232). *Block 1*

**SLSQP**
Sequential Least Squares Programming — the optimisation algorithm used in this project. Designed specifically for constrained optimisation problems with both equality constraints and bounds. Passed as `method='SLSQP'` to `sco.minimize`. *Blocks 3, 6*

**Standalone Sharpe Ratio**
The Sharpe ratio of a single asset held in isolation: return / volatility. Useful for initial asset screening but not sufficient for portfolio construction — an asset with a lower standalone Sharpe may improve portfolio Sharpe more than one with a higher standalone Sharpe, due to its lower covariance with existing holdings. *Block 3*

**Stress Test**
Application of a specific historical market scenario to a portfolio to measure actual losses under that scenario. Complements VaR by examining what happens beyond the confidence threshold. In this project the 2022 rate shock stress test showed minimum variance losing more than maximum Sharpe — a finding VaR alone would not have revealed. *Block 5*

---

## T

**Tickers List**
The ordered list of asset identifiers used throughout the project. Critical for data alignment — the covariance matrix columns and mean returns index must match this order exactly. Misalignment produces silently wrong volatility assignments. *Blocks 1, 2, 3*

**Time Value of Money**
The principle that a unit of currency today is worth more than the same unit in the future, because it can be invested to earn returns in the interim. Underlies the discounting of future option payoffs in Black-Scholes and the Itô-corrected drift in GBM. *Block 4*

---

## V

**Value at Risk (VaR)**
A risk measure answering: what is the maximum loss not expected to be exceeded over a given time period at a given confidence level? Always expressed as a positive loss figure. A 95% VaR of 8.91% means 95% of scenarios produce losses no worse than 8.91% — and 5% produce losses exceeding it. VaR says nothing about how bad losses are beyond the threshold. *Block 5*

**Value/Growth Factor**
The return spread between value stocks (IWD ETF) and growth stocks (IWF ETF). Positive factor return means value outperforming growth. Assets with negative loading on this factor are growth stocks — they suffer when value rotates in. All technology names showed negative value/growth beta, confirming growth character. JPM showed positive loading, confirming value character. *Block 2*

**Variance Reduction**
Techniques that improve Monte Carlo simulation accuracy without increasing the number of paths. Moment matching is used in this project — forcing random draws to have exactly the theoretical mean and standard deviation, eliminating pseudo-random sampling error. *Block 4*

**Volatility**
The annualised standard deviation of returns — the primary measure of investment risk in this project. Not a weighted average at the portfolio level — determined by the full covariance matrix including all pairwise asset relationships. A portfolio of two assets with near-zero covariance achieves volatility lower than either asset delivers alone. *Blocks 1, 3*

---

## W

**Weights**
The allocation proportions assigned to each asset in the portfolio. Must sum to 1 (full investment) and be non-negative (no short selling) in this project. Three sets of weights are produced: maximum Sharpe, minimum variance, and dynamic Bayesian (one per trading day in Block 6). *Block 3*

---

## Y

**y Vector**
In the factor model regression, the one-dimensional array of a single asset's daily returns — 817 observations for one stock. The variable being explained by the factor matrix. A different y vector is used on each iteration of the loop — one per asset — while the factor matrix remains constant. *Block 2*

---

*This glossary covers all key terms introduced across the six blocks of the Quantitative Investment Pipeline project.*
