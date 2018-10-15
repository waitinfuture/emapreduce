# Job date variables {#concept_dhr_xkp_y2b .concept}

During creation, time variable wildcard settings in job parameter are supported.

## Variable wildcard format {#section_gfy_zkp_y2b .section}

The format of variable wildcard supported by E-MapReduce is $\{dateexpr-1d\} or $\{dateexpr-1h\}. For example, assuming the current time is 20160427 12:08:01:

-   If it is written as $\{yyyyMMdd HH:mm:ss-1d\} in job parameters, then this parameter wildcard will be, when executed practically, replaced with 20160426 12:08:01, that is, the current date minus one day with accuracy to the second.
-   If it is written as $\{yyyyMMdd-1d\}, then it will be replaced with 20160426 when being executed, representing the day before current date.
-   If it is written as $\{yyyyMMdd\}, then it will be replaced with 20160427, representing the current date.

The dateexpr represents the expression of standard time format, and corresponding time will be formatted as per this expression and followed by corresponding time to add or deduct. Following the expression, 1d \(1 day\) to add or deduct can be written as N days or hours, for example, $\{yyyyMMdd-5d\},$\{yyyyMMdd+5d\},$\{yyyyMMdd+5h\},$\{yyyyMMdd-5h\} are all supported, and corresponding replacing methods are consistent with the descriptions above.

**Note:** E-MapReduce currently supports addition and deduction only for “hour” and “day”, that is, the format of +Nd, -Nd, +Nh and -Nh after dateexpr \(dateexpr refers to the expression of time format and N is an integer\).

## Example {#section_jfy_zkp_y2b .section}

When being executed practically, the **Parameter** in the job on the figure below will be replaced with:

```
jar ossref://emr/jar/hadoop/hadoop_wc.jar com.aliyun.emr.WordCount oss://emr/output/pt=20160426
```

