# Dataset Analysis: fundamental31 - Economic Categorization

**Region**: GLB | **Universe**: TOP3000 | **Delay**: 1  
**Total Fields**: 81

## Dataset Overview
`fundamental31` 数据集是一个综合性的基本面和市场信号集合。它由系统评估过程产生，涵盖了基本面、技术面、高频数据以及特定行业的信号。该数据集特别强调了盈利质量、偿债风险、估值水平以及分析师预期的变化，适合用于构建多因子选股模型，捕捉企业的长期价值和短期动量。

---

## 分类 1: 盈利能力 (Profitability) - 共 8 个字段
*这些字段反映了企业的核心盈利能力，包括毛利率、资产回报效率以及营业利润。*

| Field ID | Type | 简述 | 经济学意义 |
| --- | --- | --- | --- |
| `fnd31_devnorthamericaadditionalfactor4_apg` | Matrix | TTM 总资产毛利率 (GP/Assets) | 衡量企业利用资产产生毛利的能力，反映资产的使用效率。 |
| `fnd31_devnorthamericaadditionalfactor4_mpg` | Matrix | TTM 毛利率 (GP/Sales) | 衡量企业产品的市场竞争力和单位销售收入的盈利水平。 |
| `fnd31_pu` | Matrix | 意外盈利能力 (Unexpected Profitability) | 实际盈利与预期盈利的差异，反映盈余超预期的程度。 |
| `fnd31_qsg5additionalfactor3_monthly_apg` | Matrix | 滚动 TTM 总资产毛利率 | 动态衡量企业的资产盈利效率。 |
| `fnd31_qsg5additionalfactor3_monthly_mpg` | Matrix | 滚动 TTM 毛利率 | 动态衡量企业在销售端的溢价能力和成本控制。 |
| `fnd31_rp` | Matrix | 盈利比率 (Operating Income/Assets) | 衡量企业核心经营活动的盈利水平与总资产的关系。 |
| `fnd31_ttmopincev` | Matrix | TTM 营业利润与企业价值比 (OI/EV) | 衡量相对于企业整体价值的经营盈利水平，兼具盈利和估值含义。 |
| `fnd31_vefcomtt` | Matrix | TTM 经营现金流与企业价值比 (OCF/EV) | 提供基于现金流的盈利水平，用于评估企业的现金收益能力。 |

---

## 分类 2: 偿债能力与财务风险 (Solvency & Financial Risk) - 共 14 个字段
*这些字段评估企业的财务稳定性、破产风险以及信贷质量。*

| Field ID | Type | 简述 | 经济学意义 |
| --- | --- | --- | --- |
| `fnd31_adpmoc` | Matrix | 杠杆组合变化 (DA 变动) | 衡量最近一季度债务资管比相对于上季度的变化，捕捉杠杆结构变动。 |
| `fnd31_chg12mtotdebt` | Matrix | 12个月总债务变动 | 反映企业融资活动和杠杆的扩张或收缩情况。 |
| `fnd31_creditrev3m` | Matrix | 3个月信用评级修正 (CDS) | 基于 CDS 水平的变化衡量信贷质量的改善或恶化。 |
| `fnd31_creditrisk` | Matrix | 信用风险 (CDS Spread) | 使用 CDS 掉期息差衡量企业的违约风险，是信贷风险的直接指标。 |
| `fnd31_ebitdadebt` | Matrix | TTM EBITDA 与长期债务比 | 衡量企业利用经营现金流偿还长期债务的能力，高比率意味着更强的偿债能力。 |
| `fnd31_ebitdadebtchg` | Matrix | EBITDA 与债务比的年度变化 | 捕捉企业偿债能力的趋势性变化。 |
| `fnd31_eqcdspricediv` | Matrix | CDS 与股价背离度 | 衡量股价与信贷违约风险之间的历史联动关系，识别潜在的风险异常。 |
| `fnd31_md` | Matrix | 财务困境衡量标准 (Distress Measure) | 基于 Campbell、Hilscher 和 Szilagyi 模型预测企业面临财务困难的可能性。 |
| `fnd31_ohlsonscore` | Matrix | Ohlson 破产预测得分 (O-Score) | 经典的破产预测模型，通过多个财务指标评估企业在未来一年内破产的概率。 |
| `fnd31_slope_spreadsub` | Matrix | CDS 息差趋势线斜率 | 捕捉信贷风险在时间维度上的变化率，向上倾斜反映风险递增。 |
| `fnd31_creditrisk` | Matrix | 信用风险指标 | (重复引用) 用于评估企业违约风险等级。 |
| `fnd31_ohlsonscore` | Matrix | Ohlson O-Score | (重复引用) 企业破产概率的系统评估。 |
| `fnd31_md` | Matrix | 财务风险评分 | (重复引用) 衡量企业的总体财务稳定性。 |
| `fnd31_eqcdspricediv` | Matrix | 权益与信贷市场背离 | (重复引用) 捕捉市场对企业信用和股票价值的不同预期。 |

