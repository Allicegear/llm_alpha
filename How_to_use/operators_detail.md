# WorldQuant Brain Operators Details

## Arithmetic Operators

1. `abs(x)`
    - **Scope:** Combo, Regular, Selection
    - **Description:** Returns the absolute value of a number, removing any negative sign.
    - **Manual:** The absolute value is often used to ensure that only the size of a value is considered, not its direction.

2. `add(x, y, filter = false)`
    - **Alias:** `x + y`
    - **Scope:** Combo, Regular, Selection
    - **Description:** Adds two or more inputs element wise. Set filter=true to treat NaNs as 0 before summing.
    - **Manual:** The add operator performs element-wise addition on two or more inputs. If the optional filter parameter is set to true, any NaN values in the inputs are treated as zero before the addition.
        - **Example:** Suppose you have two vectors:
            - x = [1, NaN, 3]
            - y = [4, 5, NaN]
            - add(x, y) returns [1+4, NaN+5, 3+NaN] = [5, NaN, NaN]
            - add(x, y, filter=true) returns [1+4, 0+5, 3+0] = [5, 5, 3]
        - **Tips:** Use filter=true to treat NaNs as zeros. This can improve coverage and performance without the need to backfill the data.

3. `densify(x)`
    - **Scope:** Combo, Regular
    - **Description:** Converts a grouping field of many buckets into lesser number of only available buckets so as to make working with grouping fields computationally efficient.
    - **Manual:** This operator converts a grouping field with many buckets into a lesser number of only the available buckets, making working with grouping fields computationally efficient. The example below will clarify the implementation.
        - **Example:** Say a grouping field is provided as an integer (e.g., industry: tech -> 0, airspace -> 1, ...) and for a certain date, we have instruments with grouping field values among {0, 1, 2, 99}. Instead of creating 100 buckets and keeping 96 of them empty, it is better to just create 4 buckets with values {0, 1, 2, 3}. So, if the number of unique values in x is n, densify maps those values between 0 and (n-1). The order of magnitude need not be preserved.

4. `divide(x, y)`
    - **Alias:** `x / y`
    - **Scope:** Combo, Regular, Selection
    - **Description:** x / y.

5. `inverse(x)`
    - **Scope:** Combo, Regular, Selection
    - **Description:** 1 / x.

6. `log(x)`
    - **Scope:** Combo, Regular, Selection
    - **Description:** Calculates the natural logarithm of the input value. Commonly used to transform data that has positive values.
    - **Manual:** The log(x) operator computes the natural logarithm (base e) of the input x. This transformation is widely used in finance to normalize data, reduce skewness, or convert multiplicative relationships into additive ones. The input x should be positive, as the logarithm is undefined for zero or negative values.

7. `max(x, y, ..)`
    - **Scope:** Combo, Regular, Selection
    - **Description:** Maximum value of all inputs. At least 2 inputs are required.

8. `min(x, y, ..)`
    - **Scope:** Combo, Regular, Selection
    - **Description:** Minimum value of all inputs. At least 2 inputs are required.

9. `multiply(x ,y, ... , filter=false)`
    - **Alias:** `x * y`
    - **Scope:** Combo, Regular, Selection
    - **Description:** Multiplies two or more inputs element wise. Set filter=true to treat NaNs as 0 before multiplication.
    - **Manual:** Computes the product of all inputs. You can pass any number of scalars or series: multiply(a, b, c) = a × b × c. When filter=true, NaNs are replaced with 1 before the product; when false, any NaN propagates to the result.
        - **Example:** 
            - multiply(5, NaN, filter=false) = NaN
            - multiply(5, NaN, filter=true) = 5 × 1 = 5

10. `power(x, y)`
    - **Alias:** `x ^ y`
    - **Scope:** Combo, Regular, Selection
    - **Description:** x ^ y.
    - **Manual:** 
        - **Tips:**power (x, y) operator can be used to implement popular mathematical functions. For example, sigmoid(close) can be implemented using power(x) as: 1/(1+ power(2.7182, -close)).

11. `reverse(x)`
    - **Scope:** Combo, Regular, Selection
    - **Description:**  - x.

12. `sign(x)`
    - **Scope:** Combo, Regular, Selection
    - **Description:**  Returns the sign of a number: +1 for positive, -1 for negative, and 0 for zero. If the input is NaN, returns NaN. Input: Value of 7 instruments at day t: (2, -3, 5, 6, 3, NaN, -10) Output: (1, -1, 1, 1, 1, NaN, -1).
    - **Manual:** The sign(x) operator determines whether a value is positive, negative, or zero. It is commonly used to quickly identify the direction of a value. If the input is not a number (NaN), the result will also be NaN.

