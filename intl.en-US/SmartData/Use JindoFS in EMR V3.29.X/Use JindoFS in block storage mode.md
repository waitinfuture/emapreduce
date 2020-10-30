# Use JindoFS in block storage mode

The block storage mode ensures efficient read/write operations and high metadata accessibility. JindoFS stores data as blocks in OSS and caches data in local disks of clusters to accelerate data access. JindoFS uses Namespace Service to manage metadata and ensure high metadata accessibility. This topic describes how to use JindoFS in block storage mode.

The block storage mode of JindoFS has the following features:

-   JindoFS offers tremendous and scalable storage capacity by using OSS as the storage backend. The storage capacity is independent of the EMR cluster scale. The local cluster can be scaled in or out as required.
-   JindoFS stores some backup data in the local cluster to accelerate read operations. This improves the throughput by using limited local storage capacity, especially for Write Once Read Many \(WORM\) solutions.
-   JindoFS provides efficient metadata query similar to HDFS. Compared with OssFileSystem, JindoFS saves much time in metadata query. In addition, JindoFS avoids system instability when data and metadata are frequently accessed.
-   JindoFS ensures maximal data locality when jobs are executed in the EMR cluster. This reduces the load on network transmission and improves the read performance.

## Configure the block storage mode

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

3.  Configure required parameters.

    JindoFS allows you to configure multiple namespaces. A namespace named test is used in this topic.

    1.  Set **jfs.namespaces** to **test**.

        If you configure multiple namespaces, separate them with commas \(,\).

    2.  In the upper-right corner of the Service Configuration section, click **Custom Configuration**. In the **Add Configuration Item** dialog box, add the parameters described in the following table.

        |Parameter|Description|Example|
        |---------|-----------|-------|
        |jfs.namespaces.test.oss.uri|The storage backend of the test namespace.|oss://<oss\_bucket\>/<oss\_dir\>/**Note:** We recommend that you set this parameter to a directory of an OSS bucket. The namespace stores data blocks in this directory. |
        |jfs.namespaces.test.mode|The storage mode of the test namespace. Set this parameter to block.|block|
        |jfs.namespaces.test.oss.access.key|The AccessKey ID of the OSS bucket that serves as the storage backend.|xxxx**Note:** We recommend that you store data in an OSS bucket that is in the same region and under the same account as your EMR cluster. This ensures high performance and stability. In this case, you do not need to configure the AccessKey ID and AccessKey secret because the OSS bucket allows password-free access from the EMR cluster. |
        |jfs.namespaces.test.oss.access.secret|The AccessKey secret of the OSS bucket that serves as the storage backend.|

    3.  Click **OK**.

4.  In the upper-right corner of the Service Configuration section, click **Save**.

5.  Select **Restart Jindo Namespace Service** from the **Actions** drop-down list in the upper-right corner.

    After Namespace Service is restarted, you can use `jfs://test/<path_of_file>` to access files in JindoFS.


## Control disk space usage

JindoFS uses OSS as the data storage backend, which allows you to store large volumes of data. However, the capacity of local disks is limited. JindoFS automatically deletes cold data in local disks. The `storage.watermark.high.ratio` and `storage.watermark.low.ratio` parameters are used to adjust the space usage of local disks. You can set the parameters to decimal numbers between 0 and 1.

1.  Modify disk usage configurations.

    In the **Service Configuration** section for the SmartData service, click the **storage** tab and configure the parameters described in the following table.

    ![storage](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/8780113061/p161207.png)

    |Parameter|Description|
    |---------|-----------|
    |**storage.watermark.high.ratio**|The upper limit of disk usage. When the disk usage of JindoFS data exceeds this limit, JindoFS automatically deletes data in the disk. Default value: 0.4.|
    |**storage.watermark.low.ratio**|The lower limit of disk usage. After automatic data deletion is triggered, JindoFS starts to delete data until the disk usage of JindoFS data is reduced to this limit. Default value: 0.2.|

    **Note:** You can configure the upper limit and lower limit to adjust the disk space assigned to JindoFS. Make sure that the upper limit is greater than the lower limit.

2.  Save the configurations.

    1.  In the upper-right corner of the Service Configuration section, click **Save**.

    2.  In the **Confirm Changes** dialog box, specify Description and turn on **Auto-update Configuration**.

    3.  Click **OK**.

3.  Restart Jindo Storage Service to apply the configurations.

    1.  Select **Restart Jindo Storage Service** from the **Actions** drop-down list in the upper-right corner.

    2.  In the **Cluster Activities** dialog box, specify the related parameters.

    3.  Click **OK**.

    4.  In the **Confirm** message, click **OK**.


