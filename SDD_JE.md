# Software Design Document (SDD)
## Dark Fortune — Jackpot Edition

| Field | Value |
|-------|-------|
| Document Version | 1.0 |
| Author | Dinko Trendafilov |
| Status | Final |
| Validated | 10,000,000 Monte Carlo iterations |
| Last Updated | July 2026 |

---

## 1. Introduction

### 1.1 Purpose
This document describes the mathematical design, game mechanics, jackpot accumulation model, and software architecture of **Dark Fortune — Jackpot Edition**, a custom progressive jackpot slot game validated through 10,000,000 Monte Carlo simulation iterations.

### 1.2 Scope
- Game math model and RTP design
- Original organic jackpot accumulation mechanism
- Statistical validation methodology (Monte Carlo + Bayesian)
- Survival analysis framework
- Frontend demo implementation

### 1.3 Key Definitions

| Term | Definition |
|------|------------|
| RTP | Return to Player — total winnings / total wagered over time |
| Hit Frequency | % of spins returning at least one winning payline |
| Losing Line | A payline with 1-of-5 or 0-of-5 matching symbols — no direct payout |
| Organic Accumulation | Jackpot pool funded by losing lines, not by a deduction from every bet |
| Bust Rate | % of simulated sessions ending with zero playable balance |
| Bayesian CI | Credible interval derived from Beta posterior distribution |

---

## 2. Game Specification

| Parameter | Value |
|-----------|-------|
| Reels | 5 |
| Rows | 4 |
| Paylines | 20 fixed |
| Symbol pool | 30 unique |
| Total combinations | 30^5 = 24,300,000 |
| Min bet per line | 1 credit |
| Max bet per line | 1,000 credits |
| Initial balance (demo) | 10,000 credits |

---

## 3. Combinatorial Foundation

All 24,300,000 possible reel combinations are enumerated exhaustively. Each is classified by the maximum matching symbol count per payline evaluation.

| Match Count | Combinations | Probability | 1 in N |
|-------------|-------------|-------------|--------|
| 1 of 5 | 17,100,720 | 70.3733% | 1.4 |
| 2 of 5 | 6,942,600 | 28.5704% | 3.5 |
| 3 of 5 | 252,300 | 1.0383% | 96.3 |
| 4 of 5 | 4,350 | 0.0179% | 5,586 |
| 5 of 5 | 30 | 0.0001% | 810,000 |

**5-of-5 frequency per spin (20 lines):**
```
P(5-match on any line) = P(single line) × 20 = 0.0001234% × 20 = 0.002469%
→ 1 in 40,500 spins
```

---

## 4. Paytable Design

Coefficients are derived iteratively targeting **93% base RTP**.

| Match | Coefficient | Combinations | RTP Contribution |
|-------|-------------|-------------|-----------------|
| 2 of 5 | 2× | 6,942,600 | 57.14% |
| 3 of 5 | 25× | 252,300 | 26.00% |
| 4 of 5 | 420× | 4,350 | 7.52% |
| 5 of 5 | 20,000× | 30 | 2.47% |
| **Total** | | | **~93.13%** |

**Validated base RTP (10M simulation): 92.93%** — deviation 0.07% ✅

---

## 5. Progressive Jackpot Architecture

### 5.1 Design Philosophy

The jackpot pool is funded **exclusively by losing lines** — paylines where 1-of-5 matching symbols occur (~70.37% of all lines). This is an original organic accumulation model:

- The player perceives a losing line as "nothing happened"
- In reality, each losing line contributes 4.263% of the line bet to the jackpot pool
- No deduction is made from winning lines or from a global bet tax

### 5.2 Contribution Rate Derivation

```
Target jackpot RTP = 3.00%
P(losing line) = P(1-match) = 70.3733%

Contribution rate = Target_RTP / P(losing_line)
                 = 3.00% / 70.3733%
                 = 4.2630% per losing line per bet unit
```

**Expected pool accumulation:**
```
Avg losing lines per spin = 14.07 (validated: 14.07 in simulation ✅)
Accumulation per spin     = 14.07 × 4.263% = 0.5998 credits/spin (bet=1)
```

**At trigger threshold 1:5,000:**
```
Expected pool = 5,000 × 0.5998 ≈ 2,999 credits
Simulated avg = 2,881 credits ✅ (within expected range)
```

### 5.3 Trigger Mechanism

```python
if random.random() < JACKPOT_TRIGGER_PROB and jackpot_pool > 0:
    payout = jackpot_pool
    jackpot_pool = 0
    total_win += payout
```

- Evaluated once per spin
- Independent of game outcome
- Requires pool > 0 (prevents zero-payout triggers)
- Trigger probability: **1/5,000 per spin**

### 5.4 Validated Jackpot Statistics (10M spins)

| Metric | Theoretical | Actual | Status |
|--------|-------------|--------|--------|
| Jackpot RTP | 3.000% | 2.993% | ✅ |
| Avg spins between hits | 5,000 | 4,812 | ✅ |
| Avg payout | ~2,999 | 2,881 | ✅ |
| Max payout | — | 26,029 | — |
| Jackpot hits | 2,000 (exp.) | 2,078 | ✅ |

---

## 6. Total RTP

