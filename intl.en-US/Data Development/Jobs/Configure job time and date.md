# Configure job time and date

When you edit a job, you can set a time variable wildcard.

## Variable wildcard format

E-MapReduce \(EMR\) supports the following formats of variable wildcards: $\{dateexpr-1d\} and $\{dateexpr-1h\}. dateexpr specifies the standard format of time. The expression is case-sensitive. Where:

-   yyyy indicates a 4-digit year.
-   MM indicates a 2-digit month.
-   dd indicates a 2-digit day.
-   HH indicates a 2-digit hour \(24-hour clock\). hh indicates a 2-digit hour \(12-hour clock\).
-   mm indicates a 2-digit minute.
-   ss indicates a 2-digit second.

A time variable is a combination of yyyy and one or more other time formats. You can also use the plus sign \(+\) or minus sign \(-\) to add or subtract a specified period of time to or from the current time. For example, $\{yyyy-MM-dd\} indicates the current date.

-   One year after the current date can be represented as $\{yyyy+1y\} or $\{yyyy-MM-dd hh:mm:ss+1y\}.
-   Three months after the current date can be represented as $\{yyyyMM+3m\} or $\{yyyy-MM-dd hh:mm:ss+3m\}.
-   Five days before the current date can be represented as $\{yyyyMMdd-5d\} or $\{yyyy-MM-dd hh:mm:ss-5d\}.

Assume that the current time is 20160427 12:08:01.

-   If $\{yyyyMMdd HH:mm:ss-1d\} is configured as the variable wildcard, the time will be replaced with 20160426 12:08:01 when a job is executed. One day is subtracted from the current date and the new time is accurate to seconds.
-   If $\{yyyyMMdd-1d\} is configured as the variable wildcard, the time will be replaced with 20160426, which indicates the day before the current date.
-   If $\{yyyyMMdd\} is configured as the variable wildcard, the time will be replaced with 20160427, which indicates the current date.

**Note:**

-   Only days or hours can be added or subtracted. That is, dateexpr can be followed only by +Nd, -Nd, +Nh, or -Nh. N must be an integer.
-   A time variable must start with yyyy, for example, $\{yyyy-MM\}. If you want to obtain the values based on a specific period such as a month, you can use the following functions in a job:

    -   parseDate\(<Parameter name\>, <Time format\>\): You can use this function to convert a specified parameter to a date object. A parameter name indicates the variable \(key\) name specified in the Configuration Parameters section. A time format is the time format used by the variable name. For example, if the parameter name of the current\_time variable is $\{yyyyMMddHHmmss-1d\}, the time format is yyyyMMddHHmmss.
    -   formatDate\(<Date object\>, <Time format\>\): You can use this function to convert a specified date object to a time format string.
    Examples:

    -   $\{formatDate\(parseDate\(current\_time, 'yyyyMMddHHmmss'\), 'HH'\)\} retrieves the literal hour value from the current\_time variable.
    -   $\{formatDate\(parseDate\(current\_time, 'yyyyMMddHHmmss'\), 'yyyy'\)\} retrieves the literal year value from the current\_time variable.

## Example

1.  Log on to the [EMR console](https://emr.console.aliyun.com/).

2.  In the top navigation bar, select the region where your cluster residesand select a resource group based on your business requirements.

3.  Click the **Data Platform** tab.

4.  In the **Projects** section of the page that appears, find your project and click **Edit Job** in the Actions column.

5.  In the **Edit Job** pane on the left, click a specific job name and click **Job Settings** in the upper-right corner.

6.  In the **Configuration Parameters** section of the Basic Settings tab in the Job Settings panel, click the ![add](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/0399625061/p70542.png) icon to configure a variable wildcard in one of the preceding formats, as shown in the following figure.

    ![date_example](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/5734204061/p37971.png)

    After you complete the configuration, you can reference the key of the configured parameter in the job.