---

## 分类 3: 价值评估 (Valuation) - 共 22 个字段
*这些字段衡量股票相对于其基本面价值的便宜程度，包括收益率、现金流收益率等。*

| Field ID | Type | 简述 | 经济学意义 |
| --- | --- | --- | --- |
| `fnd31_coreepsp` | Matrix | TTM 核心收益价格比 (E/P) | 衡量扣除非经常性损益后的盈利水平相对于股价的吸引力。 |
| `fnd31_devnorthamericaadditionalfactor4_fwdebitdaev` | Matrix | 预测 12个月 EBITDA/EV | 基于分析师预测的 EBITDA 评估企业相对于其企业价值的估值。 |
| `fnd31_devnorthamericaadditionalfactor4_pbdwf` | Matrix | 分析师预测平均 B/P | 基于未来一季度账面价值预测的收益率，反映价值投资属性。 |
| `fnd31_devnorthamericaadditionalfactor4_pbwt` | Matrix | 时间加权估计 B/P | 综合分析师对未来预期的账面价值收益率。 |
| `fnd31_devnorthamericaadditionalfactor4_pcdwf` | Matrix | 分析师预测平均 C/P | 基于现金流预测的价值，反映企业的现金收益估值。 |
| `fnd31_devnorthamericaadditionalfactor4_pcwt` | Matrix | 时间加权估计 C/P | 利用时间加权预测现金流评估估值水平。 |
| `fnd31_devnorthamericaadditionalfactor4_ps_wt` | Matrix | 时间加权估计 S/P | 利用销售额预测评估企业的销售收益估值。 |
| `fnd31_devnorthamericaadditionalfactor4_psdwf` | Matrix | 分析师预测平均 S/P | 领先性的销售收益估值，适用于利润尚未实现的成长型公司。 |
| `fnd31_devnorthamericaadditionalfactor4_tw_ebitdaev` | Matrix | 时间加权 EBITDA/EV | 结合多个预测周期的 EBITDA 评估企业估值。 |
| `fnd31_irttmsalesev` | Matrix | 行业中性化 TTM 销售与企业价值比 | 消除行业差异后的销售估值，识别处于行业估值洼地的公司。 |
| `fnd31_monthly_avg10yepinf` | Matrix | 周期与通胀调整后的 E/P | 类似于席勒 PE 的倒数，通过平均 10 年经通胀调整后的收益来评估长期估值。 |
| `fnd31_pbroeresidual` | Matrix | PB-ROE 剩余模型组合 | 基于 PB-ROE 关系调整后的估值模型，识别盈利能力超出市场估值的股票。 |
| `fnd31_qsg5additionalfactor3_monthly_fwdebitdaev` | Matrix | 动态分析师预测 EBITDA/EV | 实时追踪分析师对企业估值的预期变化。 |
| `fnd31_qsg5additionalfactor3_monthly_pbdwf` | Matrix | 动态分析师预测 B/P | 捕捉账面价值估值预期的最新变动。 |
| `fnd31_qsg5additionalfactor3_monthly_pbwt` | Matrix | 动态时间加权 B/P | 结合不同期限的分析师 B/P 预测。 |
| `fnd31_qsg5additionalfactor3_monthly_pcdwf` | Matrix | 动态分析师预测 C/P | 现金流估值预期的最新追踪。 |
| `fnd31_qsg5additionalfactor3_monthly_pcwt` | Matrix | 动态时间加权 C/P | 综合现金流预期的估值权重。 |
| `fnd31_qsg5additionalfactor3_monthly_ps_wt` | Matrix | 动态时间加权 S/P | 综合销售预期的估值权重。 |
| `fnd31_qsg5additionalfactor3_monthly_psdwf` | Matrix | 动态分析师预测 S/P | 销售估值预期的最新动态。 |
| `fnd31_qsg5additionalfactor3_monthly_tw_ebitdaev` | Matrix | 动态时间加权 EBITDA/EV | 计算更精准、动态的企业价值收益。 |
| `fnd31_yen` | Matrix | 标准化收益率 (Normalized Yield) | 结合多种收益预测方法（如 RQI、RD）归一化后的收益估值指标。 |
| `fnd31_devnorthamericaadditionalfactor4_tw_ebitdaev` | Matrix | 估值加权组合 | (重复引用) 用于识别低估值的投资标的。 |

---

## 分类 4: 增长能力 (Growth) - 共 14 个字段
*这些字段反映了企业的历史和预期增长指标，包括盈利、毛利和 ROE 的变化。*

