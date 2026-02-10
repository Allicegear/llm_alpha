# How to improve japan sharpe in ASI

1. Remove Japan by cap
- 日本所有公司的平均市值在亚洲所有国家中排在第四,那么其在rank(group_mean(cap,1,country))的位置可能是在0.6-0.8之间。通过if_else把日本剔除后,其他国家的市值会上升,从而提高alpha的夏普比率
- Alhap Example:
signal=s_log_1p(winsorize(ts_backfill(vec_choose(df,nth=0),22),std=4));
gr = rank(group_mean(cap,1,country));
if_else(or(gr>0.8,gr<0.6),signal,nan)

2. select japan by factor loading
- 使用rsk70_mfm2_asetrd_cnt_jpn做loading mask，先在有JPN并有factor loading的sub universe上做回测，如果可以通过各种测试，那么这个alpha在ASI区域的Japan Robustness Sharpe会通过测试。
- Alhap Example:
mask = abs(rsk70_mfm2_asetrd_cnt_jpn);
mask? signal - regression_proj(signal,rsk70_mfm2_asetrd_momentum):nan;