# Project Narrative
## Quantitative Investment Pipeline — The Story Behind the Numbers

---

## The Starting Point — A Thesis and a Question

This project began with a straightforward investment conviction: artificial intelligence and data infrastructure represent one of the most significant structural growth opportunities of the current decade. Companies like NVIDIA, Palantir, Alphabet, and Microsoft sit at the centre of that opportunity — either building the compute infrastructure, developing the software platforms, or deploying AI at scale across enterprise and government. The thesis was that a portfolio deliberately tilted toward these names should outperform the broad market over the medium term.

But conviction alone is not enough. The honest question any serious analyst must ask is: what does the data actually say, and where does the data challenge the thesis? That question is what this project set out to answer — systematically, quantitatively, and without flinching from uncomfortable findings.

The universe was deliberately constructed to test the thesis rather than simply confirm it. Five technology and AI names — NVDA, TSLA, PLTR, GOOGL, MSFT — were paired with genuinely different assets: JPMorgan Chase for financial sector exposure, Johnson & Johnson for healthcare defensiveness, ExxonMobil for commodity and energy exposure, the S&P 500 ETF as a broad market benchmark, and gold as a safe haven diversifier. The combination was intentional — a growth tilt with genuine diversification, allowing the data to reveal whether the thesis was supported and where the risks actually lived.

---

## What the Data Said First — Block 1

The first finding was sobering and important. Every single asset in the universe — all ten of them — rejected the assumption of normally distributed returns at the highest possible level of statistical confidence. P-values of zero across the board. Fat tails everywhere. This matters because almost every model built in the subsequent blocks rests, to some degree, on the normality assumption. The simulation assumes it. The analytical VaR formula assumes it. The mean-variance optimisation framework is only fully valid under it.

Choosing to continue with these models after knowing the assumption is violated is not intellectual dishonesty. It is standard practice — these models remain useful approximations even when their assumptions are imperfect. But documenting the violation honestly at the outset changes how the outputs are interpreted. Every number produced by later blocks carries an asterisk: valid under normality assumptions that the data does not fully support.

The correlation matrix offered the first genuinely encouraging piece of evidence for the thesis. Gold showed correlations below 0.15 with every equity name in the universe. JNJ showed near-zero correlation with the technology cluster. The defensive and commodity assets were doing what they were supposed to do — moving largely independently of the growth names. The diversification thesis was supported in the data.

---

## Decomposing What Actually Drives Each Asset — Block 2

The factor model was where the investment thesis met its most rigorous test. Three factors were chosen based on economic reasoning: the broad market return as the primary driver of individual stock performance, the value/growth spread to capture style factor exposure, and oil prices to capture commodity cycle sensitivity. These are not arbitrary choices — they reflect genuine economic mechanisms that theory predicts should matter.

The results confirmed some expectations and challenged others. Every technology name showed negative value/growth beta — confirmed growth stocks that suffer when institutional money rotates from growth to value. JPMorgan showed a positive value beta of 0.979 — sitting almost exactly opposite the technology cluster on the style dimension, providing genuine style diversification. XOM's factor model produced a mathematically perfect fit with the oil factor, confirming it as a pure energy play with no meaningful market or style exposure. All of this matched the investment rationale built into the portfolio construction.

The alpha column told the most interesting story. NVDA generated 21.46% annual alpha — genuine, persistent outperformance above and beyond what its market exposure, growth tilt, and oil sensitivity would predict. This is the AI infrastructure moat expressed as a number. The market has consistently rewarded NVDA above what any systematic factor model would forecast, and over 817 trading days that premium has been substantial and real.

PLTR told the opposite story. Despite its compelling narrative as an AI and data analytics company, PLTR generated -3.14% annual negative alpha. It consistently underperformed its own factor predictions — delivering less return than its market beta and growth tilt alone would suggest. High volatility, negative alpha, and low factor explanatory power (R-squared of 0.271) make it the weakest profile in the portfolio. The data challenged the narrative here directly and unambiguously.

