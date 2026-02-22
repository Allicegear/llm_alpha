# WorldQuant Brain Operators Reference (Structured)

**Total Count:** 140 Operators

---

## 1. Arithmetic (24)

### `abs(x)`
- **Permission:** Base
- **Scope:** Combo, Regular, Selection
- **Description:** Absolute value of x.

### `add(x, y, filter = false)`
- **Alias:** `x + y`
- **Permission:** Base
- **Scope:** Combo, Regular, Selection
- **Description:** Add all inputs (at least 2 inputs required). If filter = true, filter all input NaN to 0 before adding.

### `arc_sin(x)`
- **Permission:** Genius
- **Scope:** Combo, Regular, Selection
- **Description:** If -1 <= x <= 1: arcsin(x); else NaN.

### `densify(x)`
- **Permission:** Base
- **Scope:** Combo, Regular
- **Description:** Converts a grouping field of many buckets into lesser number of only available buckets so as to make working with grouping fields computationally efficient. If the number of unique values in x is n, densify maps those values between 0 and (n-1).
- **Example:** Say a grouping field is provided as an integer (e.g., industry: tech -> 0, airspace -> 1, ...) and for a certain date, we have instruments with grouping field values among {0, 1, 2, 99}. Instead of creating 100 buckets and keeping 96 of them empty, it is better to just create 4 buckets with values {0, 1, 2, 3}.

### `divide(x, y)`
- **Alias:** `x / y`
- **Permission:** Base
- **Scope:** Combo, Regular, Selection
- **Description:** x / y.

### `exp(x)`
- **Permission:** Genius
- **Scope:** Combo, Regular, Selection
- **Description:** Natural exponential function: e^x.

### `fraction(x)`
- **Permission:** Genius
- **Scope:** Combo, Regular, Selection
- **Description:** This operator removes the whole number part and returns the remaining fraction part with sign. Range is between -1 to 1 and it is an odd function. Formula: `sign(x) * (abs(x) – floor(abs(x)))`.
- **Example:**
  - If x = 5.63 ⇒ fraction(x) = 0.63
  - If x = -4.59 ⇒ fraction(x) = -0.59

### `inverse(x)`
- **Permission:** Base
- **Scope:** Combo, Regular, Selection
- **Description:** 1 / x.

### `log(x)`
- **Permission:** Base
- **Scope:** Combo, Regular, Selection
- **Description:** Natural logarithm.
- **Example:** Log(high/low) uses natural logarithm of high/low ratio as stock weights.

### `log_diff(x)`
- **Permission:** Genius
- **Scope:** Combo, Regular
- **Description:** Returns `log(x[t]) - log(x[t-1])`. The operator calculates the log of the data field value today and subtracts from it the log of the data field values of yesterday.

### `max(x, y, ..)`
- **Permission:** Base
- **Scope:** Combo, Regular, Selection
- **Description:** Maximum value of all inputs. At least 2 inputs are required.

### `min(x, y ..)`
- **Permission:** Base
- **Scope:** Combo, Regular, Selection
- **Description:** Minimum value of all inputs. At least 2 inputs are required.

### `multiply(x ,y, ... , filter=false)`
- **Alias:** `x * y`
- **Permission:** Base
- **Scope:** Combo, Regular, Selection
- **Description:** Multiply all inputs. At least 2 inputs are required. Filter sets the NaN values to 1.

### `nan_mask(x, y)`
- **Permission:** Genius
- **Scope:** Combo, Regular
- **Description:** Replace input with NAN if input's corresponding mask value or the second input here, is negative.

### `power(x, y)`
- **Permission:** Base
- **Scope:** Combo, Regular, Selection
- **Description:** x ^ y. Used to implement popular mathematical functions (e.g., sigmoid).

### `reverse(x)`
- **Permission:** Base
- **Scope:** Combo, Regular, Selection
- **Description:** - x (Negative x).

