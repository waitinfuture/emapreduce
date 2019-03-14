# Kafka Manager {#concept_xqc_3hd_z2b .concept}

E-MapReduce 3.4.0 and later support Kafka Manager for use in managing Kafka clusters.

## Procedure {#section_yjv_b3d_z2b .section}

**Note:** Kafka Manager software is installed by default and the Kafka Manager authentication function is enabled when a Kafka cluster is created. We strongly recommend that you change the default password when using Kafka Manager for the first time and access Kafka Manager through the SSH tunnel. We do not recommend that you expose Port 8085 to the public network unless an IP address whitelist is configured to avoid data leakage.

-   We recommend that you access the web page through the SSH tunnel. For more information, see [Connect to clusters using SSH](reseller.en-US/Quick Start/Connect to clusters using SSH.md#).
-   Access *http://localhost:8085*.
-   Enter your user name and password. Refer to the configuration information of Kafka Manager.

    ![configuration information of Kafka Manager](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17903/155255170110849_en-US.png)

-   Add an existing Kafka cluster and make sure that the Zookeeper address of the Kafka cluster is correct. For more information, see the configuration information of Kafka Manager. Select the corresponding Kafka version. We recommend that you enable the JMX function.

    ![Add Kafka cluster](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17903/155255170110850_en-US.png)

-   Common Kafka functions are available immediately after you create a Kafka cluster.

    ![Common Kafka functions](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17903/155255170110851_en-US.png)


