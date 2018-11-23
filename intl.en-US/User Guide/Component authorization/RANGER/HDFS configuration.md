# HDFS configuration {#concept_oj5_b3d_bfb .concept}

## Integrate Ranger into HDFS {#section_qf5_d3d_bfb .section}

The previous section introduced how to create and start the cluster of the Ranger service in E-MapReduce. This section describes the step-by-step process for integrating Ranger into HDFS.

-   Enable HDFS Plugin
    1.  On Cluster Management page, click **Manage** after the cluster you want to operate in the **Actions** column.
    2.  Click **Ranger** in the service list to enter the Ranger Management page.
    3.  On the ranger configuration page, click the **Actions** drop-down menu in the upper-right corner, and select **Enable HDFS PLUGIN**, then click **OK**.

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17949/154294488811456_en-US.png)

    4.  Enter record information in the prompt box, and then click **OK**.

        You can check the progress by clicking **View Operation Logs** in the upper right corner of the page.

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17949/154294488811459_en-US.png)

-   Restart NameNode

    After the preceding task is completed, you need to restart NameNode. To restart NameNode, perform the following steps:

    1.  In the Ranger management page, click the inverted triangle icon behind RANGER in the upper left corner to switch to HDFS.
    2.  Click **Actions** in the upper right corner of the page and select RESTART NameNode.
    3.  You can check the progress by clicking **View Operation Logs** in the upper right corner of the page.

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17949/154294488811463_en-US.png)

-   Add HDFS service on Ranger WebUI

    About how to enter the Ranger UI page, see [Ranger introdcution](intl.en-US/User Guide/Component authorization/RANGER/Ranger introduction.md#).

    Add HDFS service.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17949/154294488811479_en-US.png)

    -   Standard cluster

        If the cluster you created is in standard mode \(go to the **Cluster Overview** page of the cluster to check Security Mode to make sure whether it is or not\), configure as follows:

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17949/154294488811480_en-US.png)

    -   High-security-mode cluster

        If the cluster you created is in High-security mode\(go to the go to the **Cluster Overview** page of the cluster to check Security Mode to make sure whether it is or not\), configure as follows:

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17949/154294488911481_en-US.png)


## Permission Configuration Example {#section_osm_th3_bfb .section}

The preceding section has already integrated Ranger into HDFS, which allows you to set relevant permissions. For example, authorize user test the write or execute permission of the /user/foo.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17949/154294488911482_en-US.png)

Click**emr-hdfs** in the preceding figure to enter the policy configuration page.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17949/154294488911483_en-US.png)

In terms of the preceding settings, the permissions are granted to user test. User test can access the HDFS path of /user/foo.

**Note:** The policy takes effect in 1 minute after it is added.

