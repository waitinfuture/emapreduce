# GROUP BY statement {#concept_1062537 .concept}

A GROUP BY statement groups a result set by one or more columns.

## Syntax {#section_hlz_3yk_v3u .section}

``` {#codeblock_bph_xvx_31w}
SELECT [ DISTINCT ]
{ * | projectItem [, projectItem ]* }
FROM tableExpression
[ GROUP BY { groupItem [, groupItem ]* } ];
```

## Example {#section_il4_nr9_hlt .section}

``` {#codeblock_x5g_tvo_rfd}
SELECT Customer, SUM(OrderPrice) FROM xxx
GROUP BY Customer;
```