Gold's R-squared of 0.027 was the confirmation the diversification thesis needed. The three factors — market, style, oil — explain essentially nothing about gold's daily movements. It operates in an almost completely separate return universe from equities, driven by inflation expectations, dollar strength, and geopolitical uncertainty rather than anything in the equity factor model. Including gold is not just intuitively sensible — it is quantitatively justified.

---

## Finding the Optimal Allocation — Block 3

The portfolio optimisation produced results that were both illuminating and humbling. The maximum Sharpe ratio portfolio — the allocation that maximises return per unit of risk — assigned 60% to ExxonMobil. Not NVIDIA. Not any of the technology names that anchor the investment thesis. ExxonMobil.

This is the backward-looking optimiser telling a story rooted in a specific historical regime. The dataset spans 2020 to 2023, a period that includes the 2021-2022 energy supercycle — driven by post-COVID demand recovery and the geopolitical supply shock following Russia's invasion of Ukraine. During that period XOM generated 37.78% annualised returns with moderate volatility, producing the highest standalone Sharpe ratio in the universe. The optimiser exploited this correctly given the data it had.

NVDA received 17.77% — meaningful but secondary. Its 21.46% alpha justified the allocation but its 51.4% annualised volatility limited it. The optimiser found that adding more NVDA beyond a certain weight costs more in volatility than it gains in return, particularly because NVDA is already correlated with the other technology holdings in the portfolio.

The minimum variance portfolio made a completely different decision. With 49% in gold and 30% in JNJ, it concentrated almost entirely in the two assets with the lowest individual volatility and the lowest covariance with everything else. The result was a portfolio volatility of 10.47% — lower than any individual asset in the universe could achieve alone. This is diversification working at maximum efficiency, the covariance matrix producing genuine risk cancellation rather than just averaging.

The equal weight portfolio — the naive benchmark — achieved a Sharpe of 0.846, beaten by both optimised strategies. The difference between 0.846 and 1.382 is not abstract — it represents a substantial improvement in the efficiency of risk deployment that systematic optimisation delivers over intuitive allocation.

---

## Understanding the Range of Possible Futures — Block 4

The maximum Sharpe portfolio has an expected return of 34.17%. That number, presented alone, creates a misleading impression of certainty. Block 4 replaced that single number with the full picture.

Across 10,000 simulated years, the portfolio's outcomes ranged from a loss of 41.8% to a gain of 241.9%. The median outcome was a 36.6% gain — below the mean of 40.7%, because the right tail of exceptional years pulled the average upward. One year in ten ended in loss. Roughly one year in three produced a gain exceeding 50%.

The minimum variance portfolio told a counterintuitive story. Despite lower volatility, it showed a 30.9% probability of loss — three times higher than the maximum Sharpe portfolio's 10.5%. The mechanism is straightforward once understood: the minimum variance portfolio's expected return of 5.70% provides a thin buffer above zero. A relatively modest bad year erases it entirely. The maximum Sharpe portfolio's 40.7% expected return requires a severe shock to push into loss territory despite accepting more volatility. Minimum variance does not mean minimum probability of loss — a finding that challenges a common and dangerous misconception.

---

## Quantifying the Cost of Modelling Assumptions — Block 5

Three approaches to Value at Risk were computed for each portfolio and compared directly. The comparison produced the most practically important number in the entire project.

For the maximum Sharpe portfolio at 99% confidence, the historical VaR was 57.70%. The analytical VaR — computed under normality assumptions — was 23.35%. A gap of 34 percentage points. That gap is not a modelling error. It is the direct, quantifiable cost of assuming normally distributed returns when real returns have fat tails. The -7.31% single-day loss on 9th May 2022 — a genuine market event that a normal distribution assigns near-zero probability — is embedded in the historical data and drives the historical VaR far above what any normal-distribution model would predict.