13. `signed_power(x, y)`
    - **Scope:** Combo, Regular, Selection
    - **Description:**  x raised to the power of y such that final result preserves sign of x.
    - **Manual:** x raised to the power of y such that final result preserves sign of x. For power of 2, x ^ y will be a parabola but signed_power(x, y) will be odd and one-to-one function (unique value of x for certain value of signed_power(x, y)) unlike parabola.
        - **Exmple:** 
            - signed_power(3, 2) = 9
            - signed_power(-9, 0.5) = -3

14. `sqrt(x)`
    - **Scope:** Combo, Regular, Selection
    - **Description:**  Returns the non negative square root of x. Equivalent to power(x, 0.5); for signed roots use signed_power(x, 0.5).
    - **Manual:** sqrt(x) = power(x, 0.5) for x ≥ 0. It reduces skew and compresses large positive values while keeping order. It returns NaN for x < 0. If you need a root‑like transform that keeps the sign for negative inputs, use signed_power(x, 0.5), which computes sign(x)*sqrt(abs(x)).
        - **Tips:** sqrt(-4) = NaN (use signed_power(-4, 0.5) = -2 for a sign‑preserving root)        

15. `subtract(x, y, filter=false)`
    - **Alias:** `x - y`
    - **Scope:** Combo, Regular, Selection
    - **Description:** Subtracts inputs left to right: x ? y ? … Supports two or more inputs. Set filter=true to treat NaNs as 0 before subtraction.
    - **Manual:** Performs element‑wise subtraction on scalars or series. You can pass more than two inputs; evaluation is left‑to‑right: subtract(a, b, c) = ((a − b) − c). When filter=true, any NaN in the inputs is replaced with 0 before subtraction; when false, NaNs propagate.
        - **Exmple:** 
            - subtract(NaN, 5, filter=true) = 0 − 5 = −5
            - subtract(NaN, 5, filter=false) = NaN

16. `to_nan(x, value=0, reverse=false)`
    - **Scope:** Combo, Regular, Selection
    - **Description:**  Convert value to NaN or NaN to value if reverse=true.

## Logical Operators

1. `and(input1, input2)`
    - **Scope:** Combo, Regular, Selection
    - **Description:**  Returns 1 ('true') if both inputs are 1 ('true'). Otherwise, returns 0 ('false').

2. `if_else(input1, input2, input 3)`
    - **Scope:** Combo, Regular, Selection
    - **Description:**  The if_else operator returns one of two values based on a condition. If the condition is true, it returns the first value; if false, it returns the second value.
    - **Manual:** The if_else operator lets you choose between two expressions depending on whether a condition is met. This is useful for creating Alphas that react to market events, data thresholds, or other logical rules.
        - **Syntax:** `if_else(event_condition, Alpha_expression_1, Alpha_expression_2)`
        - **Exmple:** Event = volume > adv20; if_else(Event, 2 * ts_delta(close, 3), ts_delta(close, 3))
            - When volume spikes above its 20‑day average, your position change doubles; otherwise it stays normal.

3. `input1 < input2`
    - **Scope:** Combo, Regular, Selection
    - **Description:**  Returns 1 ('true') if input1 is a smaller than input2. Otherwise, returns 0 ('false').

4. `input1 <= input2`
    - **Scope:** Combo, Regular, Selection
    - **Description:**  Returns 1 ('true') if input1 is a smaller or the same as input2. Otherwise, returns 0 ('false').

5. `input1 == input2`
    - **Scope:** Combo, Regular, Selection
    - **Description:**  Returns 1 ('true') if input1 and input2 are the same. Otherwise, returns 0 ('false').

6. `input1 > input2`
    - **Scope:** Combo, Regular, Selection
    - **Description:**  Returns 1 ('true') if input1 is a larger than input2. Otherwise, returns 0 ('false').

7. `input1 >= input2`
    - **Scope:** Combo, Regular, Selection
    - **Description:**  Returns 1 ('true') if input1 is a larger or the same as input2. Otherwise, returns 0 ('false').

8. `input1 != input2`
    - **Scope:** Combo, Regular, Selection
    - **Description:**  Returns 1 ('true') if input1 and input2 are different numbers. Otherwise, returns 0 ('false').

9. `is_nan(input)`
    - **Scope:** Combo, Regular, Selection
    - **Description:**  If (input == NaN) return 1 else return 0
    - **Manual:** is_nan(x) operator can be used to identify NaN values and replace them to a default value using if_else statement.

