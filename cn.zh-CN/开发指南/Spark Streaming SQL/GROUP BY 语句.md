# GROUP BY 语句 {#concept_1062537 .concept}

GROUP BY 语句用于根据一个或多个列对结果集进行分组。

## 语法 {#section_hlz_3yk_v3u .section}

``` {#codeblock_bph_xvx_31w}
SELECT [ DISTINCT ]
{ * | projectItem [, projectItem ]* }
FROM tableExpression
[ GROUP BY { groupItem [, groupItem ]* } ];
```

## 示例 {#section_il4_nr9_hlt .section}

``` {#codeblock_x5g_tvo_rfd}
SELECT Customer, SUM(OrderPrice) FROM xxx
GROUP BY Customer;
```

