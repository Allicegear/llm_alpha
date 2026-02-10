一、除法（比率）是最简单的模版
1.1 如何理解论坛推荐的数据字段
论坛推荐数据合集链接：support.worldquantbrain.com
可以看到论坛中老师已经初步加工了几万条推荐数据，这些数据比率是通过暴力组合后输入给LLM模型得出的有经济学意义的比率，是更有可能产生信号的。因此主要集中在基本面相关的数据。以下面一个我已经提交的alpha为例：
 
Settings: ASI | MINVOL1M | Subindustry
group_neutralize(ts_scale(fnd17_qebitd / fnd23_swds, 240),densify(sta1_minvol1mc50))
 
可以看到整个alpha里唯一的主信号来自于fnd17_qebitd / fnd23_swds这两个比率，从数据描述中可以看到 fnd17_qebitd是最近一个季度的EBIT（息税前利润）而fnd23_swds是Diluted EPS的加权平均股数。作为没有财务基础的我，只能通过kimi来解释两个指标相除是否有意义？
 
来自Kimi：EBIT（息税前利润）除以Diluted Weighted Average Shares（稀释后的加权平均股数）这个比率更接近于计算Diluted EPS（稀释每股收益）
 
那正好是我上一篇文章的EPS的近似指标，这也不难解释为什么这个指标可以撑起来一个alpha了。到这里就引发了我一个思考，BRAIN平台上有那么多的EBIT的近似数据，也有那么多的股数的数据，基于这个思路是不是可以找到更多的alpha了？ 这部分留到下一篇文章再去展开。
 
1.2 使用数据字段比率的心得体会
数据字段比率的分子和分母数据预处理是否必要? 个人经验上似乎预处理后经常反而会降低表现，这里也请老师指正是否正确，还是试验的误差导致
信号使用
Sharp
Fitness
fnd17_qebitd / sharesout
1.89
1.06
ratio = fnd17_qebitd / sharesout;
winsorize(ts_backfill(ratio,60)
1.63
0.84
a = winsorize(ts_backfill(fnd17_qebitd,60),std=4);
b = winsorize(ts_backfill(sharesout,60),std=4) ;a/b
1.66
0.87
 
这里的思考是对于基本面数据本来更新频率就是相近的季频，使用backfill填充以后会某种程度上稀释了原有的比率信号，不知道是否正确？欢迎大家留言讨论
不能直接跑比率而不进行一阶和二阶的嵌套
  开始图省事，先对所有的比率进行不嵌套的跑，作为"零阶“，有一定信号才放到一阶里。这样确实可以找到一些，也会提升很大的效率。但确实是会浪费很多可以加了嵌套后有alpha的比率。（比如以上的例子里直接跑就是噪音）
跑二阶之前，筛掉那些fitness看起来奇高的，通常都是因为异常数据而导致的，pnl都是一条折线。我筛选的阈值是fitness要小于3
由于我们在回测时只能设定一种中性化方式，所以不完全依赖check submission的结果
即使不通过，我会找出来一些表现看起来差不多的，进行手工微调（后来演化到了暴力遍历）经常会有好结果。这些已经就差一点了扔掉就太可惜了。在这个反复手工调整的过程中，对于operator的使用也有更多自己的体会
 
1.2 工程如何实现
最粗暴的办法是把整个比率赋值直接贴给pc_fields 字段，然后生成一阶代码。但这种实现方式实在不是我等程序员的作风，够用但不推荐
pc_fields = ['a/b','a/b','a/b','a/b'... ] 
first_order = get_first_order(pc_fields, ts_ops)
我目前是把代码粘贴到一个expression.txt的文本文档中，然后读取给expression_list，再放到get_first_order中
file_path = ‘’ 
expreission_list = read_expression_facoory(file_path,1) 
print(len(expreission_list)) 
first_order = get_first_order(expreission_list, ts_ops)
为了让这个可以和之前的单数据集代码是一套代码，我定义了任务类型来切换是跑单数据集还是论坛比率
if task_type == "forum": 
elif task_type == "single": 
elif task_type == "generator":
如何筛选fitness小于3的alpha？
filtered_list = [item for item in selected_alphas if item[4] < 3]