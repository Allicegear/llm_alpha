# Tips for getting started with ***Institutions*** Datasets:

- Institutional data offers insights into funds' holdings for specific financial instruments, and allows for the derivation of transaction timing and size based on this holdings data.
- Investors tend to sell both their best and worst performing holdings, as opposed to the average performers in their portfolios. This behavior, termed the 'rank effect', is separate from the disposition effect and does not offset specific types of systematic risk. To enhance the risk-adjusted return or 'Sharpe ratio', additional risk neutralization strategies can be employed.

# Example Alpha Ideas:

1. Investments by large institutional investors can occasionally be significant and can be signals to predict returns. Hence, ts_max can be useful for this dataset. Additionally, sometimes institutions invest high on a stock because they believe its return potential. To check reliability, the ts_corr of institutions field with returns can be used.
2. Institutional investors often invest based on sectors/industry/subindustry. To reduce exposure to those groups, group_neutralize can be handy.
3. Country and sector neutralizations generally work well, although we recommend you to try other groups as well
4. Since some fields of institution category datasets may beindirect predictors of returns, you can calculate the correlation between these fields and returns/close to assess its predictive power