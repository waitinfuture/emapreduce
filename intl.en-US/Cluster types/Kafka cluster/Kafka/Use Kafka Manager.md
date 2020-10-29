# Use Kafka Manager

This topic describes how to use the Kafka Manager service to manage an EMR Kafka cluster.

An EMR Kafka cluster is created. For more information, see [Create a cluster](/intl.en-US/Cluster Management/Configure clusters/Create a cluster.md).

**Note:** By default, when you create a Kafka cluster, the Kafka Manager service is installed and the authentication feature of the service is enabled.

1.  Log on to the Kafka Manager web UI in SSH mode. For more information, see [Create an SSH tunnel to access web UIs of open source components](/intl.en-US/Cluster Management/Configure clusters/Connect to a cluster/Create an SSH tunnel to access web UIs of open source components.md)

    **Note:**

    -   We recommend that you change the password if this is the first time you use the Kafka Manager service.
    -   To prevent port 8085 from being exposed, we recommend that you log on to the web UI in SSH mode. If you want to use http://localhost:8085 to log on to the web UI, you must configure an IP address whitelist to avoid data leak.
2.  On the logon page, enter the username and password configured for Kafka Manager.

    You can perform the following steps to obtain the username and password:

    1.  Log on to the [EMR console](https://emr.console.aliyun.com/).
    2.  In the top navigation bar, select the region where your cluster residesand select a resource group based on your business requirements.
    3.  Click the **Cluster Management** tab.
    4.  On the page that appears, find the cluster and click **Details** in the Actions column.
    5.  In the left-side navigation pane, click **Cluster Service** and then **Kafka-Manager**.
    6.  Click the **Configure** tab. In the **Service Configuration** section, obtain the values of the following parameters:

        -   **kafka.manager.authentication.username**: the username that is used to log on to the Kafka Manager web UI
        -   **kafka.manager.authentication.password**: the password that is used to log on to the Kafka Manager web UI
        -   kafka.manager.zookeeper.hosts: the ZooKeeper hosts of the Kafka cluster
        ![kafka configure](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/2630160061/p66976.png)

3.  On the Kafka Manager web UI, add your Kafka cluster.

    1.  Enter the cluster name.

    2.  Configure the ZooKeeper hosts of the Kafka cluster.

        Enter the value of the kafka.manager.zookeeper.hosts parameter that you obtained in [Step 2](#step_ch3_0jd_29k).

    3.  Select the Kafka version that corresponds to the Kafka cluster.

    4.  We recommend that you enable JMX polling.

        ![Add a Kafka cluster](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/2630160061/p10850.png)

        After you add the Kafka cluster, you can use the common Kafka features.

        ![Common Kafka features](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/2630160061/p10851.png)


