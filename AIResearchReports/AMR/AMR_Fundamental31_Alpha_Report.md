# AMR Fundamental31 Alpha Research Report

**Date**: 2026-02-12
**Author**: WorldQuant AI Researcher (YW80612)
**Region**: AMR
**Universe**: TOP600
**Dataset**: fundamental31 (Fundamental Models)

## 1. 摘要 Executive Summary

本次研究旨在AMR地区（TOP600）基于`fundamental31`单一数据集挖掘高Sharpe Alpha。经过22轮迭代优化，测试了数十种因子组合与数学变换，最终产出的最佳Alpha（ID: `A1JVZpaQ`）实现了 **Sharpe 1.14**，通过了 **Concentrated Weight** 和 **Robust Sharpe** 等关键稳健性测试，但在Sharpe绝对值（目标1.58）和 Margin/Turnover 比率上未能完全达到硬性提交标准。

## 2. 核心逻辑 Methodology

### 2.1 核心驱动因子 (Core Driver)
研究发现 `fnd31_devnorthamericaadditionalfactor4_capexdeplink` (Capital Expenditures to Depreciation Linkage) 是该数据集在AMR地区最强的基础信号。
- **关键算子**: `ts_mean(x, 10) / ts_std_dev(x, 10)`。
- **原理**: 该算子衡量了信号的稳定性（信噪比）。相比直接使用 `rank` 或 `zscore`，基于信噪比加权的 Capex Linkage 因子表现出极强的预测能力（单因子 Sharpe 0.81）。

### 2.2 增强因子 (Enhancements)
为了提升表现，引入了多维度因子进行线性组合：
1.  **质量 (Quality)**: `ts_decay_linear(earnshortfall, 10)`。做空 Earnings Shortfall（盈余缺口），作为负向指标扣减。
2.  **成长 (Growth)**: `ts_delta(apgghcy1, 22)`。1-Year Change in Gross Profit to Assets。作为正向指标叠加。
3.  **债务 (Debt)**: `ts_delta(chg12mtotdebt, 22)`。12-Month Change in Total Debt。债务加速增长往往预示风险，作为负向指标扣减。
4.  **反转/预期 (Reversion/Expectation)**: `ts_rank(earningstorpedo, 22)`。Earnings Torpedo (Estimate + Surprise / EPS)。高预期/惊喜往往伴随反转风险或过度拥挤，作为负向指标（权重5倍）扣减显著提升了夏普。

### 2.3 构造方式 (Construction)
采用 `Neutralization: NONE` 并在表达式末尾使用 `rank(...) - 0.5` 手动去中心化，构建 Long/Short 组合。这种方式比 `group_neutralize` (Subindustry/Sector) 效果更好，保留了更多截面Alpha信息。

## 3. 最终策略 Final Alpha

**Alpha ID**: `A1JVZpaQ`

**Expression**:
```python
rank(
    (
        ts_mean(fnd31_devnorthamericaadditionalfactor4_capexdeplink, 10) / ts_std_dev(fnd31_devnorthamericaadditionalfactor4_capexdeplink, 10) 
        - ts_decay_linear(fnd31_devnorthamericaadditionalfactor4_earnshortfall, 10) 
        + ts_delta(fnd31_devnorthamericaadditionalfactor4_apgghcy1, 22) 
        - ts_delta(fnd31_devnorthamericaadditionalfactor4_chg12mtotdebt, 22)
    ) 
    - 5 * ts_rank(fnd31_devnorthamericaadditionalfactor4_earningstorpedo, 22)
) - 0.5
```

**Configuration**:
- **Region**: AMR
- **Universe**: TOP600
- **Delay**: 1
- **Decay**: 0
- **Truncation**: 0.01
- **Neutralization**: NONE
- **Pasteurization**: ON

## 4. 绩效表现 Performance Metrics

| Metric | Value | Status |
| :--- | :--- | :--- |
| **Sharpe** | **1.14** | Fail (< 1.58) |
| **Fitness** | 0.72 | Fail (< 1.0) |
| **Turnover** | 19.55% | Pass |
| **Margin** | 8.05 bps | Fail (Ratio < 1.2) |
| **Concentrated Weight** | PASS | Pass |
| **Sub Universe Sharpe** | 1.16 | **Pass (Excellent)** |
| **Correlation** | < 0.4 | Pass |

## 5. 结论与反思 Conclusion

1.  **上限瓶颈**: 在仅允许使用 `fundamental31` 单一数据集且限定 AMR TOP600 的条件下，Sharpe 1.14 已接近该策略逻辑的极限。
2.  **稳健性**: 该 Alpha 在 Sub Universe 测试中表现优异（Sharpe 1.16），说明其逻辑在不同子样本中高度一致，并未过拟合。
3.  **改进方向**: 
    - 若允许跨数据集，引入 Price Volume 类因子（如波动率、动量）进行正交化或过滤，有望突破 1.58。
    - 增加 Turnover 限制（如 `decay` 设置）虽然能降低换手，但会显著损耗 Sharpe（测试显示 `decay=5` 时 Sharpe 降至 < 0.8），因此当前版本保留了较高的换手以维持预测力。

**建议**: 可将此 Alpha 作为 `AMR` 地区 Fundamental 策略的基础组件（Combo Component），与其他低相关 Alpha 组合使用。
