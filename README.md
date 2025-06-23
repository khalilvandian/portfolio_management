# 📈 Benchmarking Portfolio Optimization Techniques on Nasdaq Data

This project benchmarks three portfolio optimization strategies—**Constant Weights**, **Markowitz Optimization**, and **LSTM-based Forecasting**—using real historical stock data from the Nasdaq. Developed as part of the *Big Data in Business, Economics and Society* course (a.y. 2024–2025), the project investigates how traditional and machine learning methods compare in managing financial portfolios.

---

## 📊 Project Overview

**Goal:**  
Evaluate and compare the performance of three portfolio management techniques:

1. **Constant Weights Portfolio** (baseline)
2. **Markowitz Mean-Variance Optimization** (with Ledoit-Wolf shrinkage)
3. **LSTM-based Deep Learning Forecasting**

The evaluation focuses on **risk-return tradeoffs**, **diversification**, and **predictive adaptability**, using a 10-year dataset of S&P 500 stocks selected from top sectors.

---

## 🧩 Dataset Description

- **Source:** [nasdaq.com/market-activity/indexes/screener](http://nasdaq.com/market-activity/indexes/screener)
- **Period:** 2015-06-13 to 2025-06-12  
- **Frequency:** Daily business days
- **Assets:** 15 randomly selected stocks from:
  - Consumer Discretionary
  - Information Technology
  - Financials  
- **Risk-Free Option:** Added as a constant-price asset (`NoRisk`) with 0% return

---

## 📁 Data Columns

- `Open`, `Close`, `High`, `Low`, `Volume`
- Derived: Moving averages (10, 20, 50), Daily returns

---

## 🔍 Exploratory Data Analysis

- Moving averages and return visualizations
- Sectoral comparisons of daily returns
- Correlation heatmaps between assets
- Risk vs Expected Return plots
- Scatterplot comparisons of returns

---

## 📌 Strategy 1: Constant Weights Portfolio

**Method:**  
Equal budget allocation to all assets.

**Performance:**  
- Buy & Hold since: 2024-12-01  
- Final Value: `$96,057.77`  
- Total Gain: `-3.94%`  
- Serves as baseline for comparison

---

## 📌 Strategy 2: Markowitz Optimization

**Methodology:**
- Daily returns grouped by sector
- Equal investment per sector (33.3%)
- Within each sector:
  - Ledoit-Wolf shrinkage covariance estimation
  - Minimum variance optimization (SLSQP)
- Portfolio = Weighted sum of sector portfolios

**Special Considerations:**
- Risk-free asset (`NoRisk`) used in Tangency Portfolio
- Efficient Frontier and Sharpe Maximization plotted

**Results:**
- **Min Variance:** Lower volatility, high NoRisk allocation (52%)
- **Max Sharpe:** High return, but risk concentrated (XL, LEG = 100%)
- **Tangency Portfolio:** Balanced diversification

---

## 📌 Strategy 3: LSTM-Based Forecasting

**Model:**
- 2 stacked LSTM layers + Dropout
- Dense output for closing price
- Sequence length: 30 days
- Optimizers: Adam, RMSprop

**Tuning:**
- Grid search over LSTM units, dropout rates, optimizers
- Training: all historical
- Validation: 6 months
- Testing: last 6 months

**Portfolio Construction:**
- **Top-k strategy:** Buy top-k assets based on LSTM forecast
- `k ∈ {1, 3, 4, 5}` tracked monthly
- Rebalancing monthly

**Results:**
- **Top-3 to Top-5:** Best return-risk tradeoff
- **Top-1:** Highest peak gain (+10%) but unstable

---

## 📌 Summary of Results

| Strategy               | Return        | Risk / Volatility        | Notes                                                  |
|------------------------|---------------|---------------------------|---------------------------------------------------------|
| Constant Weights       | -3.94%        | Low                       | Stable but not responsive                              |
| Markowitz Min Variance | -2.18%        | Very Low                  | Safe, high NoRisk allocation                           |
| Markowitz Max Sharpe   | -29.82%       | High                      | High return potential, but risky and concentrated      |
| Tangency Portfolio     | Slightly better than Max Sharpe | Medium | Balanced tradeoff with slight risk-free allocation     |
| LSTM Top-3/Top-5       | Varies by month, up to +10% | Medium-High | Most adaptive and responsive to trends                |

---

## 💡 Conclusions

- **Markowitz models** demonstrate classic tradeoffs: Sharpe maximization leads to risk concentration; variance minimization improves diversification.
- **LSTM-based predictions** outperform in dynamic environments by adjusting allocation monthly.
- **NoRisk asset** improves stability and serves as a useful hedge.
- Strategy choice should depend on investor risk tolerance and rebalancing flexibility.

---

## 🔧 Future Improvements

- Add **seasonality modeling** in LSTMs
- Use **daily prediction granularity**
- Implement **more refined weighting** for top-k selections
- Enable **rolling monthly retraining** for LSTM models
