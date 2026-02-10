一、从数据出发，依靠简单模版遍历搜索
 
1.1 以单个数据字段作为主信号
这是对大多数人最简单容易上手的方式，也是参加了线下活动开始学习的方式。这种方式主要需要依赖数据字段本身是否带有足够优秀的信号或者经济学意义。经过简单的标准化运算符加工就可以得到可以提交的alpha。
 
group_normalize(ts_zscore(winsorize(ts_backfill(anl4_afv4_eps_high, 120), std=4), 66),densify(industry))
 
这是一个符合提交条件的alpha ，可以看到整个alpha里唯一的主信号来自于anl4_afv4_eps_high字段，Earnings per share - The highest estimation。当我把这个字段的描述输入给Kimi后，可以得知关于更多EPS的解释，其中
 
EPS的预测和估计对投资者来说非常重要，因为它可以帮助他们评估公司的未来盈利潜力。分析师会根据公司的财务状况、行业趋势、经济环境等因素，对公司的季度或年度EPS进行预测。这些预测通常被称为盈利预测，是投资者决策的重要依据。
 
这也不难解释为什么一个单独的字段就可以撑起来一个alpha，第一是estimation也就是预测，第二是EPS和股价正相关，分析师对于EPS的预测也近似于对股价涨跌的预测。由此可见一个数据字段与股价的走势如果相关性越高则就越容易找到alpha。由此从我的经验总结来看选择单数据集的时候通常有以下标准来判断优先级
是否和股价的走势直接相关
比如Analyst、Model、PV、Fundamental、 Sentiment 这些大类的数据，点进去看看大概的描述。像一些明显看起来和股价没啥关系的就先不跑了，kimi也可以帮助辅助来判断。
是否是新Release的数据集
这里主要是从相关性来考虑的，新release的数据集可能还没有被广泛使用挖掘。
观察一下Coverage和Value Score这些指标
如果coverage过低或者value score过高/过低，我都不会优先去跑
实际感受下来ASI，USA 经常可以比较容易找到，尤其是ASI，可以选择Country或者FAST作为中性化方式。但这只是我的经验，具体原因还请水平更高的同学来补充。比如ASI的fundamental 65就有很多。
遇到不通过的不要直接就扔掉，对一些差一点的可以进一步调整一下。比如增加decay 来降低turnover，从而提高fitness （在turnover高的时候有用）。其他的调优方式在FAQ里和learn文档里都有相应如何调整的方式，比如trade when  、sign_power这些
如何提高fitness: https://support.worldquantbrain.com/hc/en-us/articles/20251386376471-How-to-increase-fitness-of-alphas
如何改善Turnover：https://support.worldquantbrain.com/hc/en-us/articles/20251419309719-How-to-improve-Turnover
IS Ladder Test:https://support.worldquantbrain.com/hc/en-us/articles/20251420634135-How-to-smooth-the-PnL-curve-to-minimize-sudden-fluctuations
After cost illiquid :https://support.worldquantbrain.com/hc/en-us/articles/19083525654551-Error-message-Most-illiquid-50-instruments-after-cost-Sharpe-is-above-cutoff-of-original-universe
Correlation: https://support.worldquantbrain.com/hc/en-us/articles/20251385275671-How-do-you-reduce-correlation-of-a-good-Alpha
其他经验欢迎大家补充，我也会更新这个帖子
1.2 工程如何实现的一些Tips
以上内容可以一个数据集一个数据集的手动来做，也可以系统工程的批量来做，如果批量做可能得几个月都不用管他一直跑。最近工作比较忙，只做了简单的工程，后续有一些优化思路还没有实现。
拉取平台所有数据集的大类、覆盖率、valuescore、描述等信息到本地
借助以上的经验自定义一些阈值、并结合Kimi进行一些初筛，生成一个dataset_id的list，这也是之后的主要任务列表
依据模版对每个数据集跑一阶和二阶，以下是一些技巧和思考
这里有一个我的技巧是如果一阶需要回测过多，我通常会选取2W-3W作为一阶的分割，跑完这些以后就跑二阶，然后看看是否有信号。如果全都没信号，我会倾向于放弃这个数据集。
经常有时候看起来fitness sharp高是因为数据集其实只有最后一年的数或者long count和short count过低，我修改了select alpha的逻辑，要求最低50 long 最低50 short
拆分一阶、二阶、check submission的代码到了三个文件，一阶二阶每个都调整为5个卡槽。这样可以不等到一阶都跑完就可以开始跑二阶，二阶跑的同时就可以check submission了，这里并不需要完全的线性去跑。
按自己的喜好来管理alpha，可以通过name、tag等，这里也需要相应去调整select alpha的代码逻辑。我现在已经解锁了super alpha功能，在做SA时就发现前期的alpha管理也很重要
我创建了一个log，每个任务开始时都会自动记录，完成后除了记录外会给自己发一封邮件提醒，尤其是checksubmission 发邮件可以记录Prod Correlation，会很方便去挑选低相关性的alpha。这样做的好处是避免重复跑一个数据集，跑多了自己肯定记不住
