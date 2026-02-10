模板的拓展——以CAPM模型的思路为例

本文主要思考一个问题：如何将一个Alpha模板拓展到更多的数据集。首先，我们以一个基于传统资本资产定价模型（CAPM）的模板为例。在CAPM框架中，它通过时间序列线性回归消除市场因素引起的回报📉。

用快速表达式表示，该模板会有类似如下的表达：

ts_regression(returns, group_mean(returns, ts_mean(cap, 21), 252, rettype=0))

这个表达式返回时间序列中可能无法用市场平均值解释的回报部分。单独使用这个表达式往往会创造出类似于经典回归信号的Alpha。然而，你可以将“寻找由组平均值无法解释的值”的想法扩展到各种数据。让我们简化重写：

data = winsorize(ts_backfill(<data>, 63), std=4.0);
data_gpm = group_mean(data, log(ts_mean(cap, 21)), sector);
resid = ts_regression(data, data_gpm, 252, rettype=0);
resid

这个模板保留了原始表达式的抽象思想，但可以扩展到任何数据类型，并进行了一些值得注意的调整：

第一行的基本数据预处理，包括回填和修剪极端值✂️。

在第二行计算组平均值时，使用行业而不是市场，并选择log(ts_mean(cap, 21))进行加权，以防止大公司扭曲组平均值，同时也平滑了cap📊。

第三行的主要回归找到可能无法由data_gpm解释的部分🔍。