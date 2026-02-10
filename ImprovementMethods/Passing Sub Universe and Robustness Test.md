Robustness Test

The IS performance of an Alpha isn’t the ultimate goal when researching an idea for an Alpha. The true goal is to create a robust Alpha that can still retain performance under different scenarios. To actually test if your Alpha hypothesis is true, a strong IS performance when backtesting on the BRAIN platform isn’t enough. So you should incorporate your own robustness test into your Alpha Creation Engine (ACE).

Super/Sub universe test:
 

You can conduct the super/sub universe test on your own by using a smaller/bigger universe in the simulation setting. Though the result may differ from the result message in the IS testing status of your original Alpha, you can define the performance threshold as:

 

For sub universe test:

targetUnitPerformance = ratio * SizeTargetUni / SizeOriginalUni * originalUnitPerformance


Here, you can aim for the performance to retain 70%. For the original Alpha without any subuniverse, you can skip this test (be aware that the ILLIQUID universe is completely different from the smaller universe, so their performance can’t be compared)

For super universe test:

targetUnitPerformance = ratio * originalUnitPerformance

Here, you can also aim for the performance to retain 70%. For the original Alpha without any super universe, you can skip this test.

 

Self OS test:
Another way to assess how robust your Alpha is, is by recreating a self OS test, where you only research Alpha with a part of the backtest period as your IS, and reserve the other part as your self OS. You can choose any period you want, but a rule of thumb is the self OS period should be around 2-4 years to ensure that the IS period is long enough so that its performance is actually meaningful. After creating a submittable Alpha on the IS period, you can assess the robustness on the self OS period. Some metrics that you can use are:

 

You can create a self OS/IS period by doing so using the ace library. Here is a pseudo-code snippet:

        pnl = get_alpha_pnl(s, alpha_id)
        tvr = get_alpha_turnover(s, alpha_id)
        is_cutoff = ‘2021-01-01’
        self_is_pnl, self_os_pnl =  pnl.loc[df.index < is_ cutoff], pnl.loc[df.index >= is_ cutoff]
        self_is_tvr, self_os_tvr = tvr.loc[df.index < is_ cutoff], tvr.loc[df.index >= is_ cutoff]
From the above self IS/OS period, you can calculate your alpha Sharpe, Turnover, and Return. If you create your alpha optimization algorithm, try to only optimize it on the self IS period, and validate the optimized alpha performance on the self OS period. Some more robustness tests you can use to validate your alpha performance are:

 

OS sharpe over IS sharpe ratio:
You can define your own performance threshold as:

OSSharpe/ISSharpe > ratio

Here, you can aim for the performance to retain around 70% when comparing between the OS and IS period.

 

Turnover ratio:
A sudden turnover jump during the backtest is also undesirable:

OSSharpe/ISSharpe <= ratio

Here, you can aim for the turnover changes to be less than one when comparing between the OS and IS period.

 

Distribution test:
Rank test
To ensure that your alpha doesn’t favor some stocks, you can resimulate the Alpha but with the rank operation at the end of the Alpha, in order to redistribute the Alpha weight uniformly. And check how much the performance drops:

RankSharpe/OriginalSharpe > 0.5

You can aim to have an Alpha that retains at least 50% of its performance after the rank operation.

 

Sign test
Another test to check the your Alpha performance without the original Alpha weight distribution is by applying the sign operation at the end of the Alpha, and assessing how good the Alpha is at predicting the instrument price direction.

SignSharpe/OriginalSharpe > 0.4

You can aim to have an Alpha that retains at least 40% of its performance after the sign operation.