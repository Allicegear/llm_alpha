# WorldQuant BRAIN Operators Guide

This guide provides a comprehensive list of operators available on the WorldQuant BRAIN platform, categorized by their function.

## Arithmetic
Operators for basic mathematical operations.

| Operator       | Definition                           | Description |
| :------------- | :----------------------------------- | :---------- |
| `abs`          | `abs(x)`                             | Absolute value of x |
| `add`          | `add(x, y, filter = false), x + y`   | Add all inputs (at least 2 inputs required). If filter = true, filter all input NaN to 0 before adding |
| `densify`      | `densify(x)`                         | Converts a grouping field of many buckets into lesser number of only available buckets so as to make working with grouping fields computationally efficient. |
| `divide`       | `divide(x, y), x / y`                | x / y |
| `inverse`      | `inverse(x)`                         | 1 / x |
| `log`          | `log(x)`                             | Natural logarithm. For example: `Log(high/low)` uses natural logarithm of high/low ratio as stock weights. |
| `max`          | `max(x, y, ..)`                      | Maximum value of all inputs. At least 2 inputs are required |
| `min`          | `min(x, y ..)`                       | Minimum value of all inputs. At least 2 inputs are required |
| `multiply`     | `multiply(x ,y, ... , filter=false)` | Multiply all inputs. At least 2 inputs are required. Filter sets the NaN values to 1 |
| `power`        | `power(x, y)`                        | x ^ y |
| `reverse`      | `reverse(x)`                         | - x |
| `sign`         | `sign(x)`                            | if input > 0, return 1; if input < 0, return -1; if input = 0, return 0; if input = NaN, return NaN; |
| `signed_power` | `signed_power(x, y)`                 | x raised to the power of y such that final result preserves sign of x |
| `sqrt`         | `sqrt(x)`                            | Square root of x |
| `subtract`     | `subtract(x, y, filter=false), x - y`| x - y. If filter = true, filter all input NaN to 0 before subtracting |
| `to_nan`       | `to_nan(x, value=0, reverse=false)`  | Convert value to NaN or NaN to value if reverse=true |

## Logical
Operators for boolean logic and comparisons.

| Operator | Definition                       | Description |
| :------- | :------------------------------- | :---------- |
| `and`    | `and(input1, input2)`            | Logical AND operator, returns true if both operands are true and returns false otherwise |
| `if_else`| `if_else(input1, input2, input3)`| If `input1` is true then return `input2` else return `input3`. |
| `<`      | `input1 < input2`                | If `input1 < input2` return true, else return false |
| `<=`     | `input1 <= input2`               | Returns true if `input1 <= input2`, return false otherwise |
| `==`     | `input1 == input2`               | Returns true if both inputs are same and returns false otherwise |
| `>`      | `input1 > input2`                | Logic comparison operators to compares two inputs |
| `>=`     | `input1 >= input2`               | Returns true if `input1 >= input2`, return false otherwise |
| `!=`     | `input1 != input2`               | Returns true if both inputs are NOT the same and returns false otherwise |
| `is_nan` | `is_nan(input)`                  | If `(input == NaN)` return 1 else return 0 |
| `not`    | `not(x)`                         | Returns the logical negation of x. If x is true (1), it returns false (0), and if input is false (0), it returns true (1). |
| `or`     | `or(input1, input2)`             | Logical OR operator returns true if either or both inputs are true and returns false otherwise |

## Time Series
Operators that perform calculations over a historical window (lookback period `d`).

