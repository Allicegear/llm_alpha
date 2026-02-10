# 如何提高 Sub-universe Sharpe

在 WorldQuant BRAIN 平台上，Sub-universe Sharpe 是指 Alpha 在特定子宇宙（如行业、板块、市值分组等）中的夏普比率。如果在某些 Sub-universe 中表现不佳，会拖累整体 Alpha 的评分和夏普比率。

以下是一些提高 Sub-universe Sharpe 的常用方法：

## 1. 行业/板块中性化 (Neutralization)

最直接的方法是消除对特定行业或板块的过度暴露。如果 Alpha 在某个行业特别强势（或弱势），对其进行中性化可以平滑收益。

*   **Group Neutralize**: 使用 `group_neutralize` 算子。
    *   `group_neutralize(alpha, sector)`: 消除板块（Sector）层面的暴露。
    *   `group_neutralize(alpha, industry)`: 消除行业（Industry）层面的暴露（更细粒度）。
    *   `group_neutralize(alpha, subindustry)`: 消除子行业（Subindustry）层面的暴露（最细粒度）。
    *   `group_neutralize(alpha, market)`: 消除市场整体暴露（Market Neutral）。

*   **原理**: 确保在每个组内（例如每个行业内），Alpha 的均值为 0。这迫使 Alpha 在组内寻找相对优胜者，而不是押注该组整体上涨或下跌。

## 2. 组内排序 (Group Ranking)

类似于中性化，但更进一步，确保信号在组内均匀分布。

*   **Group Rank**: 使用 `group_rank` 算子。
    *   `group_rank(alpha, sector)`: 将 Alpha 值在每个板块内进行排序（0 到 1）。
    *   这也隐含了中性化的一面，因为排序后的均值接近 0.5，减去 0.5 后即为中性。

*   **Group Z-Score**: 使用 `group_zscore`。
    *   `group_zscore(alpha, sector)`: 将 Alpha 值在板块内标准化（减去均值，除以标准差）。
    *   这有助于解决某些板块波动率大、某些板块波动率小的问题，使各板块对总 Alpha 的贡献更均衡。

## 3. 异常值处理 (Outlier Handling)

如果某个 Sub-universe 的数据有很多异常值（outliers），可能会导致该组 Sharpe 极低。

*   **Trim/Winsorize**: 去除或限制极端值。
    *   使用 `rank(alpha)` 将所有信号映射到均匀分布，避免极端值影响。
    *   或者使用 `max(min(alpha, upper_bound), lower_bound)`。

*   **Power Decay / Rank Decay**:
    *   `power(rank(alpha), 2.0)`: 对排序后的信号加强两端权重，同时保持平滑。

## 4. 条件过滤 (Conditional Logic)

如果 Alpha 在某些特定类型的股票（例如小市值、流动性差）中表现持续糟糕，可以考虑过滤掉这些股票，或者降低权重。

*   **基于流动性的过滤**:
    *   `if(rank(volume) > 0.1, alpha, 0)`: 只在流动性较好的股票中交易。
    *   或者使用 `adv20` (20日平均成交额) 进行加权或过滤。

*   **避免硬编码**:
    *   尽量避免 `if(sector != 10, alpha, 0)` 这种硬编码过滤，因为这容易过拟合（Overfitting）。建议寻找导致该 sector 表现差的根本原因（Fundamentals），例如“高负债”或“低周转率”，然后基于该因子进行过滤。

## 5. 组合与分散 (Diversification)

*   **多因子组合**:
    *   如果 Alpha A 在科技股好，Alpha B 在能源股好，将它们组合 `Alpha A + Alpha B` 可以平滑 Sub-universe Sharpe。

## 6. 检查数据质量

*   某些数据字段（Datafields）在特定行业（如金融、公用事业）可能定义不同或缺失。
    *   使用 `is_finite(field)` 检查数据有效性。
    *   对于缺失值，使用 `fillna` 或 `if(valid, value, 0)` 进行处理，避免产生无意义的信号。

## 总结建议

1.  **首选 Group Neutralize/Rank**: 这是最有效且最符合逻辑的方法，能确保 Alpha 在通过各个 Sub-universe 检验时稳健。
2.  **标准化**: 确保信号在不同组间可比 (`group_zscore`)。
3.  **分析原因**: 观察 Simulation 结果中的 "Sub-universe" 标签页，找出表现差的具体组别，分析是方向性错误（Correlation < 0）还是波动率过大（High Volatility），再对症下药。
