# Data masking in Hive {#concept_ncg_ydg_mgb .concept}

Ranger supports data masking in Hive by masking return values of SELECT statements to hide sensitive information from users.

**Note:** This feature only supports scenarios involving HiveServer2, such as using Beeline, JDBC, or Hue to run SELECT statements. HiveClient-based scenarios are not supported, such as hive -e 'select xxxx'.

This topic describes how to use this feature in E-MapReduce.

## Configure the Hive plug-in for Ranger {#section_xwq_vfg_mgb .section}

For more information, see [Hive configurations](intl.en-US/User Guide/Component authorization/Ranger/Integrate Ranger into Hive.md#).

## Configure Data Mask Policy {#section_osz_33g_mgb .section}

You can mask Hive data accessed by users on the `emr-hive` service page in the Ranger UI.

-   Ranger supports a variety of masking types, such as show the first four characters, show the last four characters, and Hash masks.
-   A mask policy does not support wildcards. For example, you cannot use an asterisk \(\*\) to replace columns or tables in a mask policy.
-   Each mask policy is corresponding to one column. You need to configure mask policies for each column.

Perform the following steps to configure a mask policy.![Configure a mask policy](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/105886/155141002437543_en-US.png)![Configure a mask policy](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/105886/155141002437548_en-US.png)![Configure a mask policy](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/105886/155141002437550_en-US.png)

Save your mask policy.

## Mask test data {#section_z3g_4mg_mgb .section}

-   Scenario:

    User test selects column a from the testdb1.testtbl table to display only the first four characters of each value.

-   Procedure:
    1.  Configure a mask policy

        The last figure in the previous section shows the mask policy for this scenario. "show first 4" is selected as the masking type.

    2.  Verify data masking

        User test uses Beeline to connect to HiveServer2 and runs the `select a from testdb1.testtbl` statement.

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/105886/155141002437553_en-US.png)

        As shown above, after user test runs the SELECT statement, only the first four characters of values of column a are shown. The rest characters are replaced by `x` for data masking.


