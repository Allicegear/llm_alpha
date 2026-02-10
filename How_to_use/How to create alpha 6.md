模板的拓展——数据处理的重要性，继续以CAPM模型的思路为例

在前一篇系列文章👉Machine Alpha 基础知识5：模板的拓展——以CAPM模型的思路为例中，我们介绍了如何以CAPM模型的思路拓展模板。本文今天的讨论重点是将焦点转向 Beta 系数及数据处理（Beta 衡量的是股票相对于其群组的波动性，提供了其相较于同行的相对风险的信息）。

承接本系列前文，Brain 表达中的 CAPM Beta：

ts_regression(returns, group_mean(returns, ts_mean(cap, 21), 252, rettype=2))
通过将 rettype 设置为2，您将得到回归方程的斜率。

实现和扩展： 为了进一步应用这个想法，请在模板框架内进行:

数据准备：与任何数据科学实验一样，干净和准确的数据非常重要。首先进行数据预处理步骤，例如：
target_data = winsorize(ts_backfill(<target_data>, 63), std=4.0);
market_data = winsorize(ts_backfill(<market_data>, 63), std=4.0);
计算分组 Beta：这次，不是看残差，而是比较群组之间的斜率/Beta（例如，行业或产业）以得出不同的市场洞见：
target_beta = ts_regression(target_data, group_mean(market_data, log(ts_mean(cap, 21)), sector), 252, rettype=2);
完整的模板形式如下：

target_data = winsorize(ts_backfill(<target_data>, 63), std=4.0);
market_data = winsorize(ts_backfill(<market_data>, 63), std=4.0);
target_beta = ts_regression(target_data, group_mean(market_data, log(ts_mean(cap, 21)), sector), 252, rettype=2);
target_beta