10. `not(x)`
    - **Scope:** Combo, Regular, Selection
    - **Description:**  Returns the logical negation of x. Returns 0 when x is 1 (‘true’) and 1 when x is 0 (‘false’).

11. `or(input1, input2)`
    - **Scope:** Combo, Regular, Selection
    - **Description:**  Returns 1 if either input is true (either input1 or input2 has a value of 1), otherwise it returns 0.

## Time Series Operators

1. `days_from_last_change(x)`
    - **Scope:** Combo, Regular
    - **Description:**  Amount of days since last change of x Input: Value of 1 instrument in past 7 days where first element is the latest: (2, 2, 2, 7, 5, 16, 1) Output: 3.

2. `hump(x, hump = 0.01)`
    - **Scope:** Combo, Regular
    - **Description:**  Limits amount and magnitude of changes in input (thus reducing turnover).
    - **Manual:** This operator limits the frequency and magnitude of changes in the Alpha (thus reducing turnover). If today's values show only a minor change (not exceeding the Threshold) from yesterday's value, the output of the hump operator stays the same as yesterday. If the change is bigger than the limit, the output is yesterday's value plus the limit in the direction of the change.
        - **Example:** Input: Value of 1 instrument in past 2 days where first element is the latest: (2, 5), hump: 0.1, assuming limit: 1.5.
            - Output: 3.5 (from 5-1.5 instead of 2 as abs(2 - 5) greater than limit).
        - **Tips:** This operator may help reduce turnover and drawdown.

3. `jump_decay(x, d, sensitivity=0.5, force=0.1)`
    - **Scope:** Combo, Regular
    - **Description:**  If there is a huge jump in current data compare to previous one. apply force: jump_decay(x) = abs(x-ts_delay(x, 1)) > sensitivity * ts_stddev(x,d) ? ts_delay(x,1) + ts_delta(x, 1) * force: x. If stddev enabled, jump threshold will be calculated as sensitivity * stddev otherwise it is sensitivity.
    - **Manual:** 
        - **Example:** Input: Value of 1 instrument in past 2 days: (2, 4), d: 6, sensitivity: 0.5, force: 0.1 where first element is the latest.
            - Output: 4 + (-2 * 0.1) = 3.8.

4. `kth_element(x, d, k)`
    - **Scope:** Combo, Regular
    - **Description:**  Returns K-th valid value of input by looking through lookback days. This operator can be used to backfill missing data if k=1.
    - **Manual:** Returns k-th value of input by looking through lookback days while ignoring space separated scalars in ignore list. This operator is also known as backfill operator as it can be used to backfill missing data. 
        - **Example:** Input: Value of 1 instrument in past 7 days where first element is the latest: (2, 3, 5, 6, 3, 8, 10), k: 2, d: 6.
            - Output: Value at index 2 = 5.
        - **Tips:** Ignore parameter is used to provide list of separated scalars to ignore from counting. E.g. kth_element(sales/assets,252,k="1",ignore="NAN 0").

5. `last_diff_value(x, d)`
    - **Scope:** Combo, Regular
    - **Description:**  Returns last x value not equal to current x value from last d days.

6. `ts_arg_max(x, d)`
    - **Scope:** Combo, Regular
    - **Description:**  Returns the relative index of the max value in the time series for the past d days. If the current day has the max value for the past d days, it returns 0. If previous day has the max value for the past d days, it returns 1.
    - **Manual:** It returns the relative index of the max value in the time series for the past d days. If the current day has the max value for the past d days, it returns 0. If previous day has the max value for the past d days, it returns 1.
        - **Example:** If d = 6 and values for past 6 days are [6,2,8,5,9,4] with first element being today’s value then max value is 9 and it is present 4 days before today. Hence, ts_arg_max(x, d) = 4.

7. `ts_arg_min(x, d)`
    - **Scope:** Combo, Regular
    - **Description:**  Returns the relative index of the min value in the time series for the past d days; If the current day has the min value for the past d days, it returns 0; If previous day has the min value for the past d days, it returns 1.
    - **Manual:** It returns the relative index of the min value in the time series for the past d days. If the current day has the min value for the past d days, it returns 0. If previous day has the min value for the past d days, it returns 1.
        - **Example:** If d = 6 and values for past 6 days are [6,2,8,5,9,4] with first element being today’s value then min value is 2 and it is present 1 days before today. Hence, ts_arg_min(x, d) = 1.

8. `ts_av_diff(x, d)`
    - **Scope:** Combo, Regular
    - **Description:**  Returns x - tsmean(x, d), but deals with NaNs carefully. That is NaNs are ignored during mean computation.
    - **Manual:** This operator returns x – ts_mean(x, d), but it deals with NaNs carefully.
        - **Example:** If d = 6 and values for past 6 days are [6,2,8,5,9,NaN] then ts_mean(x,d) = 6 since NaN are ignored from mean computation. Hence, ts_av_diff(x,d) = 6-6 = 0