| Field ID | Type | 简述 | 经济学意义 |
| --- | --- | --- | --- |
| `fnd31_cpgspea2y` | Matrix | 未来 2 年 EPS 增长预测百分比 | 衡量分析师对企业长期盈利扩张的信心和预期。 |
| `fnd31_devnorthamericaadditionalfactor4_apgghcy1` | Matrix | 资产盈利能力 (GP/Assets) 1年变动 | 衡量企业资产盈利效率的短期改善。 |
| `fnd31_devnorthamericaadditionalfactor4_apgghcy3` | Matrix | 资产盈利能力 (GP/Assets) 3年变动 | 衡量企业资产收益率的长期趋势和稳定性。 |
| `fnd31_devnorthamericaadditionalfactor4_mpgghcy3` | Matrix | 毛利率 3年变动 | 长期衡量企业成本控制和定价权的改善。 |
| `fnd31_devnorthamericaadditionalfactor4_yoychgroeart` | Matrix | ROE 年度算术变化 | 捕捉企业净资产收益率的直接增减。 |
| `fnd31_devnorthamericaadditionalfactor4_yoychgroepct` | Matrix | ROE 年度百分比变化 | 衡量 ROE 变动的幅度，反映盈利质量的边际改善。 |
| `fnd31_fc_y2repsg` | Matrix | 2年预计 EPS 增长 | 分析师对中长期盈利增速的共识。 |
| `fnd31_monthly_apgghcm3` | Matrix | 资产毛利率季度环比变化 | 捕捉高频的基本面盈利质量改善信号。 |
| `fnd31_monthly_mpgghcm3` | Matrix | 毛利率季度环比变化 | 反映企业经营环境或竞争格局的快速变动。 |
| `fnd31_qsg5additionalfactor3_monthly_apgghcy1` | Matrix | 毛利/资产年度同比动态变化 | 用于追踪盈利路径的动态调整。 |
| `fnd31_qsg5additionalfactor3_monthly_apgghcy3` | Matrix | 毛利/资产三年动态变化 | 长期增长动量的高频追踪。 |
| `fnd31_qsg5additionalfactor3_monthly_mpgghcy3` | Matrix | 毛利率三年同比动态变化 | 衡量毛利水平扩张的连贯性。 |
| `fnd31_qsg5additionalfactor3_monthly_yoychgroeart` | Matrix | ROE 动态同比算术变化 | 评估资产运营回报的效率趋势。 |
| `fnd31_qsg5additionalfactor3_monthly_yoychgroepct` | Matrix | ROE 动态同比百分比变化 | 衡量公司创造股东回报能力的增长动量。 |

---

## 分类 5: 盈余质量与预期变化 (Earnings Quality & Revision) - 共 21 个字段
*这些字段包含分析师修正、盈余意外以及盈利趋势的稳健性。*

