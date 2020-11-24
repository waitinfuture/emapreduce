# Kafka

This topic describes how to integrate Kafka with Ranger and how to configure related permissions.

An E-MapReduce \(EMR\) Hadoop cluster is created, and Ranger is selected from the optional services during the cluster creation. For more information about how to create a cluster, see [Create a cluster](/intl.en-US/Cluster Management/Configure clusters/Create a cluster.md).

## Integrate Kafka with Ranger

1.  Enable Kafka in Ranger.

    1.  Log on to the [EMR console](https://emr.console.aliyun.com/).

    2.  In the top navigation bar, select the region where your cluster residesand select a resource group based on your business requirements.

    3.  Click the **Cluster Management** tab.

    4.  On the **Cluster Management** page, find your cluster and click **Details** in the Actions column.

    5.  In the left-side navigation pane, choose Cluster Service \> RANGER. On the page that appears, choose **Actions** \> **EnabledKafka** in the upper-right corner.

        ![Enable Kafka PLUGIN](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/5114027951/p11548.png)

    6.  In the **Cluster Activities** dialog box, configure relevant parameters and click **OK**. In the **Confirm** message, click **OK**.

        Click **History** in the upper-right corner to view the task progress.

2.  Add the Kafka service on the web UI of Ranger.

    1.  Log on to Ranger. For more information, see [Overview](/intl.en-US/Cluster types/Hadoop cluster/Ranger/Overview.md).

    2.  Add the Kafka service.

        ![Ranger WebUI](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/5114027951/p10841.png)

    3.  Configure the Kafka service.

        ![add_service](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/5114027951/p81272.png)

        |Parameter|Description|
        |---------|-----------|
        |**Service Name**|Set the value to **emr-kafka**.|
        |**Username**|Set the value to **kafka**.|
        |**Password**|Enter a custom value.|
        |**Zookeeper Connect String**|Enter a value in the format of **emr-header-1:2181/kafka-x.xx**. **Note:** Configure `kafka-x.xx` based on the actual Kafka version. |

    4.  Click **Add**.

3.  Restart the Kafka broker.

    1.  In the left-side navigation pane, choose **Cluster Service** \> **Kafka**.

    2.  On the page that appears, choose **Actions** \> **Restart Kafka Broker** in the upper-right corner.

    3.  In the **Cluster Activities** dialog box, configure relevant parameters and click **OK**. In the **Confirm** message, click **OK**.

        Click **History** in the upper-right corner to view the task progress.


## Example of permission configuration

After Kafka is integrated with Ranger, you can configure Kafka permissions in Ranger.

**Note:** By default, Ranger generates the all - topic policy in a standard cluster after the Kafka service is added. This policy does not limit the permissions of users but allows users to perform all operations. In this case, Ranger cannot control the permissions of the users.

Take user test as an example. To grant the Publish permission to user test, perform the following steps:

1.  Log on to Ranger. For more information, see [Overview](/intl.en-US/Cluster types/Hadoop cluster/Ranger/Overview.md).

2.  Click **emr-kafka**.

    ![click_emr_kafka](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/8087593061/p81278.png)

3.  Click **Add New Policy** in the upper-right corner.

4.  Configure parameters.

    ![add_Policy](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/5114027951/p81282.png)

    |Parameter|Description|
    |---------|-----------|
    |**Policy Name**|The name of the policy. You can customize a name.|
    |**topic**|The topic on which permissions are configured. You can specify multiple topics. Press Enter after you specify each topic.|
    |**Select Group**|The user group to which you want to add this policy.|
    |**Select User**|The user to whom you want to add this policy.|
    |**Permissions**|The permissions to be granted. Click ![add_permissions](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/6114027951/p81288.png) and select **Publish**.|

    Click ![add_permissions](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/6114027951/p81288.png) below **Select Group** to authorize multiple groups.

5.  Click **Add**.

    After the policy is created, authorization is completed. User test can write data into the **test** topic.

    **Note:** After you add, remove, or modify a policy, it takes about one minute for the configuration to take effect.