### `s_log_1p(x)`
- **Permission:** Genius
- **Scope:** Combo, Regular, Selection
- **Description:** Confines function to a shorter range using logarithm. Formula: `sign(x) * log(1 + abs(x))`.
- **Example:**
  - If x = 9 ⇒ sign(x) * log(1+abs(x)) = 1
  - If x = -9 ⇒ sign(x) * log(1+abs(x)) = -1

### `sigmoid(x)`
- **Permission:** Genius
- **Scope:** Combo, Regular, Selection
- **Description:** Returns 1 / (1 + exp(-x)).

### `sign(x)`
- **Permission:** Base
- **Scope:** Combo, Regular, Selection
- **Description:** If input > 0, return 1; if input < 0, return -1; if input = 0, return 0; if input = NaN, return NaN.

### `signed_power(x, y)`
- **Permission:** Base
- **Scope:** Combo, Regular, Selection
- **Description:** x raised to the power of y such that final result preserves sign of x. Formula: `sign(x) * (abs(x) ^ y)`.
- **Example:**
  - If x = 3, y = 2 ⇒ signed_power(x, y) = 9
  - If x = -9, y = 0.5 ⇒ signed_power(x, y) = -3

### `sqrt(x)`
- **Permission:** Base
- **Scope:** Combo, Regular, Selection
- **Description:** Square root of x.

### `subtract(x, y, filter=false)`
- **Alias:** `x - y`
- **Permission:** Base
- **Scope:** Combo, Regular, Selection
- **Description:** x-y. If filter = true, filter all input NaN to 0 before subtracting.

### `to_nan(x, value=0, reverse=false)`
- **Permission:** Genius
- **Scope:** Combo, Regular, Selection
- **Description:** Convert value to NaN or NaN to value if reverse=true.

### `tanh(x)`
- **Permission:** Genius
- **Scope:** Combo, Regular, Selection
- **Description:** Hyperbolic tangent of x. Output will be between -1 and 1.
- **Example:** If x = 0, tanh(x) = 0; If x = 2, tanh(x) = 0.964.

---

## 2. Logical (11)

### `and(input1, input2)`
- **Permission:** Base
- **Scope:** Combo, Regular, Selection
- **Description:** Logical AND operator, returns true if both operands are true and returns false otherwise.

### `if_else(input1, input2, input 3)`
- **Permission:** Base
- **Scope:** Combo, Regular, Selection
- **Description:** If input1 is true then return input2 else return input3.
- **Syntax:** `if_else(event_condition, Alpha_expression_1, Alpha_expression_2)`

### `input1 < input2`
- **Permission:** Base
- **Scope:** Combo, Regular, Selection
- **Description:** If input1 < input2 return true, else return false.

### `input1 <= input2`
- **Permission:** Base
- **Scope:** Combo, Regular, Selection
- **Description:** Returns true if input1 <= input2, return false otherwise.

### `input1 == input2`
- **Permission:** Base
- **Scope:** Combo, Regular, Selection
- **Description:** Returns true if both inputs are same and returns false otherwise.

### `input1 > input2`
- **Permission:** Base
- **Scope:** Combo, Regular, Selection
- **Description:** Logic comparison operators to compares two inputs.

### `input1 >= input2`
- **Permission:** Base
- **Scope:** Combo, Regular, Selection
- **Description:** Returns true if input1 >= input2, return false otherwise.

### `input1 != input2`
- **Permission:** Base
- **Scope:** Combo, Regular, Selection
- **Description:** Returns true if both inputs are NOT the same and returns false otherwise.

### `is_nan(input)`
- **Permission:** Base
- **Scope:** Combo, Regular, Selection
- **Description:** If (input == NaN) return 1 else return 0. Used to identify NaN values and replace them via `if_else`.

### `not(x)`
- **Permission:** Base
- **Scope:** Combo, Regular, Selection
- **Description:** Returns the logical negation of x.

