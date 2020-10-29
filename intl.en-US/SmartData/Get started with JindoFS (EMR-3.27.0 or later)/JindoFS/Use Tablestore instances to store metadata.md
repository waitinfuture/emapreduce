# Use Tablestore instances to store metadata

Namespace Service of JindoFS supports different metadata storage methods. This topic describes how to configure a Tablestore instance as the metadata storage backend.

-   An E-MapReduce \(EMR\) cluster is created.

    For more information, see [Create a cluster](/intl.en-US/Cluster Management/Configure clusters/Create a cluster.md).

-   A Tablestore instance is created. We recommend that you use a high-performance instance.

    For more information, see [Create instances](/intl.en-US/Quick Start/Create instances.md).

    **Note:** You must enable the transaction feature.


In SmartData 2.6.X and later, Namespace Service allows you to use Tablestore instances to store metadata. You can bind a Tablestore instance to each EMR JindoFS cluster. Namespace Service creates a Tablestore table for each namespace to manage and store metadata.

The following figure shows the structure of Namespace Service in high availability \(HA\) mode with a Tablestore instance as the metadata storage backend.

![HA](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/9807409951/p101864.png)

## Configure a Tablestore instance

To use a Tablestore instance as the metadata storage backend, you must first bind the Tablestore instance to Namespace Service. Perform the following steps:

1.  Go to the SmartData service.

    1.  Log on to the [EMR console](https://emr.console.aliyun.com/).

    2.  In the top navigation bar, select the region where your cluster resides. Select the resource group as required. By default, all resources of the account appear.

    3.  Click the **Cluster Management** tab.

    4.  On the **Cluster Management** page that appears, find the target cluster and click **Details** in the Actions column.

    5.  In the left-side navigation pane, click **Cluster Service** and then **SmartData**.

2.  Configure bigboot parameters.

    1.  Click the **Configure** tab.

    2.  Click the **bigboot** tab in the Service Configuration section.

        ![bigboot](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/3907409951/p101653.png)

3.  Configure the parameters described in the following table.

    For example, you have created a Tablestore instance named emr-jfs in the China \(Hangzhou\) region. Your EMR cluster is deployed in a VPC network. You have obtained the AccessKey ID and AccessKey secret that are used to access the Tablestore instance.

    |Parameter|Description|Required|Example|
    |---------|-----------|--------|-------|
    |**namespace.backend.type**|The backend storage type of Namespace Service.     -   rocksdb
    -   ots
    -   raft
Default value: rocksdb. Set this parameter to ots.

|Yes|ots|
    |**namespace.ots.instance**|The name of the Tablestore instance.|Yes|emr-jfs|
    |**namespace.ots.accessKey**|The AccessKey ID that is used to access the Tablestore instance.|No|kkkkkk|
    |**namespace.ots.accessSecret**|The AccessKey secret that is used to access the Tablestore instance.|No|XXXXXX|
    |**namespace.ots.endpoint**|The endpoint of the Tablestore instance. We recommend that you use a VPC endpoint.|Yes|http://emr-jfs.cn-hangzhou.vpc.tablestore.aliyuncs.com|

4.  Save the configurations.

    1.  In the upper-right corner of the Service Configuration section, click **Save**.

    2.  In the **Confirm Changes** dialog box, specify Description and turn on **Auto-update Configuration**.

    3.  Click **OK**.

5.  Select **Restart Jindo Namespace Service** from the **Actions** drop-down list in the upper-right corner.


## Configure a Tablestore instance \(HA mode\)

If your EMR cluster is deployed in HA mode, we recommend that you deploy Namespace Service in HA mode.

![HA mode](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/2986693061/p102406.png)

In HA mode, Namespace Service supports automatic failover. If the active namespace fails, the client automatically switches services to the standby namespace.

![OTS](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/2819013061/p102397.png)

1.  Go to the bigboot tab for the SmartData service and perform the following operations:

    1.  Change the value of the **jfs.namespace.server.rpc-address** parameter to **emr-header-1:8101,emr-header-2:8101**.

    2.  In the upper-right corner of the Service Configuration section, click **Custom Configuration**. In the Add Configuration Item dialog box, add **namespace.backend.ots.ha** as a key and set this parameter to **true**.

    3.  Click **OK**.

    4.  Save the configurations.

        1.  In the upper-right corner of the Service Configuration section, click **Save**.
        2.  In the **Confirm Changes** dialog box, specify Description and turn on **Auto-update Configuration**.
        3.  Click **OK**.
2.  Select **Restart Jindo Namespace Service** from the **Actions** drop-down list in the upper-right corner.

3.  Select **Restart Jindo Storage Service** from the **Actions** drop-down list in the upper-right corner.