| Operator                    | Definition                                                            | Description |
| :-------------------------- | :-------------------------------------------------------------------- | :---------- |
| `days_from_last_change`     | `days_from_last_change(x)`                                            | Amount of days since last change of x |
| `hump`                      | `hump(x, hump = 0.01)`                                                | Limits amount and magnitude of changes in input (thus reducing turnover) |
| `jump_decay`                | `jump_decay(x, d, sensitivity=0.5, force=0.1)`                        | If there is a huge jump in current data compare to previous one |
| `kth_element`               | `kth_element(x, d, k)`                                                | Returns K-th valid value of input by looking through lookback days. This operator can be used to backfill missing data if k=1 |
| `last_diff_value`           | `last_diff_value(x, d)`                                               | Returns last x value not equal to current x value from last d days |
| `ts_arg_max`                | `ts_arg_max(x, d)`                                                    | Returns the relative index of the max value in the time series for the past d days. |
| `ts_arg_min`                | `ts_arg_min(x, d)`                                                    | Returns the relative index of the min value in the time series for the past d days. |
| `ts_av_diff`                | `ts_av_diff(x, d)`                                                    | Returns `x - tsmean(x, d)`, ignoring NaNs during mean computation. |
| `ts_backfill`               | `ts_backfill(x, d)`                                                   | Returns the first valid value of the input x by looking through lookback days (d). |
| `ts_corr`                   | `ts_corr(x, y, d)`                                                    | Returns correlation of x and y for the past d days |
| `ts_count_nans`             | `ts_count_nans(x ,d)`                                                 | Returns the number of NaN values in x for the past d days |
| `ts_covariance`             | `ts_covariance(y, x, d)`                                              | Returns covariance of y and x for the past d days |
| `ts_decay_linear`           | `ts_decay_linear(x, d, dense = false)`                                | Returns the linear decay on x for the past d days. |
| `ts_delay`                  | `ts_delay(x, d)`                                                      | Returns x value d days ago |
| `ts_delta`                  | `ts_delta(x, d)`                                                      | Returns `x - ts_delay(x, d)` |
| `ts_max`                    | `ts_max(x, d)`                                                        | Returns max value of x for the past d days |
| `ts_mean`                   | `ts_mean(x, d)`                                                       | Returns average value of x for the past d days. |
| `ts_min`                    | `ts_min(x, d)`                                                        | Returns min value of x for the past d days |
| `ts_product`                | `ts_product(x, d)`                                                    | Returns product of x for the past d days |
| `ts_quantile`               | `ts_quantile(x,d, driver="gaussian" )`                                | Inverse CDF of `ts_rank` based on driver distribution (default: gaussian). |
| `ts_rank`                   | `ts_rank(x, d, constant = 0)`                                         | Rank values of x over past d days, return rank + constant. |
| `ts_regression`             | `ts_regression(y, x, d, lag = 0, rettype = 0)`                        | Returns parameters related to regression (e.g., Error Term, slope, R-Square). |
| `ts_scale`                  | `ts_scale(x, d, constant = 0)`                                        | Returns `(x - ts_min)/(ts_max - ts_min) + constant` in time series space. |
| `ts_std_dev`                | `ts_std_dev(x, d)`                                                    | Returns standard deviation of x for the past d days |
| `ts_step`                   | `ts_step(1)`                                                          | Returns days' counter |
| `ts_sum`                    | `ts_sum(x, d)`                                                        | Sum values of x for the past d days. |
| `ts_target_tvr_decay`       | `ts_target_tvr_decay(x, lambda_min=0, lambda_max=1, target_tvr=0.1)`  | Tune `ts_decay` to meet a turnover target. |
| `ts_target_tvr_delta_limit` | `ts_target_tvr_delta_limit(x, y, ...)`                                | Tune `ts_delta_limit` to meet a turnover target. |
| `ts_zscore`                 | `ts_zscore(x, d)`                                                     | Z-score measurement: `(x - tsmean(x,d)) / tsstddev(x,d)`. |

## Cross Sectional
Operators that perform calculations across all instruments at a single point in time.

| Operator      | Definition                                   | Description |
| :------------ | :------------------------------------------- | :---------- |
| `normalize`   | `normalize(x, useStd = false, limit = 0.0)`  | Subtracts mean value from each alpha element. |
| `quantile`    | `quantile(x, driver = gaussian, sigma = 1.0)`| Ranks raw vector and applies distribution (gaussian, cauchy, uniform). |
| `rank`        | `rank(x, rate=2)`                            | Ranks input among instruments; returns number between 0.0 and 1.0. |
| `scale`       | `scale(x, scale=1, longscale=1, shortscale=1)`| Scales input to booksize. |
| `scale_down`  | `scale_down(x, constant=0)`                  | Scales all values proportionately between 0 and 1. |
| `vector_neut` | `vector_neut(x, y)`                          | Finds a new vector x* orthogonal to y (neutralizes x against y). |
| `winsorize`   | `winsorize(x, std=4)`                        | Caps values to within specified multiples of standard deviation. |
| `zscore`      | `zscore(x)`                                  | Z-score measurement relative to the group mean (cross-sectional). |

## Vector
Operators dealing with vector fields (e.g., arrays of data per instrument).

| Operator | Definition | Description |
| :------- | :--------- | :---------- |
| `vec_avg`| `vec_avg(x)` | Taking mean of the vector field x |
| `vec_max`| `vec_max(x)` | Maximum value form vector field x |
| `vec_min`| `vec_min(x)` | Minimum value form vector field x |
| `vec_sum`| `vec_sum(x)` | Sum of vector field x |

## Transformational
Operators for transforming alpha values or signal structures.

| Operator    | Definition                            | Description |
| :---------- | :------------------------------------ | :---------- |
| `bucket`    | `bucket(rank(x), range="0, 1, 0.1")`  | Convert float values into user-specified buckets/indexes. |
| `trade_when`| `trade_when(x, y, z)`                 | Change or hold Alpha values based on specified conditions. |

## Group
Operators that perform calculations within defined groups (e.g., industry, sector).

| Operator                 | Definition                                | Description |
| :----------------------- | :---------------------------------------- | :---------- |
| `group_backfill`         | `group_backfill(x, group, d, std = 4.0)`  | Fills NaNs using winsorized mean of instruments in the same group. |
| `group_cartesian_product`| `group_cartesian_product(g1, g2)`         | Merge two groups into one. |
| `group_max`              | `group_max(x, group)`                     | Maximum of x for all instruments in the same group. |
| `group_mean`             | `group_mean(x, weight, group)`            | Assigns the mean value of the group to all elements. |
| `group_min`              | `group_min(x, group)`                     | Assigns the minimum value of the group to all elements. |
| `group_neutralize`       | `group_neutralize(x, group)`              | Neutralizes Alpha against specified groups. |
| `group_rank`             | `group_rank(x, group)`                    | Assigns corresponding rank within the group to each element. |
| `group_scale`            | `group_scale(x, group)`                   | Normalizes values in a group to be between 0 and 1. |
| `group_zscore`           | `group_zscore(x, group)`                  | Z-score calculation within its instrument group. |