9. `ts_backfill(x, d)`
    - **Scope:** Combo, Regular
    - **Description:**  Returns the first valid value of the input x by looking through lookback days (d). This operator can be used to backfill missing data.
    - **Manual:** The ts_backfill operator replaces NaN values with the last available non-NaN value. If the input value of the data field x is NaN, the ts_backfill operator will check available input values of the same data field for the past d number of days, and output the most recent available non-NaN input value. If the k parameter is set, then the ts_backfill operator will output the kth most recent available non-NaN input value.
        - **Syntax:** `ts_backfill(x,lookback = d, k=1, ignore="NAN")`
        - **Example:** ts_backfill(x, 252).
            - If the input value for data field x = non-NaN, then output = x.
            - If the input value for data field x = NaN, then output = most recent available non-NaN input value for x in the past 252 days
        - **Tips:** This operator improves weight coverage and may help to reduce drawdown risk.

10. `ts_corr(x, y, d)`
    - **Scope:** Combo, Regular
    - **Description:** Returns correlation of x and y for the past d days.
    - **Manual:** Pearson correlation measures the linear relationship between two variables. It's most effective when the variables are normally distributed and the relationship is linear.
        - **Example:** Input: Value of 1 instrument in past 7 days: (2, 3, 5, 6, 3, 8, 10), and another instrument value in past 7 days: (100, 190, 150, 180, 210, 220, 240), d = 7, where first element is the latest.
            - Output: 0.6891 (Pearson correlation coefficient formula).

11. `ts_count_nans(x ,d)`
    - **Scope:** Combo, Regular
    - **Description:** Returns the number of NaN values in x for the past d days Input: Value of 1 instrument in past 4 days where first element is the latest: (100, NaN, NaN, 200), d: 4 Output: Number of NaN in 4 days = 2.

12. `ts_covariance(y, x, d)`
    - **Scope:** Combo, Regular
    - **Description:** Returns covariance of y and x for the past d days.

13. `ts_decay_linear(x, d, dense = false)`
    - **Scope:** Combo, Regular
    - **Description:** Returns the linear decay on x for the past d days. Dense parameter=false means operator works in sparse mode and we treat NaN as 0. In dense mode we do not.
    - **Manual:** Returns the linear decay on x for the past d days. Dense parameter=false means operator works in sparse mode and we treat NaN as 0. In dense mode we do not. Data smoothing techniques like linear decay reduce noise in time-series data by applying a decay factor to older observations, which helps to stabilize the dataset.
        - **Example:** For a stock with the following prices over the last 5 days.
            - Day 0: 30 (outlier); Day -1: 5; Day -2: 4; Day -3: 5; Day -4: 6.
            - The calculation would be: 
                - Numerator = (30⋅5)+(5⋅4)+(4⋅3)+(5⋅2)+(6⋅1)=150+20+12+10+6=198
                - Denominator=5+4+3+2+1=15
                - Weighted Average=198/15=13.2
            - The weighted average value of 13.2 is used instead of the outlier value of 30 for assigning weight.
        - **Tips:** This operator improves turnover and drawdown.

14. `ts_delay(x, d)`
    - **Scope:** Combo, Regular
    - **Description:** Returns x value d days ago Input: Value of 1 instrument in past 7 days where first element is the latest: (2, 3, 5, 6, 3, 8, 10), d: 6 Output: Value 6 days ago = 10.

15. `ts_delta(x, d)`
    - **Scope:** Combo, Regular
    - **Description:** Returns x - ts_delay(x, d) Input: Value of 1 instrument in past 7 days where first element is the latest: (2, 3, 5, 6, 3, 8, 10), d: 6 Output: Value today – value 6 days ago = 2 - 10 = -8.

16. `ts_max(x, d)`
    - **Scope:** Combo, Regular
    - **Description:** Returns max value of x for the past d days.

17. `ts_mean(x, d)`
    - **Scope:** Combo, Regular
    - **Description:** Returns average value of x for the past d days.

18. `ts_min(x, d)`
    - **Scope:** Combo, Regular
    - **Description:** Returns min value of x for the past d days.

