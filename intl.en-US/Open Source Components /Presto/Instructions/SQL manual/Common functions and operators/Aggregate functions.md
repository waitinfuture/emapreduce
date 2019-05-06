# Aggregate functions {#concept_crf_df3_ygb .concept}

Aggregate functions have the following features:

-   Input a dataset.
-   Output a single computation result.

A majority of aggregate functions ignore `null` values during computation and return `null` when no input is made or all values are `null`, but there are a few exceptions:

-   count
-   count\_if
-   max\_by
-   min\_by
-   approx\_distinct

## Basic aggregate functions {#section_iyq_m33_ygb .section}

|Function|Syntax|Description|
|--------|------|-----------|
|arbitrary|arbitrary\(x\) → \[same as input\]|Returns an arbitrary non-null value of x.|
|array\_agg|array\_agg\(x\) → array<\[same as input\]\>|Returns an array created from the input elements.|
|avg|avg\(x\) → double|Returns the average \(arithmetic mean\) of all input values.|
|avg|avg\(time interval type\) → time interval type|Returns the average interval length of all input time series.|
|bool\_and|bool\_and\(boolean\) → boolean|Returns TRUE if every input value is TRUE. Otherwise, FALSE is returned.|
|bool\_or|bool\_or\(boolean\) → boolean|Returns TRUE if any of the input values is True. Otherwise, FALSE is returned.|
|checksum|checksum\(x\) → varbinary|Returns an order-insensitive checksum of x.|
|count|count\(\*\) → bigint|Returns the number of rows.|
|count|count\(x\) → bigint|Returns the number of non-null elements.|
|count\_if|count\_if\(x\) → bigint|Returns the number of TRUE elements of x. This function is equivalent to `count(CASE WHEN x THEN 1 END)`.|
|every|every\(boolean\) → boolean|This is an alias for `bool_and`.|
|geometric\_mean|geometric\_mean\(x\) → double|Returns the geometric mean of x.|
|max\_by|max\_by\(x, y\) → \[same as x\]|Returns the value of x associated with the maximum value of y over all input values.|
|max\_by|max\_by\(x, y, n\) → array<\[same as x\]\>|Returns an array of x associated with the n largest of all input values of y.|
|min\_by|min\_by\(x, y\) → \[same as x\]|Returns the value of x associated with the minimum value of y over all input values.|
|min\_by|min\_by\(x, y, n\) → array<\[same as x\]\>|Returns an array of x associated with the n smallest of all input values of y.|
|max|max\(x\) → \[same as input\]|Returns the maximum value among all input values.|
|max|max\(x, n\) → array<\[same as x\]\>|Returns the n largest values of all input values of x.|
|min|min\(x\) → \[same as input\]|Returns the minimum value among all input values.|
|min|min\(x, n\) → array<\[same as x\]\>|Returns the n smallest values of all input values of x.|
|sum|sum\(x\) → \[same as input\]|Returns the sum of all input values of x.|

## Bitwise aggregate functions {#section_fg4_fj3_ygb .section}

For bitwise aggregate functions, see `bitwise_and_agg` and `bitwise_or_agg` functions described in [Bitwise operators](reseller.en-US/Open Source Components /Presto/Instructions/SQL manual/Common functions and operators/Bitwise functions.md#).

## Map aggregate functions {#section_x3y_kj3_ygb .section}

|Function|Syntax|Description|
|--------|------|-----------|
|histogram|histogram\(x\) → map<K,bigint\>|Creates a statistics histogram.|
|map\_agg|map\_agg\(key, value\) → map<K,V\>|Creates a variable of the `MAP` type.|
|map\_union|map\_union\(x<K, V\>\) → map<K,V\>|Returns the union of all the input maps. If a key is found in multiple input maps, the key value in the resulting map comes from an arbitrary input map.|
|multimap\_agg|multimap\_agg\(key, value\) → map<K,array\>|Creates a variable of the `MAP` type with multiple mappings.|

## Approximate aggregate functions {#section_bjc_nj3_ygb .section}

|Function|Syntax|Description|
|--------|------|-----------|
|approx\_distinct|approx\_distinct\(x, \[e\]\) → bigint|Returns the approximate number of rows that contain distinct input values. This function provides an approximation of `count(DISTINCT x)`. 0 is returned if all input values are null. This function produces a standard error of no more than `e`, which is the standard deviation of the \(approximately normal\) error distribution over all possible sets. It is optional and set to 2.3% by default. The current implementation of this function requires that `e` be in the range \[0.01150, 0.26000\]. It does not guarantee an upper limit on the error for any specific input set.|
|approx\_percentile|approx\_percentile\(x, percentage\) → \[same as x\]|Returns the approximate percentile for all input values of x at the given percentage.|
|approx\_percentile|approx\_percentile\(x, percentages\) → array<\[same as x\]\>|Similar to the preceding function, percentages is an array and returns constant values for all input rows.|
|approx\_percentile|approx\_percentile\(x, w, percentage\) → \[same as x\]|Similar to the preceding function, w is the weighted value of x.|
|approx\_percentile|approx\_percentile\(x, w, percentage, accuracy\) → \[same as x\]|Similar to the preceding function, `accuracy` is the upper limit of the estimation accuracy, and the value must be in the range \[0, 1\].|
|approx\_percentile|approx\_percentile\(x, w, percentages\) → array<\[same as x\]\>|Similar to the preceding function, percentages is an array and returns constant values for all input rows.|
|numeric\_histogram|numeric\_histogram\(buckets, value, \[weight\]\) → map<double, double\>|Computes an approximate histogram with up to a given number of buckets. `buckets` must be a `BIGINT`. `value` and `weight` must be numeric. Weight is optional and set to 1 by default.|

## Statistical aggregate functions {#section_alp_hk3_ygb .section}

|Function|Syntax|Description|
|--------|------|-----------|
|corr|corr\(y, x\) → double|Returns the correlation coefficient.|
|covar\_pop|covar\_pop\(y, x\) → double|Returns the population covariance of input values.|
|covar\_samp|covar\_samp\(y, x\) → double|Returns the sample covariance of input values.|
|kurtosis|kurtosis\(x\) → double|Returns the excess kurtosis of all input values. Use the following expression for unbiased estimation:```
kurtosis(x) =
              n(n+1)/((n-1)(n-2)(n-3))sum[(x_i-mean)^4]/sttdev(x)^4-3(n-1)^2/((n-2)(n-3))
```

|
| | | |
|regr\_intercept|regr\_intercept\(y, x\) → double|Returns the y-intercept of the linear regression line. `y` is the dependent variable. `x` is the independent variable.|
|regr\_slope|regr\_slope\(y, x\) → double|Returns the linear regression slope of input values. `y` is the dependent variable. `X` is the independent variable.|
|skewness|skewness\(x\) → double|Returns the skewness of all input values.|
|sttdev\_pop|sttdev\_pop\(x\) → double|Returns the population standard deviation of all input values.|
|sttdev\_samp|sttdev\_samp\(x\) → double|Returns the sample standard deviation of all input values.|
|sttdev|sttdev\(x\) → double|This is an alias for `sttdev_samp`.|
|var\_pop|var\_pop\(x\) → double|Returns the population variance of all input values.|
|var\_samp|var\_samp\(x\) → double|Returns the sample variance of all input values.|
|variance|variance\(x\) → double|This is an alias for `var_samp`.|

