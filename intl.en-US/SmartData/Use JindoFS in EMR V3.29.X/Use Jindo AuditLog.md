# Use Jindo AuditLog

Jindo AuditLog allows you to audit operations in the namespaces that are in block storage mode. It records addition, deletion, and renaming operations in the namespaces.

-   An EMR cluster of the 3.29.X version is created. For more information about how to create a cluster, see [Create a cluster](/intl.en-US/Cluster Management/Configure clusters/Create a cluster.md).
-   An OSS bucket is created. For more information, see [Create buckets](/intl.en-US/Quick Start/Create buckets.md).

You can use AuditLog to analyze namespace access information, detect abnormal requests, and track errors. AuditLog stores log files in OSS. The size of a single log file cannot exceed 5 GB. You can use the lifecycle management feature of OSS to customize a retention period in days for the log files. JindoFS allows you to use Shell commands to analyze the log files generated by AuditLog.

## Audit log

Example:

```
2020-07-09 18:29:24.689 allowed=true ugi=hadoop (auth:SIMPLE) ip=127.0.0.1 ns=test-block cmd=CreateFileletRequest src=jfs://test-block/test/test.snappy.parquet dst=null perm=::rwxrwxr-x
```

The following table describes the parameters of an audit log recorded by AuditLog for a namespace in block storage mode.

|Parameter|Description|
|---------|-----------|
|Time|The time format is yyyy-MM-dd hh:mm:ss.SSS.|
|allowed|Indicates whether the operation is allowed. Valid values:-   true
-   false |
|ugi|The user who performed the operation. The information about the authentication method is also displayed.|
|ip|The client IP address.|
|ns|The name of the namespace in block storage mode.|
|cmd|The operation command.|
|src|The source path.|
|dest|The destination path. This parameter can be left empty.|
|perm|The operation permissions on the file.|

## Configure AuditLog

1.  Go to the SmartData service.

    1.  Log on to the [EMR console](https://emr.console.aliyun.com/).

    2.  In the top navigation bar, select the region where your cluster resides. Select the resource group as required. By default, all resources of the account appear.

    3.  Click the **Cluster Management** tab.

    4.  On the **Cluster Management** page that appears, find the target cluster and click **Details** in the Actions column.

    5.  In the left-side navigation pane, click **Cluster Service** and then **SmartData**.

2.  Go to the **namespace** tab for the SmartData service.

    1.  Click the **Configure** tab.

    2.  Click the **namespace** tab in the Service Configuration section.

        ![namespace](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/2819013061/p161094.png)

3.  Perform the following operations:

    1.  In the upper-right corner of the **Service Configuration** section, click **Custom Configuration**.

    2.  In the **Add Configuration Item** dialog box, configure the parameters described in the following table.

        |Parameter|Description|Required|
        |---------|-----------|--------|
        |namespace.auditlog.enable|        -   true: Enable AuditLog.
        -   false: Disable AuditLog.
|Yes|
        |namespace.auditlog.oss.uri|The OSS bucket where the log files generated by AuditLog are stored.Set this parameter in the format of oss://<yourbucket\>/auditLog.

Replace <yourbucket\> with the OSS bucket name.

|Yes|
        |namespace.auditlog.oss.accessKey|The AccessKey ID used to access the OSS bucket.|No|
        |namespace.auditlog.oss.accessSecret|The AccessKey secret used to access the OSS bucket.|No|
        |namespace.auditlog.oss.endpoint|The endpoint of the OSS bucket.|No|

    3.  In the upper-right corner of the Service Configuration section, click **Deploy Client Configuration**.

    4.  In the **Cluster Activities** dialog box, specify **Description** and click **OK**.

    5.  In the **Confirm** message, click **OK**.

4.  Restart Namespace Service.

    1.  Select **Restart Jindo Namespace Service** from the **Actions** drop-down list in the upper-right corner.

    2.  In the **Cluster Activities** dialog box, specify **Description** and click **OK**.

    3.  In the **Confirm** message, click **OK**.

5.  Configure a retention period for log files.

    OSS provides the lifecycle management feature for you to manage the lifecycles of files in OSS. You can use this feature to customize a retention period for the log files generated by AuditLog.

    1.  Log on to the [OSS console](https://oss.console.aliyun.com/).

    2.  In the left-side navigation pane, click Buckets. On the page that appears, click the name of the created bucket.

    3.  In the left-side navigation pane, choose **Basic Settings** \> **Lifecycle**. In the **Lifecycle** section, click **Configure**.

    4.  Click **Create Rule**. In the **Create Rule** dialog box, configure the parameters.

        For more information, see [Configure lifecycle rules](/intl.en-US/Console User Guide/Manage buckets/Basic settings/Configure lifecycle rules.md).

    5.  Click **OK**.


## Analyze log files

JindoFS allows you to use Shell commands to analyze the log files generated by AuditLog. You can run a MapReduce job in the EMR console to analyze the most active commands or IP addresses in the log files. The analysis command is `jindo auditlog`.

The following table describes the parameters in the command.

|Parameter|Description|Required|
|---------|-----------|--------|
|--src|The OSS bucket where the log files generated by AuditLog are stored. By default, this parameter is set to the value of the namespace.auditlog.oss.uri parameter specified in [Step 3](#step_pld_w9e_qxj). You can customize a value.|No|
|--ns|The namespace you want to analyze. By default, all namespaces in block storage mode are analyzed.|No|
|--type|The analysis objects:-   ip: the most active IP addresses
-   cmd: the most active commands

|Yes|
|--min|The time range in minutes.|No**Note:** You can specify only one of --min and --day. |
|--day|The time range in days.day 1 indicates the current day. |

In the EMR console, create a MapReduce job and run the jindo audit command. Example:

```
jindo auditlog --src oss://<yourbucket>/auditlog/ --ns test --type ip --day 1 --top 2
```

The following information is returned:

```
16 openFileStatusRequest
6  deleteFileletRequest
```