19. `ts_product(x, d)`
    - **Scope:** Combo, Regular
    - **Description:** Returns product of x for the past d days.
    - **Manual:** ts_product(x, d) can be used to calculate geometric mean of data fields. The geometric mean is generally a better method for averaging rates of return, growth rates of fundamentals.
        - **Example:** geometric mean of daily stock returns for past 10 days can be calculated as:
            - Input: Value of 1 instrument in past 7 days where first element is the latest: (2, 3, 5, 6, 3, 8, 10), d: 7.
            - Output: 2 * 3 * 5 * 6 * 3 * 8 * 10 = 43,200

20. `ts_quantile(x,d, driver="gaussian" )`
    - **Scope:** Combo, Regular
    - **Description:** It calculates ts_rank and apply to its value an inverse cumulative density function from driver distribution. Possible values of driver (optional ) are "gaussian", "uniform", "cauchy" distribution where "gaussian" is the default. Input: Value of 1 instrument in past 7 days where first element is the latest: (8, 10, 4, 6, 5, 3, 2), d: 7, driver: ’gaussian’ Output: quantile = 0.82 from SD = 2.82, mean = 5.43, zscore = 0.911.

21. `ts_rank(x, d, constant = 0)`
    - **Scope:** Combo, Regular
    - **Description:** Rank the values of x for each instrument over the past d days, then return the rank of the current value + constant. If not specified, by default, constant = 0. Input: Value of 1 instrument in past 3 days where first element is the latest: (100, 0, 200), d: 3 Output: 0.5.

22. `ts_regression(y, x, d, lag = 0, rettype = 0)`
    - **Scope:** Combo, Regular
    - **Description:** Returns various parameters related to regression function.
    - **Manual:** Returns parameters related to linear regression of Y on X over d days. rettype controls output (0=Error Term, 2=Slope, 6=R-Square, etc.).
        - **Example:** ts_regression(est_netprofit, est_netdebt, 252, lag = 0, rettype = 2).
            - Taking the data from the past 252 trading days (1 year), return the β coefficient from the equation when estimating the est_netprofit using the est_netdebt

23. `ts_scale(x, d, constant = 0)`
    - **Scope:** Combo, Regular
    - **Description:** Returns (x - ts_min(x, d)) / (ts_max(x, d) - ts_min(x, d)) + constant. This operator is similar to scale down operator but acts in time series space.
    - **Manual:** This operator returns (x – ts_min(x, d)) / (ts_max(x, d) – ts_min(x, d)) + constant.This operator is similar to scale down operator but acts in time series space.
        - **Example:** If d = 6 and values for last 6 days are [6,2,8,5,9,4] with first element being today’s value, ts_min(x,d) = 2, ts_max(x,d) = 9.
            - ts_scale(x,d,constant = 1) = 1 + (6-2)/(9-2) = 1.57.

24 `ts_std_dev(x, d)`
    - **Scope:** Combo, Regular
    - **Description:** Returns standard deviation of x for the past d days.

25 `ts_step(1)`
    - **Scope:** Combo, Regular
    - **Description:** Returns days' counter.

26 `ts_sum(x, d)`
    - **Scope:** Combo, Regular
    - **Description:** Sum values of x for the past d days.   

27 `ts_target_tvr_decay(x, lambda_min=0, lambda_max=1, target_tvr=0.1)`
    - **Scope:** Combo, Regular
    - **Description:** Tune "ts_decay" to have a turnover equal to a certain target, with optimization weight range between lambda_min, lambda_max.

28 `ts_target_tvr_delta_limit(x, y, lambda_min=0, lambda_max=1, target_tvr=0.1)`
    - **Scope:** Combo, Regular
    - **Description:** Tune "ts_delta_limit" to have a turnover equal to a certain target with optimization weight range between lambda_min, lambda_max. Also, please be aware of the scaling for x and y. Besides setting y as adv20 or volume related data, you can also set y as a constant.

29 `ts_zscore(x, d)`
    - **Scope:** Combo, Regular
    - **Description:** Z-score is a numerical measurement that describes a value's relationship to the mean of a group of values. Z-score is measured in terms of standard deviations from the mean: (x - tsmean(x,d)) / tsstddev(x,d). This operator may help reduce outliers and drawdown. Input: Value of 1 instrument in past 5 days where first element is the latest: (100, 0, 50, 60, 25), d: 5 Output: (100-47)/33.7 = 1.57 from SD: 33.7, mean = 47.

## Cross Sectional Operators

