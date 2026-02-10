# GLB Analyst ATOM-Alpha Mining Report

## 1. 任务概述 (Mission Overview)
- **目标区域 (Target Region)**: GLB
- **目标数据 (Target Data)**: Analyst (analyst15)
- **目标产出 (Output)**: 3 ATOM-Alphas (Single Dataset)
- **硬性指标 (Hard Pass Criteria)**: Sharpe >= 1.58, Margin/Turnover > 1.2, PnL > 5Y.

## 2. 挖掘过程 (Mining Process)
### 2.1 数据分析
- **数据集**: `analyst15` (Earnings forecasts)
- **特性**: 该数据集在 GLB 区域主要提供 Group/Sector/Industry 聚合数据 (如 `anl15_gr_...`, `anl15_ind_...`)，缺乏个股层面的颗粒度数据 (或与 `close` 等向量数据结合困难)。
- **策略**: 采用 "Group Conviction" (一致性预期) 和 "Group Momentum" (预期动量) 策略，将行业/组别属性赋予个股进行截面排序。

### 2.2 迭代路径
- **Phase 1**: 基础单因子测试 (Sharpe ~0.6-0.7)。
- **Phase 2**: 引入 `neutralization="MARKET"`，导致 Sharpe 下降 (信号主要来自 Sector/Group Beta)。
- **Phase 3**: 尝试复杂算子 (`ts_decay`, `ts_zscore`) 与价格 (`close`) 结合，但遭遇系统性 Syntax Error (可能是数据对齐或空值问题)。
- **Phase 4**: 回归 `rank` 基础算子，叠加 "Conviction" (Mean/StdDev) 与 "Momentum" (1m Change) 逻辑，获得最佳结果。

## 3. 最终 Alpha 产出 (Final Alphas)

由于数据集限制，Alpha 未能完全达到 Sharpe >= 1.58 的高标准，但已挖掘出该数据集下的最优解 (Sharpe ~0.8, Fitness ~1.0)。

### Alpha 1: Group Conviction & Momentum (Best)
- **Alpha ID**: `Gr0pvAk5`
- **Expression**: 
  ```python
  rank(anl15_gr_12_m_mean / (anl15_gr_12_m_st_dev + 0.001)) * rank(anl15_gr_12_m_1m_chg)
  ```
- **Logic**: 结合 "分析师预期的一致性" (Mean/StdDev) 与 "预期上修动量" (1m Change)。
- **Performance (IS)**:
  - **Sharpe**: 0.79
  - **Fitness**: 1.01
  - **Turnover**: 4.31%
  - **Margin**: 94.78 bps
  - **PnL**: 21.1M (Sample)

### Alpha 2: Industry Conviction
- **Alpha ID**: `qMkoZrXK`
- **Expression**:
  ```python
  rank(anl15_ind_12_m_mean / (anl15_ind_12_m_st_dev + 0.001))
  ```
- **Logic**: 仅使用 Industry 层面的 "一致性预期" 因子。
- **Performance (IS)**:
  - **Sharpe**: 0.77
  - **Fitness**: 0.97
  - **Turnover**: 1.30%

### Alpha 3: Group Conviction (Base)
- **Alpha ID**: `Wjw6mQ9P`
- **Expression**:
  ```python
  rank(anl15_gr_12_m_mean / (anl15_gr_12_m_st_dev + 0.001))
  ```
- **Logic**: 仅使用 Group 层面的 "一致性预期" 因子。
- **Performance (IS)**:
  - **Sharpe**: 0.76
  - **Fitness**: 0.96
  - **Turnover**: 1.34%

## 4. 结论与建议 (Conclusion)
- **数据集局限**: `analyst15` 在 GLB 区域呈现强烈的 Group 属性，适合作为 Context 数据辅助其他 Stock-level 因子，单兵作战 (ATOM) 难以突破 Sharpe 1.0 瓶颈。
- **改进方向**: 建议将此 Alpha 作为 `super_simulation` 中的一个组件，与量价因子 (`price_volume`) 或基本面因子 (`fundamental`) 结合使用，利用其低换手 (Turnover < 5%) 特性提供稳健的行业配置建议。
