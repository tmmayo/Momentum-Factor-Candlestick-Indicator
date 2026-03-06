# Candlestick Pattern Indicators and Momentum Signal Analysis

Author: tmmayo  
Date: February 2026  

## Project Overview

This project studies the predictive power of candlestick patterns and technical momentum indicators for equity returns.  

The analysis evaluates whether commonly used technical signals contain statistically significant information for predicting future market movements. We test multiple indicators using regression analysis, directional accuracy, and risk-adjusted performance metrics.

In addition, we construct a composite signal that aggregates the most effective indicators and evaluate its performance within a dynamic asset allocation strategy.

The analysis combines statistical modeling, signal evaluation, and visualization to better understand how technical indicators behave across different time horizons.

---

## Data

- **SPX daily data**: 2000 – 2026  
- **Nasdaq Composite Index**: 2010 – 2026 (strategy backtest)

---

## Indicator Set

The study includes **17 technical indicators**, categorized into several groups:

### Single-candle patterns
- Hammer  
- Inverted Hammer  
- Shooting Star  

### Multi-candle reversal patterns
- Bullish Engulfing  
- Bearish Engulfing  
- Morning Star  
- Evening Star  

### Continuation patterns
- Rising Three Methods  
- Falling Three Methods  

### Momentum indicators
- RSI Oversold / Overbought  
- RSI Divergence  

### Trend indicators
- MACD Crossovers  

Each indicator generates a trading signal:

$$
s_t \in \{-1,0,1\}
$$

where

- $1$ = bullish signal  
- $-1$ = bearish signal  
- $0$ = neutral  

---

## Signal Correlation Analysis

To assess redundancy among indicators, we compute the **Jaccard similarity matrix** across all binary signals.

The Jaccard similarity between two indicators is defined as:

$$
J(A,B) = \frac{|A \cap B|}{|A \cup B|}
$$

Results indicate that most indicators exhibit **low pairwise similarity**, suggesting limited overlap in signal activation.

This implies that combining signals may provide diversification benefits.

---

## Horizon Analysis

Predictive power is evaluated across multiple holding horizons:

$$
h = 1,2,...,10
$$

Empirical results show that most indicators do **not exhibit immediate predictive power at \(h=1\)**.

Instead, signal effectiveness typically appears with delay, with the strongest predictive performance around:

$$
h \approx 5
$$

This suggests that technical signals often capture **short-term momentum or reversal dynamics that unfold gradually rather than instantly**.

---

## Performance Evaluation

Signals are evaluated using three primary metrics:

### Directional Accuracy

Measures the proportion of times a signal correctly predicts return direction.

### Regression Significance

We estimate the regression model:

$$
r_{t+h} = \alpha + \beta s_t + \epsilon_t
$$

where

- $r_{t+h}$ = future return  
- $s_t$ = indicator signal  

Statistical significance is assessed using **Newey-West adjusted t-statistics** to correct for autocorrelation and heteroskedasticity.

### Sharpe Ratio

Single-strategy backtests evaluate risk-adjusted performance:

$$
SR = \frac{E[R]}{\sigma(R)}
$$

Indicators are ranked according to:

1. Newey-West t-statistic  
2. Sharpe ratio  
3. Directional accuracy  

---

## Empirical Results

Performance is analyzed in two regimes:

- **Full Sample (2000–2026)**
- **Post-COVID Period**

### Top indicators (Full Sample)

- Dark Cloud Cover (reverted)
- Bullish Engulfing (reverted)
- Inverted Hammer
- RSI Oversold
- RSI Bullish Divergence

### Top indicators (Post-COVID)

- MACD Bearish Cross (reverted)
- Inverted Hammer
- RSI Oversold
- Dark Cloud Cover (reverted)

These results suggest that some signals remain robust across regimes while others exhibit **regime-dependent behavior**.

---

## Composite Signal Construction

To improve robustness, we construct a composite signal using **inverse-variance weighting**:

$$
w_i = \frac{1/\sigma_i^2}{\sum_j 1/\sigma_j^2}
$$

where $\sigma_i$ denotes the volatility of each strategy return series.

The composite signal is defined as:

$$
S_t = \sum_i w_i s_{i,t}
$$

This approach assigns higher weights to indicators with more stable return distributions.

---

## Strategy Construction

We implement a **signal-driven asset allocation strategy** that adjusts exposure between an equity index and a defensive asset.

Equity weight is defined as:

$$
w_{stock,t} = 0.8 + tilt \cdot signal_t
$$

where

- baseline equity allocation = 80%
- signal controls tactical exposure adjustments

Portfolio return is calculated as:

$$
r_t = w_{stock,t} r_{stock,t} + (1-w_{stock,t}) r_{fx,t}
$$

---

## Strategy Interpretation

The strategy maintains a **core exposure to equities** while dynamically adjusting risk exposure based on predictive signals.

- Bullish signals increase equity allocation
- Bearish signals reduce equity exposure
- Neutral signals maintain baseline allocation

This approach functions as a **benchmark-tilting strategy** that overlays tactical signals on top of a long-term investment portfolio.

---

## Backtest Results

The composite strategy achieves:

- **Sharpe Ratio:** 0.50  
- **Annualized Return:** 1.09%  
- **Very low volatility and drawdown**

Because signals are sparse, the strategy behaves more like a **market timing overlay** rather than a full allocation model.

---

## Future Improvements

Potential extensions include:

- exposure scaling  
- conditional long-only strategies  
- integration into multi-factor alpha models  

---