This finding connects directly back to Block 1. The normality rejection that was documented as a limitation at the project's outset has now been given a specific monetary consequence: if you manage risk using analytical VaR under normality assumptions, you are underestimating your 99% tail risk by 34 percentage points for this specific portfolio. That is not a theoretical concern. It is an operational risk.

The 2022 stress test reinforced the lesson from a different angle. Minimum variance lost more than maximum Sharpe in 2022 — 23.54% versus 22.48% — because the historical covariance structure the optimiser relied upon broke down during the rate shock. Gold fell alongside equities in the early stages of the crisis as investors raised cash. JNJ proved less defensive than historical covariances suggested. XOM's 60% weight in the maximum Sharpe portfolio, meanwhile, provided a significant cushion as energy was the single best-performing sector of 2022. The model's assumptions about diversification held better for maximum Sharpe than for minimum variance in this specific regime — which the static optimisation framework had no mechanism to anticipate.

---

## Responding to Regime Change — Block 6

The central limitation identified in Block 3 — that static optimisation anchors permanently to historical regimes — was addressed directly in Block 6. By replacing fixed historical mean returns with 60-day rolling estimates, the optimiser was given the ability to respond to changing market conditions rather than remaining anchored to a specific period's data.

The regime shift visible in the results was dramatic. By December 2023, XOM's 60-day rolling return had fallen to -41.98% annually — the energy supercycle had faded completely. GLD's rolling return had risen to 51.42% as rate expectations shifted and gold surged. JPM reached 74.95% as financial sector momentum built toward the anticipated end of the rate hiking cycle. The dynamic optimiser responded correctly to all of these shifts — reducing XOM from 60% to zero and replacing it with 61% GLD and 26% JPM.

The dynamic Sharpe of 3.380 must be interpreted carefully. It is inflated by in-sample evaluation — the weights for each day were generated using the same data on which performance was measured. A proper out-of-sample test, applying each day's weights to the following day's actual returns, would produce a lower and more honest Sharpe ratio. The directional value of the dynamic approach is genuine — it correctly identified and responded to the energy-to-gold regime shift. The magnitude of the performance improvement is overstated by the evaluation methodology.

There is also a subtler limitation worth stating explicitly. The 60-day rolling mean is a momentum signal. It assigns higher expected returns to assets that have recently performed well, creating a tendency to chase peaks. PLTR's 401.96% rolling return in January 2021 reflected a post-IPO surge that had already concluded. Assigning heavy weight at that point meant buying after the peak. No quantitative signal can reliably distinguish a sustainable regime shift from a temporary surge. That distinction requires human judgment — domain knowledge about the economic forces driving the signal and whether they are likely to persist.

---

## The Tension That Runs Through Everything

The most important insight this project produced is not a number. It is the observation that quantitative models and human judgment are not alternatives — they are complements.

The factor model cannot tell you whether NVDA's alpha will persist. It can only tell you it has existed historically. The optimiser cannot distinguish XOM's 2022 energy supercycle from a permanent structural shift in energy markets. It can only observe what happened. The VaR calculation cannot capture the specific character of a future crisis that differs from any historical precedent. It can only measure what past crises produced.

What the models do — and do well — is impose rigorous structure on investment decisions. They force explicit choices about which factors matter, which assets provide genuine diversification, and what the realistic range of outcomes looks like rather than a single comfortable estimate. They produce numbers that can be challenged, debugged, and refined — unlike intuitive allocation decisions that often cannot be articulated precisely enough to be tested.

The combination of structured quantitative analysis and informed human judgment is how serious investment management works. This project demonstrates the quantitative half of that combination — the tools, the methodology, and the intellectual honesty about where those tools reach their limits.

---

*Built as part of a self-directed transition into investment analysis, working through Python for Finance by Yves Hilpisch. All six blocks implement and extend techniques from Chapters 11–13 applied to a real portfolio construction problem.*

*Donald | 2024*