### `or(input1, input2)`
- **Permission:** Base
- **Scope:** Combo, Regular, Selection
- **Description:** Logical OR operator returns true if either or both inputs are true and returns false otherwise.

---

## 3. Time Series (43)

### `days_from_last_change(x)`
- **Permission:** Base
- **Scope:** Combo, Regular
- **Description:** Amount of days since last change of x.

### `hump(x, hump = 0.01)`
- **Permission:** Base
- **Scope:** Combo, Regular
- **Description:** Limits amount and magnitude of changes in input (thus reducing turnover).

### `hump_decay(x, p=0)`
- **Permission:** Genius
- **Scope:** Combo, Regular
- **Description:** Helps to ignore the values that changed too little corresponding to previous ones.

### `inst_tvr(x, d)`
- **Permission:** Genius
- **Scope:** Combo, Regular
- **Description:** Total trading value / Total holding value in the past d days.

### `jump_decay(x, d, sensitivity=0.5, force=0.1)`
- **Permission:** Genius
- **Scope:** Combo, Regular
- **Description:** If there is a huge jump in current data compare to previous one, apply force: jump_decay(x) = abs(x-ts_delay(x, 1)) > sensitivity * ts_stddev(x,d) ? ts_delay(x,1) + ts_delta(x, 1) * force: x.

### `kth_element(x, d, k)`
- **Permission:** Base
- **Scope:** Combo, Regular
- **Description:** Returns K-th value of input by looking through lookback days. Can be used to backfill missing data if k=1.

### `last_diff_value(x, d)`
- **Permission:** Base
- **Scope:** Combo, Regular
- **Description:** Returns last x value not equal to current x value from last d days.

### `ts_arg_max(x, d)`
- **Permission:** Base
- **Scope:** Combo, Regular
- **Description:** Returns the relative index of the max value in the time series for the past d days. (Current day = 0, Previous day = 1).

### `ts_arg_min(x, d)`
- **Permission:** Base
- **Scope:** Combo, Regular
- **Description:** Returns the relative index of the min value in the time series for the past d days.

### `ts_av_diff(x, d)`
- **Permission:** Base
- **Scope:** Combo, Regular
- **Description:** Returns `x - tsmean(x, d)`, but deals with NaNs carefully (NaNs are ignored during mean computation).

### `ts_backfill(x,lookback = d, k=1, ignore="NAN")`
- **Permission:** Base
- **Scope:** Combo, Regular
- **Description:** Replaces NaN values with the last available non-NaN value from the past d days.

### `ts_co_kurtosis(y, x, d)`
- **Permission:** Genius
- **Scope:** Combo, Regular
- **Description:** Returns cokurtosis of y and x for the past d days.

### `ts_co_skewness(y, x, d)`
- **Permission:** Genius
- **Scope:** Combo, Regular
- **Description:** Returns coskewness of y and x for the past d days.

### `ts_corr(x, y, d)`
- **Permission:** Base
- **Scope:** Combo, Regular
- **Description:** Returns Pearson correlation of x and y for the past d days.

### `ts_count_nans(x ,d)`
- **Permission:** Base
- **Scope:** Combo, Regular
- **Description:** Returns the number of NaN values in x for the past d days.

### `ts_covariance(y, x, d)`
- **Permission:** Base
- **Scope:** Combo, Regular
- **Description:** Returns covariance of y and x for the past d days.

### `ts_decay_exp_window(x, d, factor = f)`
- **Permission:** Genius
- **Scope:** Combo, Regular
- **Description:** Returns exponential decay of x with smoothing factor for the past d days. (0 < factor < 1).

### `ts_decay_linear(x, d, dense = false)`
- **Permission:** Base
- **Scope:** Combo, Regular
- **Description:** Returns the linear decay on x for the past d days. Dense=false treats NaN as 0.

### `ts_delay(x, d)`
- **Permission:** Base
- **Scope:** Combo, Regular
- **Description:** Returns x value d days ago.

