# Kafka Manager guide {#concept_xqc_3hd_z2b .concept}

E-MapReduce 3.4.0 and later versions support the Kafka Manager service for managing Kafka clusters.

## Procedure {#section_yjv_b3d_z2b .section}

**Note:** By default, the Kafka Manager software is installed and the Kafka Manager authentication function is enabled when a Kafka cluster is created. We strongly recommend that you change the default password when using Kafka Manager for the first time and access Kafka Manager through the SSH tunnel. We do not recommend that you expose Port 8085 to the public network unless an IP address whitelist is configured to avoid data leakage.

-   We recommend you access the web page through the SSH tunnel. See [Connect to clusters using SSH](intl.en-US/User Guide/Connect to clusters using SSH.md#) User Guide \> Connect to Cluster through SSH for more information.
-   Access http://localhost:8085.
-   Enter your username and password. Refer to the configuration information of Kafka Manager.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17903/154218659510849_en-US.png)

-   Add an existing Kafka cluster and make sure that the Zookeeper address of the Kafka cluster is correct. For more information, see the configuration information of Kafka Manager. Select the corresponding Kafka version. We recommend you enable the JMX function.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17903/154218659510850_en-US.png)

-   Common Kafka functions are available immediately after you create a Kafka cluster.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17903/154218659510851_en-US.png)


