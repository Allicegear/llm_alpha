1. group_rank(winsorize(ts_backfill(news21_a/news21_b,252),std=4),gr)
- news21_a/news21_b: fast_d1 field in news21 dataset

2. ts_mean(max(vec_avg(same_dataset),vec_avg(same_dataset),vec_avg(same_dataset)),d)
- same_dataset: same dataset
- d: days
- finder the biggest signal

3. -ts_mean(ts_target_tvr_hump(group_rank({data},country), lambda_min=0, lambda_max=1, target_tvr=0.1),5)
- data: dataset
- country: country
- the template worked in EUR region

4. {ts_op1}({ts_op2}({x}, {d1}), {d2})
- 动量加速（差分→差分）经济含义：捕捉趋势的加速或减速 
- d : days 
- ts_op：ts_delta、ts_av_diff、ts_min_diff、ts_min_max_diff  

5. {norm_op}({diff_op}({x}, {d1}), {d2})
- 标准化动量（标准化→差分）经济含义：对动量信号做排序或标准化  
- d : days 
- norm_op：ts_rank、ts_zscore、ts_median、ts_std_dev、ts_skewness  
- diff_op：ts_delta、ts_av_diff、ts_min_diff、ts_min_max_diff  

6. {agg_op}({norm_op}({x}, {d1}), {d2})
- 聚合信号（聚合→标准化）经济含义：对标准化信号做平滑或时间衰减  
- d : days 
- agg_op：ts_sum、ts_decay_linear、ts_decay_exp_window  
- norm_op：ts_rank、ts_zscore、ts_median、ts_std_dev、ts_skewness、ts_entropy  

7. {pos_op}({inner_op}({x}, {d1}), {d2})
- 位置信号（位置→聚合/标准化）经济含义：找出极端值相对于聚合信号出现的位置  
- d : days 
- pos_op：ts_arg_min、ts_arg_max、ts_delay  
- inner_op：ts_rank、ts_sum、ts_zscore、ts_std_dev  