### `ts_delta(x, d)`
- **Permission:** Base
- **Scope:** Combo, Regular
- **Description:** Returns `x - ts_delay(x, d)`.

### `ts_delta_limit(x, y, limit_volume=0.1)`
- **Permission:** Genius
- **Scope:** Combo, Regular
- **Description:** Limit the change in the Alpha position x between dates to a specified fraction of y.

### `ts_entropy(x,d)`
- **Permission:** Genius
- **Scope:** Combo, Regular
- **Description:** Calculates the probability distribution and information entropy via a histogram for input in the past d days.

### `ts_max(x, d)`
- **Permission:** Base
- **Scope:** Combo, Regular
- **Description:** Returns max value of x for the past d days.

### `ts_mean(x, d)`
- **Permission:** Base
- **Scope:** Combo, Regular
- **Description:** Returns average value of x for the past d days.

### `ts_min(x, d)`
- **Permission:** Base
- **Scope:** Combo, Regular
- **Description:** Returns min value of x for the past d days.

### `ts_median(x, d)`
- **Permission:** Genius
- **Scope:** Combo, Regular
- **Description:** Returns median value of x for the past d days.

### `ts_min_diff(x, d)`
- **Permission:** Genius
- **Scope:** Combo, Regular
- **Description:** Returns `x - ts_min(x, d)`.

### `ts_min_max_cps(x, d, f = 2)`
- **Permission:** Genius
- **Scope:** Combo, Regular
- **Description:** Returns `(ts_min(x, d) + ts_max(x, d)) - f * x`.

### `ts_min_max_diff(x, d, f = 0.5)`
- **Permission:** Genius
- **Scope:** Combo, Regular
- **Description:** Returns `x - f * (ts_min(x, d) + ts_max(x, d))`.

### `ts_percentage(x, d, percentage=0.5)`
- **Permission:** Genius
- **Scope:** Combo, Regular
- **Description:** Returns percentile value of x for the past d days. (percentage=0.5 returns median).

### `ts_product(x, d)`
- **Permission:** Base
- **Scope:** Combo, Regular
- **Description:** Returns product of x for the past d days. Can be used for geometric mean.

### `ts_quantile(x,d, driver="gaussian" )`
- **Permission:** Base
- **Scope:** Combo, Regular
- **Description:** Calculates ts_rank and applies inverse CDF from driver distribution (gaussian, uniform, cauchy).

### `ts_rank(x, d, constant = 0)`
- **Permission:** Base
- **Scope:** Combo, Regular
- **Description:** Rank the values of x for each instrument over the past d days, then return the rank of the current value + constant.

### `ts_regression(y, x, d, lag = 0, rettype = 0)`
- **Permission:** Base
- **Scope:** Combo, Regular
- **Description:** Returns parameters related to linear regression of Y on X over d days. rettype controls output (0=Error Term, 2=Slope, 6=R-Square, etc.).

### `ts_scale(x, d, constant = 0)`
- **Permission:** Base
- **Scope:** Combo, Regular
- **Description:** Returns `(x - ts_min(x, d)) / (ts_max(x, d) - ts_min(x, d)) + constant`.

### `ts_skewness(x, d)`
- **Permission:** Genius
- **Scope:** Combo, Regular
- **Description:** Return skewness of x for the past d days.

### `ts_std_dev(x, d)`
- **Permission:** Base
- **Scope:** Combo, Regular
- **Description:** Returns standard deviation of x for the past d days.

### `ts_step(1)`
- **Permission:** Base
- **Scope:** Combo, Regular
- **Description:** Returns days' counter.

### `ts_sum(x, d)`
- **Permission:** Base
- **Scope:** Combo, Regular
- **Description:** Sum values of x for the past d days.

