1. ts_delta(xx,days) 操作符对于2 year Sharpe of 1.XX is below cutoff of 1.58 有奇效.

2. 使用ts_mean在analyst数据集构造预测偏差因子. ts_mean(sign(field),day).

3. 减少无效回测: 识别伪装成vector的matrix字段,就是每天基本稳定地只有一个数据的vector字段. 这种字段用 vec_sum,vec_avg,vec_choose,vec_min,vec_max这5个vector运算符回测的差距绝不会超过2%,而别的vec运算符也没有意义. 已知的此类数据集: earnings4, model109, model135.

4. YOLO 模式 (Ctrl+Y)：YOLO = "You Only Live Once"。通常在 AI CLI 中，这意味着**“自动执行模式”**。

5. impove robust universe sharpe, 使用signed_power， 参数1.3.

6. INd 区域推荐新手从model、pv、other这三个数据类别做起,进一步，探索analyst、fundamental、imbalance、institutions、risk这五个数据类别.老手尝试earnings、insiders、option这三个数据类别.高手挑战broker、macro、sentiment、shortinterest、socialmedia这五个数据类别.
