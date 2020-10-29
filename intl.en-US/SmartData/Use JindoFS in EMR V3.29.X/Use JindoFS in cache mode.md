# Use JindoFS in cache mode

When you use JindoFS in cache mode, files are stored as objects in Object Storage Service \(OSS\), and the frequently used files are cached in an EMR cluster to improve the data access efficiency. In cache mode, JindoFS can access files in OSS without the need to convert the file formats, and JindoFS is fully compatible with OSS clients. This topic describes how to use JindoFS in cache mode.

In cache mode, JindoFS supports the object semantics of OSS and is fully compatible with various OSS clients. This ensures that you can access files in OSS without the need to migrate data or convert data formats. In cache mode, JindoFS caches frequently used files in an EMR cluster. This improves the read and write performance and relieves pressure on bandwidth.

## Methods to access files in OSS

In cache mode, you can use one of the following methods to access files in OSS based on your business requirements:

-   OSS Scheme

    For more information, see [\(Recommended\) Configure OSS Scheme](#section_f4w_h8l_gd0).

-   JFS Scheme

    For more information, see [Configure JFS Scheme](#section_kvg_dk2_4hm).


## \(Recommended\) Configure OSS Scheme

OSS Scheme refers to the original method of accessing files in OSS. You can use the `oss://<bucket_name>/<path_of_your_file>` command to access files in OSS. After you create an EMR cluster, you can use this method to access files in OSS without additional configurations. You can also run existing jobs to read or write data from or to OSS without the need to modify the configurations.

## Configure JFS Scheme

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

3.  Configure the required parameters.

    JindoFS allows you to configure multiple namespaces. A namespace named test is used in this topic.

    1.  Set **jfs.namespaces** to **test**.

        If you configure multiple namespaces, separate them with commas \(,\).

    2.  Click **Custom Configuration**. In the **Add Configuration Item** dialog box, configure the parameters described in the following table and click OK.

        |Parameter|Description|Example|
        |---------|-----------|-------|
        |jfs.namespaces.test.oss.uri|The storage backend of the test namespace.|oss://<oss\_bucket\>/<oss\_dir\>/**Note:** Set this parameter to a directory for a specific OSS bucket or the root directory. |
        |jfs.namespaces.test.mode|The storage mode of the test namespace. Set this parameter to cache.|cache|

4.  In the upper-right corner of the Service Configuration section, click **Save**.

5.  Select **Restart Jindo Namespace Service** from the **Actions** drop-down list in the upper-right corner.

    After Namespace Service is restarted, you can use `jfs://test/<path_of_file>` to access files in JindoFS.


## Enable local cache

After you enable local cache, hot data blocks are cached on local disks. By default, this feature is disabled, and EMR directly reads data from OSS.

1.  In the left-side navigation pane, click **Cluster Service** and then **SmartData**. On the SMARTDATA page, click the **Configure** tab. In the Service Configuration section, click the **client** tab.

2.  Set **jfs.cache.data-cache.enable** to **1** to enable local cache.

    The configuration immediately takes effect on the client, without the need to restart the SmartData service.


After you enable local cache, Jindo automatically manages cached data. It clears cache based on the high and low watermarks that you configured. For more information about how to configure the watermarks, see [Control disk space usage](/intl.en-US/SmartData/Get started with JindoFS (EMR-3.27.0 or later)/Use JindoFS in block storage mode.md).

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


## Access an OSS bucket

If you access an OSS bucket that is under the same Alibaba Cloud account and in the same region as your EMR cluster, you do not need to configure an AccessKey pair. In other cases, you must configure an AccessKey pair and an OSS bucket endpoint. Configure the parameters based on the method that you use to access files in OSS:

-   OSS Scheme
    1.  In the left-side navigation pane, click **Cluster Service** and then **SmartData**. On the SMARTDATA page, click the **Configure** tab. In the Service Configuration section, click the **smartdata-site** tab.
    2.  Click **Custom Configuration**. In the **Add Configuration Item** dialog box, configure the parameters described in the following table and click **OK**.

        |Parameter|Description|
        |---------|-----------|
        |**fs.jfs.cache.oss-accessKeyId**|The AccessKey ID of the OSS bucket that serves as the storage backend.|
        |**fs.jfs.cache.oss-accessKeySecret**|The AccessKey secret of the OSS bucket that serves as the storage backend.|
        |**fs.jfs.cache.oss-endpoint**|The endpoint of the OSS bucket that serves as the storage backend.|

-   JFS Scheme
    1.  In the left-side navigation pane, choose **Cluster Service** \> **SmartData**. On the SMARTDATA page, click the **Configure** tab. In the Service Configuration section, click the **namespace** tab.
    2.  Set **jfs.namespaces** to **test**.
    3.  Click **Custom Configuration**. In the **Add Configuration Item** dialog box, configure the parameters described in the following table and click **OK**.

        |Parameter|Description|
        |---------|-----------|
        |**jfs.namespaces.test.oss.uri**|The storage backend of the test namespace. Example: oss://<oss\_bucket.endpoint\>/<oss\_dir\>. The OSS bucket endpoint is specified in this parameter. |
        |**jfs.namespaces.test.oss.access.key**|The AccessKey ID of the OSS bucket that serves as the storage backend.|
        |**jfs.namespaces.test.oss.access.secret**|The AccessKey secret of the OSS bucket that serves as the storage backend.|


## Advanced configurations

You can configure some advanced parameters to optimize cache performance. After you configure the parameters, you do not need to restart the SmartData service. The configurations take effect immediately on the client.

-   In the **Service Configuration** section, click the **client** tab and configure the parameters described in the following table.

    |Parameter|Description|
    |---------|-----------|
    |**client.oss.upload.threads**|The number of OSS upload threads for each data write stream. Default value: 4.|
    |**client.oss.upload.max.parallelism**|The maximum number of concurrent OSS upload threads of a process. This parameter prevents upload threads from occupying an excessive amount of bandwidth and memory. Default value: 16.|

-   In the **Service Configuration** section, click the **smartdata-site** tab and configure the parameters described in the following table.

    |Parameter|Description|
    |---------|-----------|
    |**fs.jfs.cache.copy.simple.max.byte**|The threshold for the size of a file that is renamed over a common copy interface. If the size of a file is smaller than this threshold, a common copy interface is used. If the size is larger than this threshold, the Multipart Copy interface is used to improve copy efficiency. **Note:** If you have enabled the fast copy feature of OSS, set this parameter to **-1**. This value indicates that all files are renamed over a common copy interface. This way, you can obtain the optimal rename performance. |
    |**fs.jfs.cache.write.buffer.size**|The buffer size of data write streams. Unit: bytes. You must set this parameter to a power of 2. The maximum value is 8388608 \(8 MB\). If too much memory is occupied by write streams, we recommend that you set this parameter to a small value. Default value: 1048576.|
    |**fs.oss.committer.magic.enabled**|Specifies whether to enable Jindo Job Committer. This Job Committer does not require rename operations and improves job commit performance. Default value: true. **Note:** In cache mode, the performance of renaming files in OSS is less than standard. We recommend that you use Jindo Job Committer. |


