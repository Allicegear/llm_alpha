# ATOM Alpha Mining Report - AMR Region

## 1. 任务概述 (Mission Overview)
- **目标区域**: AMR (Americas)
- **目标数据集**: Fundamental (`other466`)
- **目标宇宙**: TOP600
- **目标**: 挖掘 3 个 ATOM-Alpha (单数据集因子)，追求 Sharpe > 1.58, Margin/Turnover > 1.2.

## 2. 执行过程 (Execution Process)
- **总批次**: 7 批 (Batches)
- **总模拟数**: 56 个 Alpha 表达式
- **策略演进**:
    1.  **Batch 1-2**: 基础财务比率 (RoA, Cash/Assets, Debt/Equity)。结果 Sharpe ~0.5，Turnover 极低 (<2%)。
    2.  **Batch 3-4**: 引入变化率 (`ts_delta`, `ts_rank`) 和行业中性化 (`group_rank(..., subindustry)`)。Sharpe 提升至 ~0.55，但 Turnover 仍低。
    3.  **Batch 5**: 引入价格 (`price_close`)、市值 (`mkt_cap`) 和成交量 (`volume_trade`)。构建估值 (E/P, B/P) 和动量因子。Sharpe 提升至 ~0.60。
    4.  **Batch 6-7**: 复合因子 (Composite) 和流动性加权动量。结合 动量 (Momentum)、价值 (Value)、质量 (Quality) 和 流动性 (Liquidity)。Sharpe 达到峰值 0.66。

## 3. 最佳 Alpha 推荐 (Top 3 Alphas)

虽然未能达到 Sharpe 1.58 的硬性指标 (受限于单数据集和市场有效性)，以下三个因子表现最佳，且逻辑稳健。

### Alpha 1: 流动性加权动量 (Liquidity Weighted Momentum)
- **ID**: `QPr0g1Aw` (Batch 6)
- **表达式**: 
  ```python
  rank(ts_mean(oth466_des_volume_trade / oth466_des_mkt_cap_q, 22) * ts_zscore(oth466_des_price_close, 22))
  ```
- **逻辑**: 在流动性充裕 (Turnover Ratio 高) 的股票中寻找动量效应。高成交量往往验证了价格趋势的有效性。
- **绩效**:
    - Sharpe: 0.66
    - Turnover: 18.28%
    - Fitness: 0.88
    - Margin: 3.5 bps

### Alpha 2: 质量动量复合 (Quality + Momentum)
- **ID**: `wpwoPmxQ` (Batch 7)
- **表达式**: 
  ```python
  rank(rank(oth466_aor_tr) + rank(ts_zscore(oth466_des_price_close, 22)))
  ```
- **逻辑**: 结合 资产回报率 (RoA) 和 价格动量。买入盈利能力强且处于上升趋势的股票。
- **绩效**:
    - Sharpe: 0.66
    - Turnover: 18.10%
    - Fitness: 0.86
    - Margin: 3.4 bps

### Alpha 3: 全面复合因子 (Value + Momentum + Quality)
- **ID**: `mL2a13RW` (Batch 7)
- **表达式**: 
  ```python
  rank(rank(oth466_is_net_inc_basic_q / oth466_des_mkt_cap_q) + rank(ts_zscore(oth466_des_price_close, 22)) + rank(oth466_aor_tr))
  ```
- **逻辑**: 结合 盈利收益率 (Earnings Yield)、价格动量 和 RoA。多维度打分，追求稳健性。
- **绩效**:
    - Sharpe: 0.63
    - Turnover: 18.11%
    - Fitness: 0.80
    - Margin: 3.2 bps

## 4. 失败案例分析 (Failure Analysis)
- **反转因子 (Reversion)**: `ts_delta(price, 5) * -1` 和 `ts_zscore(price, 22) * -1` 均产生负 Sharpe (-0.53, -0.63)。说明在 AMR TOP600 中，月度和周度级别主要是 **动量 (Momentum)** 效应，而非反转。
- **纯基本面 (Pure Fundamental)**: 仅使用资产负债表数据 (如 Cash/Assets) 的 Sharpe 难以突破 0.55，且 Turnover 过低，无法满足 Fitness 要求。

## 5. 结论 (Conclusion)
在限制使用单一 Fundamental 数据集 (`other466`) 的前提下，引入价格和成交量字段是提升绩效的关键。通过构建复合因子，我们成功将 Sharpe 从 0.5 提升至 0.66，Turnover 从 1% 提升至 18%。尽管未达 1.58 的高门槛，但已挖掘出该数据集在 AMR 区域的有效 Alpha 逻辑。
