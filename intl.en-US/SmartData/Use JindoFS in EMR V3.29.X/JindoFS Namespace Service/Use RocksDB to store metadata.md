# Use RocksDB to store metadata

Namespace Service of JindoFS supports different metadata storage backends. By default, RocksDB is used. This topic describes how to configure RocksDB as the metadata storage backend.

RocksDB cannot be configured in high availability mode. If you need to configure a metadata storage backend in high availability mode, we recommend that you use Tablestore or Raft instances. For more information, see [Use Tablestore instances to store metadata](/intl.en-US/SmartData/Get started with JindoFS (EMR-3.27.0 or later)/JindoFS/Use Tablestore instances to store metadata.md) and [t1886330.md\#](/intl.en-US/SmartData/Get started with JindoFS (EMR-3.27.0 or later)/JindoFS/Use Raft-RocksDB-Tablestore to store metadata.md).

The following figure shows the structure of a single RocksDB instance for Namespace Service.

![RocksDB](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/0907409951/p102002.png)

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

3.  Set **namespace.backend.type** to **rocksdb**.

4.  Save the configurations.

    1.  In the upper-right corner of the Service Configuration section, click **Save**.

    2.  In the **Confirm Changes** dialog box, specify Description and turn on **Auto-update Configuration**.

    3.  Click **OK**.

5.  Select **Restart Jindo Namespace Service** from the **Actions** drop-down list in the upper-right corner.


