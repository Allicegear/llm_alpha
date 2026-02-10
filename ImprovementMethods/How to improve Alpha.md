# How to improve Alpha

1. 提高Robust Universe Sharpe
    - group_rank
    - signed_power
2. 降低Turnover
    - ts_target_tvr_decay
    - ts_decay_linear
    - change decay
3. Weight过于集中
    - ts_backfill
    - ts_arg_max
    - ts_arg_min
    - ts_av_diff
    - trade_when
    - Switch universe 
4. 2 year Sharpe<1.58
    - use group_ series operator
5. 流动性不足
    - group_neutralize
    - Switch universe 
6. 提高sharpe/fitness
    - signed_power
7. 降低 product correlation
    - max-trade
    - change decay
    - change ts_ series operator time window
    - switch to another group with group_neutralize