### `ts_target_tvr_decay(x, lambda_min=0, lambda_max=1, target_tvr=0.1)`
- **Permission:** Genius
- **Scope:** Combo, Regular
- **Description:** Tune "ts_decay" to have a turnover equal to a certain target.

### `ts_target_tvr_delta_limit(x, y, lambda_min=0, lambda_max=1, target_tvr=0.1)`
- **Permission:** Genius
- **Scope:** Combo, Regular
- **Description:** Tune "ts_delta_limit" to have a turnover equal to a certain target with optimization weight range between lambda_min, lambda_max. Also, please be aware of the scaling for x and y. Besides setting y as adv20 or volume related data, you can also set y as a constant.

### `ts_vector_neut(x,y,d)`
- **Permission:** Genius
- **Scope:** Combo, Regular
- **Description:** Returns `x - ts_vector_proj(x,y,d)`.

### `ts_zscore(x, d)`
- **Permission:** Base
- **Scope:** Combo, Regular
- **Description:** Time series Z-score: `(x - tsmean(x,d)) / tsstddev(x,d)`.

---

## 4. Cross Sectional (13)

### `generalized_rank(open, m=1)`
- **Permission:** Genius
- **Scope:** Combo, Regular
- **Description:** Adds difference between instrument values raised to power m to the rank.

### `normalize(x, useStd = false, limit = 0.0)`
- **Permission:** Base
- **Scope:** Combo, Regular
- **Description:** Subtracts the mean of all valid alpha values from each element. Optional standardization (useStd) and limiting.

### `quantile(x, driver = gaussian, sigma = 1.0)`
- **Permission:** Base
- **Scope:** Combo, Regular
- **Description:** Rank raw vector, shift, and apply distribution (gaussian, cauchy, uniform).

### `rank(x, rate=2)`
- **Permission:** Base
- **Scope:** Combo, Regular
- **Description:** Ranks input among all instruments returning value between 0.0 and 1.0. rate=0 for precise sort.

### `regression_neut(y, x)`
- **Permission:** Genius
- **Scope:** Combo, Regular
- **Description:** Cross-sectional regression Y (target) on X. Returns `Y - (a + b*X)`.

### `regression_proj(y, x)`
- **Permission:** Genius
- **Scope:** Combo, Regular
- **Description:** Cross-sectional regression Y (target) on X. Returns `a + b*X`.

### `scale(x, scale=1, longscale=1, shortscale=1)`
- **Permission:** Base
- **Scope:** Combo, Regular
- **Description:** Scales input to booksize. Ensures sum of abs(x) over all instruments equals 1 (if scale=1).

### `scale_down(x,constant=0)`
- **Permission:** Base
- **Scope:** Combo, Regular
- **Description:** Scales all values in each day proportionately between 0 and 1 such that minimum value maps to 0 and maximum value maps to 1. Constant is the offset by which final result is subtracted.

### `truncate(x,maxPercent=0.01)`
- **Permission:** Genius
- **Scope:** Combo, Regular
- **Description:** Truncates all values of x to maxPercent (decimal notation).

### `vector_neut(x, y)`
- **Permission:** Genius
- **Scope:** Combo, Regular
- **Description:** Neutralizes vector x to y (orthogonalization).

### `vector_proj(x, y)`
- **Permission:** Genius
- **Scope:** Combo, Regular
- **Description:** Returns vector projection of x onto y.

### `winsorize(x, std=4)`
- **Permission:** Base
- **Scope:** Combo, Regular
- **Description:** Winsorizes x to restrict values between lower and upper limits (multiples of std).

### `zscore(x)`
- **Permission:** Base
- **Scope:** Combo, Regular
- **Description:** Cross-sectional Z-score: `(x - mean(x)) / std(x)`.

---

## 5. Vector (11)

### `vec_avg(x)`
- **Permission:** Base
- **Scope:** Combo, Regular
- **Description:** Taking mean of the vector field x.

### `vec_count(x)`
- **Permission:** Genius
- **Scope:** Combo, Regular
- **Description:** Number of elements in vector field x.

