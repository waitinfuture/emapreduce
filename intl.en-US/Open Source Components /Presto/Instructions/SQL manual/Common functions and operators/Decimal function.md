# Decimal function {#concept_vlq_vnb_ygb .concept}

## Literal value {#section_k4z_34b_ygb .section}

Use the following syntax to define the literal value of the `DECIMAL type`:

```
DECIMAL 'xxxx.yyyyy'

```

The `precision` of the DECIMAL type for the literal value is equal to the number of digits in the literal value \(including leading 0s\). The `scale` is equal to the number of digits in the fractional part \(including trailing 0s\). For example:

|Literal value|Data type|
|-------------|---------|
|DECIMAL ‘0’|DECIMAL\(1\)|
|DECIMAL ‘12345’|DECIMAL\(5\)|
|DECIMAL ‘0000012345.1234500000’|DECIMAL\(20, 10\)|

## Operators {#section_qrd_j4b_ygb .section}

-   Arithmetic operators

    Assume that variables x and y are of the `DECIMAL` type.

    -   x: `DECIMAL(xp,xs)`
    -   y: `DECIMAL(yp,ys)`
    They observe the following rules when used in arithmetic operation:

    -   `x + y` or `x - y`

        -   precision = *min*\(38, 1 + *min*\(xs, ys\) + *min*\(xp-xs, yp-ys\)\)
        -   scale = *max*\(xs, ys\)
    -   `x * y`

        -   precision = *min*\(38, xp + yp\)
        -   scale = xs + ys
    -   `x / y`

        -   precision = *min*\(38, xp + ys + *max*\(0, ys-xs\)\)
        -   scale = *max*\(xs, ys\)
    -   `x % y`

        -   precision = *min*\(xp - xs, yp - ys\) + *max*\(xs, bs\)
        -   scale = *max*\(xs, ys\)
-   Comparison operators

    All standard comparison operators and `BETWEEN` operators work for the `DECIMAL` type.

-   Unary decimal operators

    The `-` operator performs negation for the `DECIMAL` type.


