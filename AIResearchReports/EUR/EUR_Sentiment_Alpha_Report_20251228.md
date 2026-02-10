# EUR Sentiment Alpha Research Report

**Date:** 2025-12-28
**Author:** AI Assistant (WorldQuant BRAIN)
**Region:** EUR
**Universe:** TOP2500
**Dataset:** `sentiment21` (AI Sentiment Score Data)

---

## 1. Executive Summary

This report details the development of a high-performance, low-correlation Alpha factor for the European equity market. The final Alpha, `9qoL18wo`, achieves a **Sharpe Ratio of 1.69** and a **Production Correlation of 0.48**, successfully overcoming the high correlation barrier common in sentiment strategies.

The winning strategy utilizes a **"Confidence-Weighted Robust Trend"** logic. It calculates the long-term sentiment trend using robust statistics (Median) to filter out noise, and normalizes this trend by the width of the sentiment confidence interval (Uncertainty). This structural innovation significantly decorrelates the signal from standard mean-based sentiment alphas.

### Key Performance Metrics (Alpha `9qoL18wo`)

| Metric | Value | Constraint | Status |
| :--- | :--- | :--- | :--- |
| **Sharpe Ratio** | **1.69** | > 1.58 | ✅ PASS |
| **Fitness** | **1.24** | > 1.0 | ✅ PASS |
| **Turnover** | **12.27%** | 1% - 70% | ✅ PASS |
| **Prod Correlation** | **0.48** | < 0.70 | ✅ PASS |
| **Self Correlation** | **0.13** | < 0.70 | ✅ PASS |
| **Margin** | 10.9 bps | > 10 bps | ✅ PASS |

---

## 2. Methodology & Logic

### 2.1 The "Correlation Wall" Challenge
Early iterations (Batches 1-18) using standard logic (`Mean_Pos - k * Mean_Neg - Volatility`) achieved high Sharpe (> 1.70) but consistently failed the correlation check (Corr ~ 0.74). This indicated that the "Mean-Variance" sentiment signal is saturated in the production pool.

### 2.2 The Breakthrough: Robustness & Ratio
To break the correlation, we shifted the Alpha's statistical basis and functional form:

1.  **Robust Statistics (Median vs. Mean):**
    *   Replaced `ts_mean` with `ts_mean(median_field)`.
    *   **Why:** Median is robust to outliers (extreme news events) which often drive high correlation across different alphas during market shocks.
    *   **Fields:** `snt21_2pos_median_99`, `snt21_2neg_median_105`.

2.  **Confidence Interval as Uncertainty:**
    *   Replaced `Standard Deviation` with **Confidence Interval Width** (`Conf_Up - Conf_Low`).
    *   **Why:** Captures the model's uncertainty about the sentiment score itself, rather than just the volatility of the score over time.
    *   **Fields:** `snt21_2neg_conf_up_98`, `snt21_2neg_conf_low_92`.

3.  **Ratio Structure:**
    *   Changed from `Trend - Uncertainty` to `Trend / Uncertainty`.
    *   **Why:** This non-linear transformation alters the signal distribution, suppressing low-confidence signals more aggressively than linear subtraction.

### 2.3 Final Formula
```python
(
    ts_mean(snt21_2pos_median_99, 120) 
    - 2.4 * ts_mean(snt21_2neg_median_105, 120)
) 
/ 
(
    ts_mean(snt21_2neg_conf_up_98 - snt21_2neg_conf_low_92, 60) 
    + 0.001
)
```

**Configuration:**
*   **Decay:** 2
*   **Delay:** 1
*   **Neutralization:** SUBINDUSTRY
*   **Truncation:** 0.01
*   **Pasteurization:** ON

---

## 3. Iteration History (Selected Highlights)

| Batch | Alpha ID | Logic | Sharpe | Corr | Outcome |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **B19** | `om2Mpdxv` | Inner Subindustry Neut | 1.69 | 0.74 | Fail (High Corr) |
| **B19** | `E53X7d9r` | Double Neut (Country) | 1.68 | 0.74 | Fail (High Corr) |
| **B20** | `58Ro0Rmz` | Long Window (180d) | 1.71 | 0.74 | Fail (High Corr) |
| **B20** | `YPZGKZVl` | Median Trend - Vol | 1.76 | 0.74 | Fail (High Corr) |
| **B21** | `N1MvmAQq` | Linear Decay | 0.39 | - | Fail (Low Sharpe) |
| **B22** | `zqa16YwG` | Median + 180d + Trunc0.05 | 1.79 | 0.75 | Fail (Higher Corr) |
| **B23** | `9qoL18wo` | **Median Trend / CI Width** | **1.69** | **0.48** | **WINNER** |

---

## 4. Conclusion

The research confirms that while the European sentiment space is crowded, significant alpha opportunity remains by leveraging **robust statistics** and **structural modifications**. Specifically, moving away from simple linear combinations of mean values allows for the extraction of a cleaner, uncorrelated signal that maintains high predictive power. Alpha `9qoL18wo` is ready for production submission.
