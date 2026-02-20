# Getting Started with the EUR TOPCS1600 Universe

## Characteristics of the EUR TOPCS1600 Universe
The EUR TOPCS1600 universe is a new EUR universe that adds diversity by focusing on new and unique country exposures and moving away from countries that are already crowded in the BRAIN pool and actively researched in other universes.

## Tips for Success
- Liquidity is important in EUR TOPCS1600. Ensure that a significant part of your performance comes from the upper 40% of stocks by capitalization (Sharpe By Capitalization in Visualization Tool), i.e.:
  - Buckets 60–80
  - Buckets 80–100
- Use the Sub-Universe test as an additional quality filter. Try to maximize sub-universe Sharpe, not just barely pass the minimum.
- Use Investability-Constrained Sharpe statistics or set Max Trade Setting ON.
  - Investability-constrained Sharpe > 0.8
  - Margin > 5‰
- Implement rank test:
  - Apply a rank() operator to your Alpha and check its performance.
  - The Sharpe after rank should still be positive.
  - If performance disappears after ranking, your Alpha is possibly too sensitive to exact value levels and is less robust.
- Start with datasets where you previously had Alphas with Sharpe > 0.8 but < 1 on other EUR universes
- Seek to leverage your existing work:
  - Re-simulate your EUR TOP1200 Alphas on the EUR TOPCS1600 universe.
  - Identify Alphas that:
    - Maintain or improve Sharpe under investability constraints.
    - Show good performance in upper cap buckets (60–80, 80–100).