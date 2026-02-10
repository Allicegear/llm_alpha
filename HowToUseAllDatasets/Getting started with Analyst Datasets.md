# Tips for getting started with ***Analyst*** Datasets:

- Analyst datasets have well-structured data fields that can be considered as analysts’ sentiment scores for various fundamental ratios. Common time series and cross-sectional operations such as ts_rank, zscore, or rank can be applied to these fields.  

- Comparing analyst scores on the same fundamental ratio from two different time periods can provide useful signals. For this purpose, you can use the ts_delta operator.  

- General guidance for using the group operators on model data fields includes trying the following: group_rank, group_neutralize, group_normalize, and group_zscore.  

- Since some analyst datasets indirectly seeks to predict returns, you can assess their potential predictive power by taking the correlation of data fields with returns/close prices.  

- Analyst datasets can be large and serve as signals that seeks to predict potential returns. Therefore, ts_delta can be useful for this dataset. For instance, changes in EPS can detect earning surprises.  
    - When calculating differences, be cautious of stock-split events when dealing with this kind of dataset.  

- To reduce exposure to groups, group_neutralize can be helpful.  
    - Country and sector neutralizations generally work well, though we recommend trying other groups as well.

---

# Example Alpha Ideas:

- Check differences between estimates of long-term versus short-term horizons, and set reversal or momentum signals based on which time horizon is higher. The same idea can apply to comparisons between estimates and actuals.  

- Assign Alphas based directly on the estimates, or delta(estimates), or correlation(estimates, returns on the same horizon).   

- Assign Alphas in the event of high dispersion among estimates or estimates of different time horizons.  