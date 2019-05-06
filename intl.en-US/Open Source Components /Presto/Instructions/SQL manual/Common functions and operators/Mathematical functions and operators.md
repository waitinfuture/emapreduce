# Mathematical functions and operators {#concept_rpt_2jb_ygb .concept}

## Mathematical operators {#section_sds_4kb_ygb .section}

|Operator|Description|
|--------|-----------|
|+|Addition|
|-|Subtraction|
|\*|Multiplication|
|/|Division \(integer division performs truncation\)|
|%|Modulus \(remainder\)|

## Mathematical functions {#section_y51_qkb_ygb .section}

Presto provides a variety of mathematical functions, as listed in the following table.

|Function|Syntax|Description|
|--------|------|-----------|
|abs|abs\(x\) →|x|
|cbrt|cbrt\(x\) → double|Returns the cube root of x.|
|ceil|ceil\(x\)|Returns x rounded up to the nearest integer. This is an alias for `ceiling`.|
|ceiling|ceiling\(x\)|Returns x rounded up to the nearest integer.|
|cosine\_similarity|cosine\_similarity\(x, y\) → double|Returns the cosine similarity between the sparse vectors x and y.|
|degrees|degress\(x\) -\> double|Converts angle x in radians to degrees.|
|e|e\(\)-\>double|Returns the constant Euler’s number.|
|exp|exp\(x\)-\>double|Returns e^x of the given number x.|
|floor|floor\(x\)|Returns x rounded down to the nearest integer.|
|from\_base|from\_base\(string, radix\) → bigint|Returns the value of string interpreted as a base-radix number.|
|inverse\_normal\_cdf|inverse\_normal\_cdf\(mean,sd,p\)-\>double|Computes the inverse of the Normal CDF with given mean and standard deviation \(SD\) for the cumulative probability.|
|ln|ln\(x\)-\>double|Returns the natural logarithm of x.|
|log2|log2\(x\)-\>double|Returns the base 2 logarithm of x.|
|log10|log10\(x\)-\>double|Returns the base 10 logarithm of x.|
|log|log\(x,b\) -\> double|Returns the base b logarithm of x.|
|mod|mod\(n,m\)|Returns the modulus \(remainder\) of n divided by m.|
|pi|pi\(\)-\>double|Returns the constant Pi.|
|pow|pow\(x,p\)-\>double|Returns x raised to the power of p. This is an alias for `power`.|
|power|power\(x,p\)-\>double|Returns x raised to the power of p.|
|radians|radians\(x\)-\>double|Converts angle x in degrees to radians.|
|rand|rand\(\)-\>double|Returns a pseudo-random number in the range \[0.0, 1.0\). This is an alias for `random`.|
|random|random\(\)-\>double|Returns a pseudo-random number in the range \[0.0, 1.0\).|
|random|random\(n\)|Returns a pseudo-random number in the range \[0.0, n\).|
|round|round\(x\)|Returns x rounded to the nearest integer.|
|round|round\(x, d\)|Returns x rounded to d decimal places.|
|sign|sign\(x\)|Sign function. If x is an integer, 0 is returned when x is equal to 0; 1 is returned when x is greater than 0; and -1 is returned when x is smaller than 0. If x is a floating point number, NAN is returned when x is NAN; 1 is returned when x is +∞; and -1 is returned when x is -∞.|
|sqrt|sqrt\(x\)-\>double|Returns the square root of x.|
|to\_base|to\_base\(x, radix\)-\>varchar|Returns the base-radix representation of x.|
|truncate|truncate\(x\) → double|Returns x rounded to an integer by dropping digits after the decimal point.|
|width\_bucket|width\_bucket\(x, bound1, bound2, n\) → bigint|Returns the bin number of x in an equi-width histogram with the specified bound1 and bound2 bounds and n number of buckets.|
|width\_bucket|width\_bucket\(x, bins\)|Returns the bin number of x in a histogram according to the bins specified by the array bins.|
|acos|acos\(x\)-\>double|Returns the arc cosine of x, which is a radian.|
|asin|asin\(x\)-\>double|Returns the arc sine of x, which is a radian.|
|atan|atan\(x\)-\>double|Returns the arc tangent of x, which is a radian.|
|atan2|atan2\(y,x\)-\>double|Returns the arc tangent of y/x, which is a radian.|
|cos|cos\(x\)-\>double|Returns the cosine of x, which is a radian.|
|cosh|cosh\(x\)-\>double|Returns the hyperbolic cosine of x, which is a radian.|
|sin|sin\(x\)-\>double|Returns the sine of x, which is a radian.|
|tan|tan\(x\)-\>double|Returns the tangent of x, which is a radian.|
|tanh|tanh\(x\)-\>double|Returns the hyperbolic tangent of x, which is a radian.|
|infinity|infinity\(\) → double|Returns the constant representing positive infinity.|
|is\_finite|is\_finite\(x\) → boolean|Determines if x is finite.|
|is\_infinite|is\_infinite\(x\) → boolean|Determines if x is infinite.|
|is\_nan|is\_nan\(x\) → boolean|Determines if x is not a number.|
|nan|nan\(\)|Returns the constant representing not-a-number \(NAN\).|

