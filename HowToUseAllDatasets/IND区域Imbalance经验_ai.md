IND区域Imbalance优化经验:

1. operators:

| 算子 | 作用 | 影响 |
|------|------|------|
| `ts_av_diff(x, n)` | 当前值与n日均值的差 | **大幅提升Sharpe和Fitness** |
| `ts_delta(x, n)` | n日变化量 | 简洁有效，Robust高 |
| `signed_power(x, 0.4)` | 压缩分布，保留符号 | **平滑信号，提升Sharpe** |
| `ts_rank(x, n)` | 时序排名归一化 | **作为权重因子效果佳** |
| `group_neutralize(x, industry)` | 行业中性化 | **去除行业偏差** |

2. 外层包装影响（关键发现！）
`rank()`虽然常用于标准化，但可能**破坏信号的原始分布**，降低Robust Sharpe。当内层信号质量足够好时，不加rank反而更优。

3.常见陷阱与解决方案

| 陷阱 | 表现 | 原因 | 解决方案 |
|------|------|------|---------|
| 过度使用rank() | Robust Sharpe低 | 破坏信号分布 | 尝试去掉rank |
| 窗口太短 | Fitness低，噪音大 | 信号不稳定 | 增加时间窗口 |
| 窗口太长 | Sharpe下降 | 信号滞后 | 找到平衡点 |
| decay参数>0 | 降低所有指标 | 信号衰减过快 | 保持decay=0 |

4. tips
**signed_power 降低生产相关性的关键**
**ts_delta vs ts_av_diff**
 - ts_delta 更简单但与生产alpha相关性更高
 - ts_av_diff 配合 signed_power 能够同时达到指标和相关性要求
 
 
5. template
# 模板1: 基础ts_av_diff模板 
-group_neutralize(signed_power(ts_av_diff({FIELD}, 20), 0.4), industry)

# 模板2: 双字段乘法调制 
-group_neutralize(ts_delta({FIELD}, 20) * (1 - {SCORE_FIELD}), industry)

# 模板3: ts_rank权重因子 (
-group_neutralize(ts_rank({SCORE_FIELD}, 30) * ts_av_diff({FIELD}, 25), industry)

# 模板4: 带rank的稳健版本 
-rank(group_neutralize(signed_power(ts_av_diff({FIELD}, {WINDOW}), 0.4), industry))

