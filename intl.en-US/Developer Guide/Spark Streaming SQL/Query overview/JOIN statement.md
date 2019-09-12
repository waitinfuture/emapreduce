# JOIN statement {#concept_1062591 .concept}

Spark SQL supports joining batch processing data and streaming data, joining batch processing data, and joining streaming data. The JOIN statement has the same semantics as a traditional JOIN statement for batch processing.

## Syntax {#section_l04_tfw_ogq .section}

``` {#codeblock_f0u_v1d_7q6}
tableReference [, tableReference ]* | tableexpression
[ joinType ] JOIN tableexpression [ joinCondition ];
```

## Constraint {#section_u1j_1ym_ddx .section}

When joining streaming data, note that some join types are not supported. For more information, see [Spark official documentation](http://spark.apache.org/docs/latest/structured-streaming-programming-guide.html). The following table lists some join types.

|Left table|Right table|Join type|Supported|
|----------|-----------|---------|---------|
|Stream|Static|Inner|Supported, not stateful.|
|Left Outer|Supported, not stateful.|
|Right Outer|Not supported.|
|Full Outer|Not supported.|
|Static|Stream|Inner|Supported, not stateful.|
|Left Outer|Not supported.|
|Right Outer|Supported, not stateful.|
|Full Outer|Not supported.|
|Stream|Stream|Inner|Supported. You can optionally specify a watermark on both sides and time constraints for state cleanup.|
|Left Outer|Conditionally supported. You must specify a watermark on right and time constraints for correct results. You can optionally specify a watermark on left for all state cleanup.|
|Right Outer|Conditionally supported. You must specify a watermark on left and time constraints for correct results. You can optionally specify a watermark on right for all state cleanup.|
|Full Outer|Not supported.|