| Field ID | Type | 简述 | 经济学意义 |
| --- | --- | --- | --- |
| `fnd31_chgqtrepssurp` | Matrix | 季度盈利意外变动 | 反映实际盈利相对于分析师预期盈余的意外增长。 |
| `fnd31_devnorthamericaadditionalfactor4_earningstorpedo` | Matrix | 盈余“鱼雷”指标 (Earnings Torpedo) | 综合预期和意外收益，用于识别可能面临业绩大幅回撤的高估值或预期过热公司。 |
| `fnd31_devnorthamericaadditionalfactor4_numrevy2` | Matrix | FY2 分析师净修正数量 | 衡量对下一财年业绩修正的广度和共识度。 |
| `fnd31_devnorthamericaadditionalfactor4_rev6fy2` | Matrix | FY2 前 6个月平均 EPS 修正 | 捕捉分析师对长期业绩预测的趋势。 |
| `fnd31_devnorthamericaadditionalfactor4_slope4qeps5y` | Matrix | 5年 TTM EPS 趋势线斜率 | 反映过去 5 年每股收益的长期增长速度。 |
| `fnd31_devnorthamericaadditionalfactor4_twepsstdrev` | Matrix | 时间加权盈余修正分散度 | 衡量分析师对未来业绩预期的分歧程度，高分歧通常对应高风险。 |
| `fnd31_devnorthamericaadditionalfactor4_y5speq4rqsr` | Matrix | 5年 TTM EPS 趋势线性拟合度 (R-sqr) | 衡量收益增长的可预测性和稳定性，高质量盈利对应高拟合度。 |
| `fnd31_devnorthamericaadditionalfactor4_y5speq4vc` | Matrix | 5年 TTM EPS 波动率 (稳定性) | 数值较低（标准差小）反映收益增长更平滑、更可靠。 |
| `fnd31_earnshortfall` | Matrix | 会计收益与现金流缺口 (Shortfall) | 衡量净利润与自由现金流的差异，较大的缺口通常预示着潜在的盈余管理或坏账风险。 |
| `fnd31_fc_fqsurstd` | Matrix | 最近季度盈利意外 (Standardized) | 标准化后的盈余惊喜，是著名的 PEAD（盈余公告后漂移）策略的核心驱动。 |
| `fnd31_fqsurstd60dlag` | Matrix | 过去 60 天滞后的季度盈利意外 | 捕捉盈余公告后的动能效应和反应不足带来的机会。 |
| `fnd31_qsg5additionalfactor3_monthly_earningstorpedo` | Matrix | 动态盈余雷区指标 | 高频追踪估值与盈利匹配风险。 |
| `fnd31_qsg5additionalfactor3_monthly_numrevy2` | Matrix | 动态 FY2 净修正统计 | 捕捉分析师观点的最新转变倾向。 |
| `fnd31_qsg5additionalfactor3_monthly_rev6fy2` | Matrix | 动态平均 EPS 修正追踪 | 识别中长期预期的边际变化。 |
| `fnd31_qsg5additionalfactor3_monthly_slope4qeps5y` | Matrix | 动态盈利增长趋势预测 | 评估盈利跑道是否变窄或增宽。 |
| `fnd31_qsg5additionalfactor3_monthly_twepsstdrev` | Matrix | 动态盈余预期分歧 | 衡量市场的不确定性情绪。 |
| `fnd31_qsg5additionalfactor3_monthly_y5speq4rqsr` | Matrix | 动态收益稳定性判定 | 用于筛选盈余质量突出的个股。 |
| `fnd31_qsg5additionalfactor3_monthly_y5speq4vc` | Matrix | 动态收益波动指标 | 评估盈利预测的风险边界。 |
| `fnd31_rev3my1std` | Matrix | FY1 EPS 预测 3个月修正相对差 | 捕捉分析师对当前财年收益预期的积极或消极调整。 |
| `fnd31_rev3my2std` | Matrix | FY2 EPS 预测 3个月修正相对差 | 捕捉对下一财年预期的趋势修正。 |
| `fnd31_twepsrev` | Matrix | 时间加权盈余修正 (6个月均值) | 衡量过去半年分析师共识的整体修正方向。 |

---

## 分类 6: 波动率与市场风险 (Volatility & Market Risk) - 共 4 个字段
*这些字段描述股票收益的波动性、非对称性以及市场定价的稳健性。*

| Field ID | Type | 简述 | 经济学意义 |
| --- | --- | --- | --- |
| `fnd31_60dsigma` | Matrix | 60天收益率波动率 | 经典的风险度量指标，通常反映较低的风险溢价或用于低波动增强策略。 |
| `fnd31_dtsm1_rd` | Matrix | 1个月日收益率波动率 | 衡量短期股价变动的剧烈程度，捕捉短期市场情绪和极端波动风险。 |
| `fnd31_rqi_rd` | Matrix | 1个月收益率四分位距 (IQR) | 相对于标准差更稳健的离散度衡量指标，用于描述股价分布的宽度。 |
| `fnd31_slope_spreadsub` | Matrix | 趋势线斜率 | 用于捕捉资产风险调整后的趋势方向。 |

---

## 分类 7: 运营效率 (Efficiency) - 共 2 个字段
*反映企业资本支出效率和存货管理能力。*

| Field ID | Type | 简述 | 经济学意义 |
| --- | --- | --- | --- |
| `fnd31_capexdeplink` | Matrix | 资本支出与折旧关联度 | 衡量 CAPEX 与折旧的平衡，反映企业是在维持现状还是在进行大规模扩张。 |
| `fnd31_chginvavgast` | Matrix | 存货变动占平均资产比 | 衡量企业经营状况，存货意外增加可能是需求疲软的预警信号，而良性增加反映扩张。 |

---

## 因子使用建议

### 1. 估值与质量的平衡
该数据集具有非常丰富的带有“分析师修正”和“估值收益率”属性的字段。建议将 `Valuation` 分类中的 yield 因子（如 `fnd31_yen`）与 `Earnings Quality` 中的稳定性因子（如 `fnd31_devnorthamericaadditionalfactor4_y5speq4rqsr`）结合使用，构建 GARP (以合理价格增长) 策略。

### 2. 捕捉盈余意外脉冲
使用 `fnd31_fc_fqsurstd` 及其滞后项构建事件驱动策略。通常盈余公告后的漂移效应在小市值和分析师覆盖较少的股票中更为显著。

### 3. 时间加权因子的高级应用
数据集提供的 `tw_` (Time Weighted) 因子相较于简单的时点预测更具稳健性。在多因子建模时，优先使用这些经过加权处理的预期值。

### 4. 偿债与破产预警
在构建纯基本面组合时，务必将 `fnd31_ohlsonscore` 或 `fnd31_md` 作为风险排除项（Masking）或惩罚项，以规避潜在的财务暴雷风险。