### `vec_filter(vec, value=nan)`
- **Permission:** Genius
- **Scope:** Combo, Regular
- **Description:** Filter vector by values. Output remains vector data.

### `vec_ir(x)`
- **Permission:** Genius
- **Scope:** Combo, Regular
- **Description:** Information Ratio (Mean / Standard Deviation) of vector field x.

### `vec_norm(x)`
- **Permission:** Genius
- **Scope:** Combo, Regular
- **Description:** Sum of all absolute values of vector field x.

### `vec_powersum(x,constant=2)`
- **Permission:** Genius
- **Scope:** Combo, Regular
- **Description:** Sum of power of vector field x.

### `vec_max(x)`
- **Permission:** Genius
- **Scope:** Combo, Regular
- **Description:** Maximum value form vector field x.

### `vec_min(x)`
- **Permission:** Genius
- **Scope:** Combo, Regular
- **Description:** Minimum value form vector field x.

### `vec_range(x)`
- **Permission:** Genius
- **Scope:** Combo, Regular
- **Description:** Difference between maximum and minimum element in vector field x.

### `vec_stddev(x)`
- **Permission:** Genius
- **Scope:** Combo, Regular
- **Description:** Standard Deviation of vector field x.

### `vec_sum(x)`
- **Permission:** Base
- **Scope:** Combo, Regular
- **Description:** Sum of vector field x.

---

## 6. Transformational (5)

### `bucket(rank(x), range="0, 1, 0.1" or buckets = "2,5,6,7,10")`
- **Permission:** Base
- **Scope:** Combo, Regular
- **Description:** Convert float values into indexes for user-specified buckets. Useful for creating group values.

### `generate_stats(alpha)`
- **Permission:** Base
- **Scope:** Combo
- **Description:** Calculates Alpha statistics for each day in the IS period.

### `left_tail(x, maximum = 0)`
- **Permission:** Genius
- **Scope:** Combo, Regular
- **Description:** NaN everything greater than maximum.

### `right_tail(x, minimum = 0)`
- **Permission:** Genius
- **Scope:** Combo, Regular
- **Description:** NaN everything less than minimum.

### `trade_when(x, y, z)`
- **Permission:** Base
- **Scope:** Combo, Regular
- **Description:** Change Alpha values only under a specified condition (x), otherwise hold or exit (z).

---

## 7. Group (15)

### `combo_a(alpha, nlength = 250, mode = 'algo1')`
- **Permission:** Base
- **Scope:** Combo
- **Description:** Combines multiple alpha signals into a single weighted output by balancing each alpha's historical return with its variability.

### `group_backfill(x, group, d, std = 4.0)`
- **Permission:** Base
- **Scope:** Combo, Regular
- **Description:** Backfills NaN values using the winsorized mean of the group over the last d days.

### `group_cartesian_product(g1, g2)`
- **Permission:** Genius
- **Scope:** Combo, Regular
- **Description:** Merge two groups into one group (Cartesian product of indices).

### `group_extra(x, weight, group)`
- **Permission:** Genius
- **Scope:** Combo, Regular
- **Description:** Replaces NaN values by their corresponding group means.

### `group_max(x, group)`
- **Permission:** Genius
- **Scope:** Combo, Regular
- **Description:** Maximum of x for all instruments in the same group.

### `group_mean(x, weight, group)`
- **Permission:** Base
- **Scope:** Combo, Regular
- **Description:** All elements in group equals to the mean.

### `group_min(x, group)`
- **Permission:** Genius
- **Scope:** Combo, Regular
- **Description:** Minimum of x for all instruments in the same group.

### `group_median(x, group)`
- **Permission:** Genius
- **Scope:** Combo, Regular
- **Description:** All elements in group equals to the median value of the group.

