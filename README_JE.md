# 🎰 Dark Fortune — Jackpot Edition

> A mathematically rigorous progressive jackpot slot game, validated through 10,000,000 Monte Carlo simulation iterations. Designed around an original organic jackpot accumulation mechanism with a 96% total RTP and a 4% casino margin.

---

## 🎮 Play the Live Demo

**👉 [Click here to play Dark Fortune — Jackpot Edition](https://dinkotrendafilov.github.io/Dark-Fortune-Jackpot-Edition/dark_fortune_jackpot_edition.html)**

No installation needed. Just click and play!

---

## 📁 Project Structure


**Features:**
- Animated 5×4 reel grid with smooth spin animation
- Dynamic paytable — updates in real-time with bet changes
- Prioritized win line visualization (5-of-5 > 4-of-5 > 3-of-5 > 2-of-5)
- Win line color coding: gold (5-of-5), blue (4-of-5), white (other)
- Synthesized Web Audio sound hierarchy
- Progressive jackpot pool counter with 3.5-minute triumphant melody on trigger
- Auto-spin with melody stop on next spin
- Bet per line: 1–1,000 credits, Active lines: 1–20

## 📌 Project Overview

| Parameter | Value |
|-----------|-------|
| Reels | 5 |
| Rows | 4 |
| Paylines | 20 (fixed) |
| Symbols | 30 unique |
| Total combinations | 24,300,000 |
| Base RTP | **92.93%** |
| Jackpot RTP | **3.00%** |
| Total RTP | **95.93%** |
| Casino Margin | **4.07%** |
| Simulation spins | 10,000,000 |

---

## 💡 Original Mechanism — Organic Jackpot Accumulation

The jackpot pool is funded **exclusively by losing lines** — lines where no matching symbols occur (1-of-5 match, probability ~70.37%). This is a fundamentally different approach from the industry standard of deducting a percentage from every bet:

- **2-of-5 match** → pays **2×** the line bet directly
- **3-of-5 match** → pays **25×** the line bet directly
- **4-of-5 match** → pays **420×** the line bet directly
- **5-of-5 match** → pays **20,000×** the line bet directly
- **1-of-5 match** (losing line, ~70%) → contributes **4.263%** of bet to jackpot pool

The jackpot triggers randomly at **1 in 5,000** per spin — the player never knows when it will hit.

---

## 📊 Validated RTP Distribution (10M Simulation)

| Match Group | Probability | Coefficient | RTP Contribution |
|-------------|-------------|-------------|-----------------|
| 2 of 5 | 28.57% | 2× | ~57.14% |
| 3 of 5 | 1.04% | 25× | ~26.00% |
| 4 of 5 | 0.018% | 420× | ~7.52% |
| 5 of 5 | 0.0001% | 20,000× | ~2.47% |
| **Base Total** | | | **~93.13%** |
| Progressive Jackpot | organic | variable | **2.99%** |
| **Grand Total** | | | **~96.12%** |

---

## 💎 Progressive Jackpot Statistics (10M Spins)

| Metric | Value |
|--------|-------|
| Jackpot hits | 2,078 |
| Avg spins between hits | 4,812 (theoretical: 5,000) |
| Avg jackpot payout | 2,881 credits |
| Min jackpot payout | 0.77 credits |
| Max jackpot payout | 26,029 credits |
| Jackpot RTP | **2.993%** (target: 3.00%) ✅ |
| Total contributing lines | 140,735,767 |
| Total contributed | 5,999,535 credits |
| Total paid out | 5,986,770 credits |

---

## 📈 Hit Rate Analysis

| Metric | Value |
|--------|-------|
| Overall hit rate (any win) | **96.81%** |
| Avg winning lines per spin | 5.93 / 20 |
| Line hit rate | 29.63% |
| Avg losing lines per spin | 14.07 / 20 |
| Zero-win spins | **3.19%** |

---

## 🎯 5-of-5 Match Validation

| Metric | Value |
|--------|-------|
| Theoretical frequency | 1 in 40,500 spins |
| Actual frequency | 1 in 42,194 spins |
| Theory / Actual ratio | 0.96× ✅ |
| Bayesian 95% CI | [1 in 37,151 — 1 in 47,911] |
| Theoretical within CI | ✅ Yes |

---

## 💀 Survival Analysis (10,000 initial credits, 20 credits/spin)

| Spins | Survival | Bust | Avg Balance | Avg Survivor | Avg Bust At |
|-------|----------|------|-------------|--------------|-------------|
| 500 | **100.00%** | 0.00% | 9,671 | 9,671 | — |
| 1,000 | **100.00%** | 0.00% | 9,373 | 9,373 | — |
| 2,000 | **100.00%** | 0.00% | 8,574 | 8,574 | — |
| 5,000 | 76.20% | 23.80% | 6,214 | 8,151 | spin 4,147 |
| 10,000 | 31.50% | 68.50% | 4,301 | 13,624 | spin 6,196 |
| 25,000 | 11.10% | 88.90% | 2,159 | 19,341 | spin 8,300 |

**Key insight:** Players who survive 25,000 spins average **19,341 credits** — nearly **double** their starting capital of 10,000. This is the characteristic jackpot effect: most players lose gradually, while jackpot winners achieve exceptional returns.

---
---

## 🛠️ Technical Stack

| Component | Technology |
|-----------|------------|
| Mathematical model | Python 3.14 |
| Statistical validation | `scipy`, `numpy`, `matplotlib` |
| Bayesian inference | `scipy.stats.beta` |
| Simulation | Monte Carlo — 10,000,000 iterations |
| Frontend demo | Vanilla HTML5 / CSS3 / Web Audio API |

---

## 📁 Project Structure

```
dark-fortune-jackpot-edition/
├── README.md
├── SDD.md
├── Dark_Fortune_Jackpot_Edition.py     # Full Python mathematical model
├── dark_fortune_jackpot_edition.html   # Playable HTML5 demo
└── results/
    └── simulation_10M_results.txt
```

---

## 👤 Author

**Dinko Trendafilov**
- ML Certificate — SoftUni (Grade: 5.84/6.00)
- GitHub: [DinkoTrendafilov](https://github.com/DinkoTrendafilov)

---

*This project is a mathematical and technical demonstration. No real money is involved.*