normalize(x, useStd = false, limit = 0.0):
Calculates the mean value of all valid alpha values for a certain date, then subtracts that mean from each element.
normalize(x, useStd = false, limit = 0.0).
This operator calculates the mean value of all valid alpha values for a certain date, then subtracts that mean from each element. If useStd= true, the operator calculates the standard deviation of the resulting values and divides each normalized element by it. If limit is not equal to 0.0, operator puts the limit of the resulting alpha values (between -limit to + limit).
Example:
If for a certain date, instrument value of certain input x is [3,5,6,2], mean = 4 and standard deviation = 1.82.
normalize(x, useStd = false, limit = 0.0) = [3-4,5-4,6-4,2-4] = [-1,1,2,-2].
normalize(x, useStd = true, limit = 0.0) = [-1/1.82,1/1.82,2/1.82,-2/1.82] = [-0.55,0.55,1.1,-1.1].

quantile(x, driver = gaussian, sigma = 1.0):
Rank the raw vector, shift the ranked Alpha vector, apply distribution (gaussian, cauchy, uniform). If driver is uniform, it simply subtract each Alpha value with the mean of all Alpha values in the Alpha vector.
quantile(x, driver = gaussian, sigma = 1.0).
Rank the input raw Alpha vector.
The ranked Alpha value would be within [0, 1].
Shift the ranked Alpha vector.
For every Alpha value in the ranked Alpha vector, it is shifted as: Alpha_value = 1/N + Alpha_value * (1 - 2/N), here assume there are N instruments with value in the Alpha vector. The shifted Alpha value would be within [1/N, 1-1/N].
Apply distribution for each Alpha value in the ranked Alpha vector using the specified driver. Driver can be one of "gaussian", "uniform", "cauchy".
Note : Sigma only affects the scale of the final value.
This operator may help reduce outliers.

rank(x, rate=2):
Ranks the input among all the instruments and returns an equally distributed number between 0.0 and 1.0. For precise sort, use the rate as 0.
rank(x, rate=2).
The Rank operator ranks the value of the input data x for the given stock among all instruments, and returns float numbers equally distributed between 0.0 and 1.0. When rate is set to 0, the sorting is done precisely. The default value of rate is 2.
This operator may help reduce outliers and drawdown while improving the Sharpe.
Example:
Rank(close); Rank (close, rate=0) # Sorts precisely.
X = (4,3,6,10,2) => Rank(x) = (0.5, 0.25, 0.75, 1, 0).

scale(x, scale=1, longscale=1, shortscale=1):
Scales input to booksize. We can also scale the long positions and short positions to separate scales by mentioning additional parameters to the operator.
scale (x, scale=1, longscale=1, shortscale=1).
The operator scales the input to the book size. We can optionally tune the book size by specifying the additional parameter 'scale=booksize_value'. We can also scale the long positions and short positions to separate scales by specifying additional parameters: longscale=long_booksize and shortscale=short_booksize. The default value of each leg of the scale is 0, which means no scaling, unless specified otherwise. Scale the alpha so that the sum of abs(x) over all instruments equals 1. To scale to a different book size, use Scale(x) * booksize.
This operator may help reduce outliers.
Please check examples for the application of the same.
Examples:
scale(returns, scale=4); scale (returns, scale= 1) + scale (close, scale=20); scale (returns, longscale=4, shortscale=3).

scale_down(x,constant=0):
Scales all values in each day proportionately between 0 and 1 such that minimum value maps to 0 and maximum value maps to 1. Constant is the offset by which final result is subtracted.
scale_down(x,constant=0).
Scales all values in each day proportionately between 0 and 1 such that minimum value maps to 0 and maximum value maps to 1. constant is the offset by which final result is subtracted.
Example:
If for a certain date, instrument values of certain input x is [15,7,0,20], max = 20 and min = 0.
scale_down(x,constant=0) = [(15-0)/20,(7-0)/20,(0-0)/’20,(20-0)/20] = [0.75,0.35,0,1].
scale_down(x,constant=1) = [0.75-1,0.35-1,0-1,1-1] = [-0.25,-0.65,-1,0].

