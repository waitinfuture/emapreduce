# Configure parameters

EMR supports configuring parameters for components including HDFS, Yarn, Spark, Kafka, and Druid.

## Configure parameters for existing components

Perform the following steps to modify the parameters for existing components.

1.  Log on to the [Alibaba Cloud E-MapReduce console](https://emr.console.aliyun.com/).
2.  Log on to the [Alibaba Cloud E-MapReduce console](https://partners-intl.console.aliyun.com/#/emr).
3.  Click the **Cluster Management** tab to jump to the Cluster Management page.
4.  Click a cluster ID to jump to the Clusters and Services page as needed.
5.  In the Services list, click the component you want to modify parameters. For example, **HDFS**.

    ![Services](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/3609129951/p38139.png)

6.  Click the **Configurations** tab to jump to the Service Configuration page.

    ![Configure the service](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/3609129951/p38140.png)

7.  You can select configuration types and configuration files in the **Quick Configuration** area.
8.  Locate the parameter you want to modify and set the value of the parameter. If you cannot find the parameter you want to modify, click **Custom Configuration** to add a configuration item.

    ![](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/3609129951/p38141.png)

    -   Key: parameter name
    -   Value: parameter value
    -   Description: parameter description
9.  Click **Save**.

Refer to the [Deploy configurations](#section_gkw_dnm_4gb) section for making your parameter modification take effect.

## Deploy configurations

![](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/6721309951/p38143.png)

-   Deploy client configurations
    1.  After modifying a client configuration parameter, click **Deploy Client Config** and the **Run Action** dialog box is displayed.
    2.  Click All Matched or Designated for the execution scope.
    3.  Enter the execution reason.
    4.  Click **OK** to make the modification take effect.
    5.  You can click **View Operation Logs** to view the status and progress of the execution.
-   Deploy server configurations
    1.  After modifying a server configuration parameter, restart the corresponding service.
    2.  Click **Deploy Client Config** and the **Run Action** dialog box pops up.
    3.  Click the All Matched radio button or the Designated radio button for the execution scope.
    4.  Select the Scrolling Execute check box if you need to perform a rolling restart.
    5.  Enter the execution reason.
    6.  Click **OK** to start the rolling execution.
    7.  You can click **View Operation Logs** to view the status and progress of the execution.

## Roll back parameter configurations

Perform the following steps to roll back parameter configurations.

1.  Log on to the [Alibaba Cloud E-MapReduce console](https://emr.console.aliyun.com/).
2.  Log on to the [Alibaba Cloud E-MapReduce console](https://partners-intl.console.aliyun.com/#/emr).
3.  Click the **Cluster Management** tab to jump to the Cluster Management page.
4.  Click the cluster ID that you want to jump to the Clusters and Services page.
5.  In the Services list, click the component that you want to modify parameters. For example, **HDFS**.
6.  On the HDFS page, click the **Configuration Change History** tab to jump to the Configuration Change History tab page.
7.  Click **roll back** in the Actions column for a record to roll back the corresponding parameter configurations.

See the [Deloy configurations](#section_gkw_dnm_4gb) section for making a configuration rollback take effect.