| Component | RTP |
|-----------|-----|
| Base game | 92.93% |
| Progressive jackpot | 2.99% |
| **Total** | **95.92%** |
| **Casino margin** | **4.08%** |

---

## 7. Hit Rate Analysis

| Metric | Value |
|--------|-------|
| Overall hit rate | **96.81%** |
| Avg winning lines/spin | 5.93 / 20 |
| Line hit rate | 29.63% |
| Avg losing lines/spin | 14.07 / 20 |
| Zero-win spins | **3.19%** |
| Volatility index (σ/μ) | **6.56** |

**Note:** Zero-win spins of 3.19% means 96.81% of spins return something — excellent engagement profile for a 93% base RTP model.

---

## 8. Bayesian Validation — 5-of-5 Frequency

Using Beta posterior with uniform prior:

```
Observed hits:     237 in 10,000,000 spins
Posterior mean:    0.002380%  (1 in 38,760)
Theoretical:       0.002469%  (1 in 40,500)
95% CI:            [1 in 37,151 — 1 in 47,911]
Result:            ✅ Theoretical within 95% credible interval
```

---

## 9. Survival Analysis

**Setup:** 1,000 independent sessions, 10,000 initial credits, 20 credits/spin (1 × 20 lines)

| Spins | Survival | Bust | Avg Balance | Avg Survivor | Avg Bust At |
|-------|----------|------|-------------|--------------|-------------|
| 500 | 100.00% | 0.00% | 9,671 | 9,671 | — |
| 1,000 | 100.00% | 0.00% | 9,373 | 9,373 | — |
| 2,000 | 100.00% | 0.00% | 8,574 | 8,574 | — |
| 5,000 | 76.20% | 23.80% | 6,214 | 8,151 | 4,147 |
| 10,000 | 31.50% | 68.50% | 4,301 | 13,624 | 6,196 |
| 25,000 | 11.10% | 88.90% | 2,159 | 19,341 | 8,300 |

**Interpretation:**

The 100% survival through 2,000 spins confirms the 93% base RTP creates a stable short-session experience. The progressive jackpot creates the bimodal distribution visible at 10,000+ spins: most players lose gradually while jackpot winners (surviving at 25k) average 19,341 credits — **+93% return** on initial capital.

---

## 10. Frontend Demo Architecture

### 10.1 Technology
- Pure HTML5 / CSS3 / Vanilla JavaScript
- Web Audio API (fully synthesized, no external files)
- Zero build step — open directly in browser

### 10.2 Game Loop

```
Player presses SPIN
  → Stop any playing melody (AudioContext close/recreate)
  → Deduct bet from credits
  → Generate random 5×4 matrix
  → Animate reels (staggered 140ms left to right)
  → Evaluate all active paylines
  → Feed jackpot pool from losing lines (1-match)
  → Check jackpot trigger (independent 1:5,000 random check)
  → Calculate total win
  → 400ms dramatic pause
  → Display priority-filtered win lines
  → Play appropriate sound
  → Update credits and HUD
  → Await hold duration (varies by win tier)
  → Clear win lines at START of next spin
```

### 10.3 Win Line Priority System

```javascript
const displayLines = lines5.length ? lines5
                   : lines4.length ? lines4
                   : lines3.length ? lines3
                   : lines2;
```

Only the most significant lines are shown — eliminating visual noise when multiple tiers win simultaneously.

### 10.4 Audio Hierarchy

| Event | Sound | Duration |
|-------|-------|----------|
| Reel stop | Percussive tone | 0.1s |
| 2-match win | Brief chord | 0.2s |
| 3-match (spooky symbol) | Eerie whisper (3 variants) | 0.6s |
| 3-match (normal) | Mid chord | 0.2s |
| 4-match | Big cascading tones | 0.4s |
| 5-match | Triumphant melody | 30s |
| Jackpot trigger | Full orchestral theme | 210s |

---

## 11. Design Decisions & Trade-offs

| Decision | Rationale |
|----------|-----------|
| Jackpot from losing lines only | Organic accumulation — player perceives nothing lost, pool grows naturally |
| 93% base RTP (not 90%) | Significantly better survival — 100% survive 2,000 spins vs bust at ~1,000 |
| 420× for 4-of-5 | Balances volatility — meaningful hit without distorting base RTP |
| 20,000× for 5-of-5 | Rare enough to be dramatic (1 in 40,500 spins), large enough to be memorable |
| 1:5,000 jackpot trigger | Sweet spot — avg 2,881 credit jackpot, hits realistically within player sessions |
| Uniform symbol distribution | Correct for mathematical modeling; weighted strips are production enhancement |
| Client-side RNG | Demo only — production requires server-side certified PRNG |

---

## 12. Production Considerations

| Area | Demo | Production |
|------|------|------------|
| RNG | `random.random()` / `Math.random()` | Certified PRNG (GLI/eCOGRA audited) |
| Result authority | Client browser | Server-authoritative |
| Reel strips | Uniform | Weighted per symbol per reel |
| Jackpot pool | Session-only | Persistent database |
| Responsible gambling | None | Loss limits, self-exclusion, session timers |
| Compliance | None | RGL / Malta Gaming Authority certification |

---

*Document prepared as part of portfolio demonstration. All mathematics independently derived and validated through 10,000,000 Monte Carlo iterations.*