vector_neut(x, y):
For given vectors x and y, it finds a new vector x* (output) such that x* is orthogonal to y.
vector_neut(x,y).
Input1 neutralize to input2.
For given vector A (i.e., input1) and B (i.e., input2), it finds a new vector A' (i.e., output) such that A' is orthogonal to B. It calculates projection of A onto B, and then subtracts projection vector from A to find the rejection vector (i.e., A') which is perpendicular to the B.
This operator may help reduce correlation, depending on the neutralization used.
Example:
Input: Vector of value of 3 instruments at day t, A: (1, 2, 3) and another vector of value of 3 instruments at day t, B: (3, 4, 5).
Output: (-0.56, -0.08, 0.4) from (−0.56)·3 + (−0.08)·4 + 0.4·5 = 0.
Show that the resulting vector is orthogonal to vector B.

winsorize(x, std=4):
Winsorizes x to make sure that all values in x are between the lower and upper limits, which are specified as multiple of std. Input: Value of 7 instruments at day t: (2, 4, 5, 6, 3, 8, 10), std: 1 Output: (2.81, 4, 5, 6, 3, 8, 8.03) from SD. = 2.61, mean = 5.42.

zscore(x):
Z-score is a numerical measurement that describes a value's relationship to the mean of a group of values. Z-score is measured in terms of standard deviations from the mean.
Z-score is a statistical tool that indicates how many standard deviations a data point lies from the average of a group of values. Essentially, it measures how unusual a data point is in relation to the mean, making it a handy tool for understanding deviation and comparison.
The formula to calculate a Z-score is:
Z-score = (x-mean(x))/std(x)
Where:
x is an individual data point
mean(x) is the average of the data set.
std(x) is the standard deviation of the data set.
By this definition, the mean of the Z-scores in a distribution is always 0, and the standard deviation is always 1.
A Z-score tells you how many standard deviations a particular data point is from the mean. If the Z-score is positive, the data point is above the mean, and if it's negative, it's below the mean.
Z-scores may be especially useful for normalizing and comparing different data fields for different stocks or different data fields. They allow researchers to calculate the probability of a score occurring within a standard normal distribution and compare two scores that are from different samples (which may have different means and standard deviations).
This operator may help reduce outliers.
Input: Value of 5 instruments at day t: (100, 0, 50, 60, 25).
Output: (1.57, -1.39, 0.09, 0.39, -0.65) from SD: 33.7, mean: 47.

Vector operators:

vec_avg(x):
Taking mean of the vector field x Input: Vector of value of 1 instrument in a day: (2, 3, 5, 6, 3, 8, 10) Output: 37 / 7 = 5.29.

vec_max(x):
Maximum value form vector field x.

vec_min(x):
Minimum value form vector field x.

vec_sum(x):
Sum of vector field x Input: Vector of value of 1 instrument in a day: (2, 3, 5, 6, 3, 8, 10) Output: 2 + 3 + 5 + 6 + 3 + 8 + 10 = 37.

Transformational operators:
bucket(rank(x), range="0, 1, 0.1" or buckets = "2,5,6,7,10"):
Convert float values into indexes for user-specified buckets. Bucket is useful for creating group values, which can be passed to GROUP as input.
Convert float values into indexes for user-specified buckets. Bucket is useful for creating group values, which can be passed to group operators as input.
If buckets are specified as "num_1, num_2, …, num_N", it is converted into brackets consisting of [(num_1, num_2, idx_1), (num_2, num_3, idx_2), ..., (num_N-1, num_N, idx_N-1)].
Thus with buckets="2, 5, 6, 7, 10", the vector "-1, 3, 6, 8, 12" becomes "0, 1, 2, 4, 5".
If range if specified as "start, end, step", it is converted into brackets consisting of [(start, start+step, idx_1), (start+step, start+2*step, idx_2), ..., (start+N*step, end, idx_N)].
Thus with range="0.1, 1, 0.1", the vector "0.05, 0.5, 0.9" becomes "0, 4, 8".
Note that two hidden buckets corresponding to (-inf, start] and [end, +inf) are added by default. Use the skipBegin, skipEnd parameters to remove these buckets. Use skipBoth to set both skipEnd and skipBegin to true.
If you want to assign all NAN values into a separate group of their own, use NANGroup. The index value will be one after the last bucket.
Examples:
my_group = bucket(rank(volume), range="0.1,1,0.1");
group_neutralize(sales/assets, my_group).
my_group = bucket(rank(volume), buckets ="0.2,0.5,0.7", skipBoth=True, NANGroup=True);
group_neutralize(sales/assets, my_group).

trade_when(x, y, z):
Used in order to change Alpha values only under a specified condition and to hold Alpha values in other cases. It also allows to close Alpha positions (assign NaN values) under a specified condition.
This operator can be used to change Alpha values only under a specified condition and to retain Alpha values in other cases. It also allows for closing Alpha positions (assigning NaN values) under a specified condition.
Trade_When (x=triggerTradeExp, y=AlphaExp, z=triggerExitExp).
If triggerExitExp > 0, Alpha = NaN.
Else if triggerTradeExp > 0, Alpha = AlphaExp;
else, Alpha = previousAlpha.
This operator may help reduce correlation and reduce turnover.
Examples:
Trade_When (volume >= ts_sum(volume,5)/5, rank(-returns), -1).
If (volume >= ts_sum(volume,5)/5), Alpha = rank(-returns);
else trade previous Alpha;
exit condition is always false.
Trade_When (volume >= ts_sum(volume,5)/5, rank(-returns), abs(returns) > 0.1).
If abs(returns) > 0.1, Alpha = nan;
else if volume >= ts_sum(volume,5)/5, Alpha = rank(-returns);
else trade previous Alpha.

Group operators:

group_backfill(x, group, d, std = 4.0):
If a certain value for a certain date and instrument is NaN, from the set of same group instruments, calculate winsorized mean of all non-NaN values over last d days.
group_backfill(x, group, d, std = 4.0).
If a certain value for a certain date and instrument is NaN, from the set of same group instruments, calculate winsorized mean of all non-NaN values over last d days. Winsorized mean means inputs are truncated by std * stddev where stddev is the standard deviation of inputs.
Example:
If d = 4 and there are 3 instruments(i1, i2, i3) in a group whose values for past 4 days are x[i1] = [4,2,5,5], x[i2] = [7,NaN,2,9], x[i3] = [NaN,-4,2,NaN] where first element is most recent, then if we want to backfill x, we will only have to backfill x[i3]’s first element because every other instrument’s first element is non-NaN.
The non-NaN values of other groups are [4,2,5,5,7,2,9,-4,2]. Mean = 3.56, Standard deviation is 3.71 and none of the item is outside the range of 3.56 – 4 * 3.71 and 3.56 + 4 * 3.71. Hence, we don’t need to clip elements to those limits. Hence, Winsorized mean = backfilled value = 3.56.
For three instruments, group_backfill(x, group, d, std = 4.0) = [4,7,3.56].

group_cartesian_product(g1, g2):
Merge two groups into one group. If originally there are len_1 and len_2 group indices in g1 and g2, there will be len_1 * len_2 indices in the new group.

group_max(x, group):
Maximum of x for all instruments in the same group.

group_mean(x, weight, group):
All elements in group equals to the mean.
group_mean(x, group) operator can be used to calculate harmonic mean of datafields. Harmonic mean gives equal weight to each value in terms of their reciprocal contribution and is considered a better method for calculating average for fundamental ratios and factors. For example, Harmonic mean of P/E ratio (price-to-earnings) for an industry can be calculated as:
1 /(group_mean(eps/close,1, industry)).

group_min(x, group):
All elements in group equals to the min value of the group.

group_neutralize(x, group):
Neutralizes Alpha against groups. These groups can be subindustry, industry, sector, country or a constant.
group_neutralize(x, group).
Neutralize alpha against groups. Difference between normalize and group_neutralize is in normalize, every element is subtracted by mean of all values of all instruments on that day whereas in group_neutralize, element is subtracted by mean of all values of the group of instruments that it belongs on that day.
This operator may help reduce correlation, depending on the neutralization used.
Example:
If values of field x on a certain date for 10 instruments is [3,2,6,5,8,9,1,4,8,0] and first 5 instruments belong to one group, last 5 belong to other, then mean of first group = (3+2+6+5+8)/5 = 4.8 and mean of second group = (9+1+4+8+0)/5 = 4.4. Subtracting means from instruments of respective groups gives [3-4.8, 2-4.8, 6-4.8, 5-4.8, 8-4.8, 9-4.4, 1-4.4, 4-4.4, 8-4.4, 0-4.4] = [-1.8, -2.8, 1.2, 0.2, 3.2, 4.6, -3.4, -0.4, 3.6, -4.4].

group_rank(x, group):
Each elements in a group is assigned the corresponding rank in this group.
group_rank(x, group).
Group operators are a type of cross-sectional operator that compares stocks at a finer level, where the cross-sectional operation is applied within each group, rather than across the entire market. The group_rank operator allocates the stocks to their specified group, then within each group, it ranks the stocks based on their input value for data field x and returns an equally distributed number between 0.0 and 1.0.
This operator may help reduce both outliers and drawdown while reducing correlation.
Example: group_rank(x, subindustry).
The stocks are first grouped into their respective subindustry.
Within each subindustry, the stocks within that subindustry are ranked based on their input value for data field x and assigned an equally distributed number between 0.0 and 1.0.
Input: Value of 3 instruments of Group A: (100, 0, 50).
Output: (1, 0, 0.5).

group_scale(x, group):
Normalizes the values in a group to be between 0 and 1. (x - groupmin) / (groupmax - groupmin).

group_zscore(x, group):
Calculates group Z-score - numerical measurement that describes a value's relationship to the mean of a group of values. Z-score is measured in terms of standard deviations from the mean. zscore = (data - mean) / stddev of x for each instrument within its group. Input: Value of 5 instruments of Group A: (100, 0, 50, 60, 25) Output: (1.57, -1.39, 0.09, 0.39, -0.65).
