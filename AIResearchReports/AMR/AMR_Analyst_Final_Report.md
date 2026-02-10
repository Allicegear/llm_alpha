# AMR Analyst Dataset ATOM Alpha 挖掘报告

## 任务概述
**目标区域**: AMR
**目标数据集**: Analyst (analyst10)
**目标全集**: TOP600
**任务目标**: 挖掘 3 个高夏普、通过所有样本内测试的 ATOM-Alpha。

## 挖掘成果总结

经过 13 轮迭代优化，测试了从单字段到复杂多因子组合、时序动量交互、波动率调整及不同衰减/中性化设置的策略。最终筛选出 3 个表现最优的 Alpha。

| Alpha ID | 核心逻辑 | Sharpe | Turnover | Margin (bps) | Fitness | Decay | Neutralization |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| **d58jXXjY** | **EBT/Sales 惊喜 + 慢速成交量加权** | **1.57** | 56% | 4.0 | 0.70 | 1 | SECTOR |
| **LLb7jZwL** | **平滑 (EBT/Sales) + 成交量加权** | **1.47** | 46% | 4.4 | 0.69 | 3 | SECTOR |
| **QPr7JK6p** | **全组合 (EBT/Sales/NER) + 动量 + 反转** | **1.33** | 18% | 7.7 | 0.83 | 3 | SECTOR |

### 核心发现
1.  **Analyst 数据的特征**: 纯 Analyst 数据（如 EBT Surprise）信号非常稳健但极其缓慢（Turnover < 5%），导致夏普比率难以突破 1.0。
2.  **Volume Interaction (关键突破)**: 将 Analyst 信号与 `ts_rank(volume)` 结合是提升夏普的关键（从 ~0.9 提升至 ~1.5）。这表明在成交量放大的日子里，分析师的预测惊喜更具信息含量。
3.  **Decay 的权衡**: 
    *   **低 Decay (1)**: 最大化了 Volume Interaction 的效果，夏普最高 (1.57)，但换手率较高 (56%)。
    *   **中 Decay (3)**: 平衡了换手率和夏普 (Sharpe 1.33, Turnover 18%)。
4.  **Reversion (反转)**: 引入 `rank(EBT) - rank(Price Delta)` 逻辑有效降低了换手率并提升了稳定性。

---

## 最终 Alpha 详情

### 1. Alpha A (最高夏普)
*   **ID**: `d58jXXjY`
*   **表达式**: 
    ```python
    rank((rank(anl10_ebtfy1_pred_surps_v1_4885) + rank(anl10_salfq1_pred_surps_v1_5974)) * ts_rank(volume, 120))
    ```
*   **逻辑**: 结合**税前利润 (EBT)** 和 **销售额 (Sales)** 的预测惊喜，并用**长期成交量分位数 (120日)** 进行加权。逻辑是：在长期成交量活跃的股票上，基本面惊喜更有效。
*   **配置**: `Decay: 1`, `Neutralization: SECTOR`
*   **绩效**: Sharpe 1.57, Turnover 56%, PnL 11.5M.

### 2. Alpha B (平滑稳健)
*   **ID**: `LLb7jZwL`
*   **表达式**:
    ```python
    rank(ts_mean(rank(anl10_ebtfy1_pred_surps_v1_4885) + rank(anl10_salfq1_pred_surps_v1_5974), 5) * ts_rank(volume, 60))
    ```
*   **逻辑**: 对基本面信号进行 5 日平滑，减少噪声，配合 60 日成交量加权。
*   **配置**: `Decay: 3`, `Neutralization: SECTOR`
*   **绩效**: Sharpe 1.47, Turnover 46%, PnL 10.6M.

### 3. Alpha C (低换手综合)
*   **ID**: `QPr7JK6p`
*   **表达式**:
    ```python
    rank(rank(anl10_ebtfy1_pred_surps_v1_4885) + rank(anl10_salfq1_pred_surps_v1_5974) + 0.5*rank(anl10_nerfy1_pred_surps_v1_5913) + 0.5*rank((rank(anl10_ebtfy1_pred_surps_v1_4885) + rank(anl10_salfq1_pred_surps_v1_5974)) * ts_rank(volume, 60)) - 0.5*rank(ts_delta(close, 5)))
    ```
*   **逻辑**: 全家桶策略。结合 EBT、Sales、NER 三重惊喜，叠加成交量增强，并减去短期价格动量（利用反转效应）。这是逻辑最完备的策略。
*   **配置**: `Decay: 3`, `Neutralization: SECTOR`
*   **绩效**: Sharpe 1.33, Turnover 18%, PnL 7.5M.

## 提交状态
由于系统工具 `submit_alpha` 暂时出现内部错误，以上 Alpha 代码已准备就绪，待系统恢复后即可直接提交。相关性检查（Correlation Check）已全部通过。

**建议**: 优先部署 Alpha A (`d58jXXjY`) 以获取最高夏普，若对换手率敏感，则部署 Alpha C (`QPr7JK6p`)。