### `group_neutralize(x, group)`
- **Permission:** Base
- **Scope:** Combo, Regular
- **Description:** Neutralizes Alpha against groups (element subtracted by group mean).

### `group_normalize(x, group, constantCheck=False, tolerance=0.01, scale=1)`
- **Permission:** Genius
- **Scope:** Combo, Regular
- **Description:** Normalizes input such that each group's absolute sum is 1.

### `group_percentage(x, group, percentage=0.5)`
- **Permission:** Genius
- **Scope:** Combo, Regular
- **Description:** All elements in group equals to the value over the percentage of the group.

### `group_rank(x, group)`
- **Permission:** Base
- **Scope:** Combo, Regular
- **Description:** Ranks stocks within their specified group (0.0 to 1.0).

### `group_scale(x, group)`
- **Permission:** Base
- **Scope:** Combo, Regular
- **Description:** Normalizes the values in a group to be between 0 and 1.

### `group_vector_neut(x,y,g)`
- **Permission:** Genius
- **Scope:** Combo, Regular
- **Description:** Similar to vector_neut(x, y) but x neutralize to y for each group g.

### `group_zscore(x, group)`
- **Permission:** Base
- **Scope:** Combo, Regular
- **Description:** Calculates group Z-score: `(data - group_mean) / group_stddev`.

---

## 8. Special (4)

### `in`
- **Permission:** Base
- **Scope:** Selection
- **Description:** in keyword.

### `inst_pnl(x)`
- **Permission:** Genius
- **Scope:** Combo, Regular
- **Description:** Generate pnl per instruments. Utilizes pv1 dataset.

### `self_corr(input)`
- **Permission:** Base
- **Scope:** Combo
- **Description:** Returns correlation matrix of input with itself over lookback window.

### `universe_size`
- **Permission:** Base
- **Scope:** Selection
- **Description:** Returns the size of the universe.

---

## 9. Reduce (14)

### `reduce_avg(input, threshold=0)`
- **Permission:** Base
- **Scope:** Combo
- **Description:** Average of non-NAN elements of d(..., :).

### `reduce_choose(input, nth, ignoreNan=true)`
- **Permission:** Base
- **Scope:** Combo
- **Description:** Choose the 'nth' element in the array.

### `reduce_count(input, threshold)`
- **Permission:** Base
- **Scope:** Combo
- **Description:** Count the number of element of d(..., :) > threshold.

### `reduce_ir(input)`
- **Permission:** Base
- **Scope:** Combo
- **Description:** Information Ratio of values in the array.

### `reduce_kurtosis(input)`
- **Permission:** Base
- **Scope:** Combo
- **Description:** Kurtosis of values in the array.

### `reduce_max(input)`
- **Permission:** Base
- **Scope:** Combo
- **Description:** Maximum of elements of d(..., :).

### `reduce_min(input)`
- **Permission:** Base
- **Scope:** Combo
- **Description:** Minimum of elements of d(..., :).

### `reduce_norm(input)`
- **Permission:** Base
- **Scope:** Combo
- **Description:** Absolute sum of number of element of d(..., :).

### `reduce_percentage(input, percentage=0.5)`
- **Permission:** Base
- **Scope:** Combo
- **Description:** Return the value of percentage in the sorted array.

### `reduce_powersum(input, constant=2, precise=false)`
- **Permission:** Base
- **Scope:** Combo
- **Description:** Sum of power, sum(power(x, constant)).

### `reduce_range(input)`
- **Permission:** Base
- **Scope:** Combo
- **Description:** Return the range of values in the array.

### `reduce_skewness(input)`
- **Permission:** Base
- **Scope:** Combo
- **Description:** Skewness of values in the array.

### `reduce_stddev(input, threshold=0)`
- **Permission:** Base
- **Scope:** Combo
- **Description:** Standard deviation of values in the array.

### `reduce_sum(input)`
- **Permission:** Base
- **Scope:** Combo
- **Description:** Sum the number of element of d(..., :).