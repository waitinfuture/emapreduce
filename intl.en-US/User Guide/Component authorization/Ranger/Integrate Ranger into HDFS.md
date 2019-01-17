# Integrate Ranger into HDFS {#concept_oj5_b3d_bfb .concept}

## Procedure {#section_qf5_d3d_bfb .section}

This section describes the step-by-step process for integrating Ranger into HDFS.

-   Enable the HDFS plug-in
    1.  On the Cluster Management page, click **Manage** next to the cluster you want to operate in the **Actions** column.
    2.  Click **Ranger** in the service list to enter the Ranger Management page.
    3.  On the Ranger Configuration page, click the **Actions** drop-down menu in the upper-right corner, select **Enable HDFS PLUGIN**, and click **OK**.

        ![Enable HDFS PLUGIN](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17949/154771023711456_en-US.png)

    4.  Enter the record information in the prompt box and click **OK**.

        You can check the progress by clicking **View Operation Logs** in the upper-right corner of the page.

        ![View Operation Logs](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17949/154771023711459_en-US.png)

-   Restart NameNode

    After enabling the HDFS plug-in, you need to restart NameNode. To do so, complete the following steps:

    1.  In the Ranger Management page, click the Ranger drop-down menu in the upper-left corner, and select HDFS.
    2.  Click **Actions** in the upper-right corner of the page and select **RESTART NameNode**.
    3.  You can check the progress by clicking **View Operation Logs** in the upper-right corner of the page.

        ![View Operation Logs ](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17949/154771023711463_en-US.png)

-   Add the HDFS service to Ranger UI

    For more information about how to access the Ranger UI, see [Introduction to Ranger](reseller.en-US/User Guide/Component authorization/Ranger/Introduction to Ranger.md#).

    Add the HDFS service.

    ![Ranger UI](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17949/154771023711479_en-US.png)

    -   Standard cluster

        To check the mode of the cluster you created, go to the **Cluster Overview** page. If your cluster is in standard mode, configure it as follows:

        ![Standard cluster](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17949/154771023711480_en-US.png)

    -   High-security-mode cluster

        To check the mode of the cluster you created, go to the **Cluster Overview** page. If your cluster is in high-security mode, configure it as follows:

        ![High-security-mode cluster](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17949/154771023811481_en-US.png)


## Permission configuration {#section_osm_th3_bfb .section}

After integrating Ranger into HDFS, you can set permissions, such as granting the test user the write or execute permission for /user/foo.

![Permission configuration](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17949/154771023811482_en-US.png)

In the preceding figure, click **emr-hdfs** to enter the policy configuration page.

![Policy configuration](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17949/154771023811483_en-US.png)

Permissions are granted to the test user. They can now access the HDFS path of /user/foo.

**Note:** The policy takes effect one minute after it is added.

