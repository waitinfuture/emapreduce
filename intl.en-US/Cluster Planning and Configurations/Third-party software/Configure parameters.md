# Configure parameters {#concept_ucb_bjh_4gb .concept}

EMR supports configuring parameters for components including HDFS, Yarn, Spark, Kafka, and Druid.

## Configure parameters for existing components {#section_plt_smh_4gb .section}

Perform the following steps to modify the parameters for existing components.

1.  Log on to the [Alibaba Cloud E-MapReduce console](https://partners-intl.console.aliyun.com/#/emr).
2.  Click the **Cluster Management** tab to jump to the Cluster Management page.
3.  Click a cluster ID to jump to the Clusters and Services page as needed.
4.  In the Services list, click the component you want to modify parameters. For example, **HDFS**.

    ![Services](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/119950/155928423238139_en-US.png)

5.  Click the **Configurations** tab to jump to the Service Configuration page.

    ![Configure the service](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/119950/155928423238140_en-US.png)

6.  You can select configuration types and configuration files in the **Quick Configuration** area.
7.  Locate the parameter you want to modify and set the value of the parameter. If you cannot find the parameter you want to modify, click **Custom Configuration** to add a configuration item.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/119950/155928423338141_en-US.png)

    -   Key: parameter name
    -   Value: parameter value
    -   Description: parameter description
8.  Click **Save**.

Refer to the [Deploy configurations](#section_gkw_dnm_4gb) section for making your parameter modification take effect.

## Deploy configurations {#section_gkw_dnm_4gb .section}

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/119950/155928423338143_en-US.png)

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

## Roll back parameter configurations {#section_ocw_2qm_4gb .section}

Perform the following steps to roll back parameter configurations.

1.  Log on to the [Alibaba Cloud E-MapReduce console](https://partners-intl.console.aliyun.com/#/emr).
2.  Click the **Cluster Management** tab to jump to the Cluster Management page.
3.  Click the cluster ID that you want to jump to the Clusters and Services page.
4.  In the Services list, click the component that you want to modify parameters. For example, **HDFS**.
5.  On the HDFS page, click the **Configuration Change History** tab to jump to the Configuration Change History tab page.
6.  Click **roll back** in the Actions column for a record to roll back the corresponding parameter configurations.

See the [Deloy configurations](#section_gkw_dnm_4gb) section for making a configuration rollback take effect.

