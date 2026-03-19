## Setup & How to Run

```bash
pip install pandas openpyxl matplotlib seaborn scipy jupyter
jupyter notebook trader_sentiment_analysis.ipynb
```

Place `fear_greed_index.csv` and `historical_data.xlsx` in the same directory as the notebook, then run all cells top-to-bottom.

---

##  Datasets

| Dataset            | Rows    | Cols | Source          |

| Fear/Greed Index   | 2,644   | 4    | Alternative.me  |
| Hyperliquid Trades | 211,224 | 16   | Hyperliquid DEX |

- **Date overlap:** March 2023 – February 2025
- **Traders:** 32 unique accounts · **Coins:** 246 unique tokens
- **Missing values:** None · **Duplicates:** None

---

##  Methodology

1. **Data Cleaning:** Parsed Unix-ms timestamps → dates; collapsed 4 Fear/Greed classes → binary `Fear`/`Greed`; filtered to trade closes for PnL events.
2. **Alignment:** Inner join on date; overlap = Mar 2023 – Feb 2025 (184k+ merged rows).
3. **Metrics engineered:** Daily PnL per trader, win rate, avg trade size, leverage proxy (`Size USD / |Start Position|`), long/short ratio, trade frequency.
4. **Analysis:** Compared PnL/behavior across sentiment; Welch t-test for significance; median-split segmentation (leverage, frequency, win rate tier).

---

##  Key Insights

| # | Insight                                | Numbers |

| 1 | **Fear days yield higher average PnL** | $169,744 (Fear) vs $96,111 (Greed) |
| 2 | **Infrequent traders are Fear-day specialists** | 3.75× more PnL on Fear vs Greed |
| 3 | **High leverage is a Greed-day trap** | 81% PnL drop on Greed days for high-lev segment |
| 4 | **Consistent winners are sentiment-agnostic** | ~$130k on both Fear & Greed days |
| 5 | **Long bias overcrowds on Greed days** | 64% long (Greed) vs 40% (Fear) |

---

##  Strategy Recommendations

### Strategy 1: "Fear-First" Rule — for High-Leverage Traders
> **When FG Index < 40 (Fear):** Deploy positions at normal leverage.  
> **When FG Index > 60 (Greed):** Cut leverage by ≥50%, reduce open exposure.  
> *High-leverage traders earn 5× more on Fear days than Greed days.*

### Strategy 2: "Wait & Strike" — for Patient / Infrequent Traders
> **When FG Index < 40 (Fear):** Actively open high-conviction positions.  
> **When FG Index > 60 (Greed):** Hold winners, avoid new longs, prefer exits.  
> *Infrequent traders earn 3.75× more on Fear days — their edge is patience, not activity.*

---
*Analysis by: R. Yaswanth Durga Naik | Submitted to Primetrade.ai Hiring Team*
