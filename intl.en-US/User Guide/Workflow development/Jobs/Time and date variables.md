# Time and date variables {#concept_dhr_xkp_y2b .concept}

When you are creating a job, variable wildcards are supported in the job parameters for both time and date.

## Variable wildcard format {#section_gfy_zkp_y2b .section}

The format of the variable wildcards supported by E-MapReduce is either $\{dateexpr-1d\} or $\{dateexpr-1h\}. For example, assuming the current date and time is 2016/04/27 12:08:01:

-   If $\{yyyyMMdd HH:mm:ss-1d\} is displayed, the parameter wildcard is replaced with 20160426 12:08:01 when executed, which is the current date minus one day, and time accurate to the second.
-   If $\{yyyyMMdd-1d\} is displayed, the parameter wildcard is replaced with 20160426 when executed, which is the current date minus one day.
-   If $\{yyyyMMdd\} is displayed, the parameter wildcard is replaced with 20160427, which is the current date.

**dateexpr** represents the standard format of expressing time. Time is therefore formatted according to this expression and is followed by the amount of time that you want to add or deduct, which can be written as N. For example, $\{yyyyMMdd-5d\}, $\{yyyyMMdd+5d\}, $\{yyyyMMdd+5h\}, or $\{yyyyMMdd-5h\}.

**Note:** E-MapReduce currently supports the addition and deduction of hours and days only.

## Example {#section_jdv_tkz_ngb .section}

1.  Click **Job Settings** on the top right of the **Edit Jobs** page.
2.  Click the add icon to add new parameters on the **Parameter Configuration** part，and fill in the parameter according to the Variable wildcard format that mentioned above.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17875/154829923437971_en-US.png)

3.  You can now use the reference of the parameter key in the job editing. 

