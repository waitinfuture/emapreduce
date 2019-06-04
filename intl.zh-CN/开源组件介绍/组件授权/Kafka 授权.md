# Kafka 授权 {#concept_zys_5hc_bfb .concept}

如果没有开启 Kafka 认证（如 Kerberos 认证或者简单的用户名密码），即使开启了 Kafka 授权，用户也可以伪造身份访问服务。所以建议创建高安全模式，即支持Kerberos 的 Kafka 集群，详见[Kerberos 简介](intl.zh-CN/开源组件介绍/Kerberos认证/Kerberos 简介.md#)。

**说明：** 本文的权限配置只针对E-MapReduce的高安全模式集群，即Kafka以Kerberos的方式启动。

## 添加配置 {#section_nj3_djc_bfb .section}

1.  在**集群管理**页面，在需要添加配置的Kafka集群后单击**详情**。
2.  在左侧导航栏中单击**集群与服务管理**页签，然后单击服务列表中的 **Kafka**。
3.  在上方单击**配置**页签。
4.  在**服务配置**列表的右上角单击自定义配置，添加如下几个参数:

|key|value|备注|
|---|-----|--|
|authorizer.class.name|kafka.security.auth.SimpleAclAuthorizer|无|
|super.users|User:kafka|User:kafka 是必须的，可添加其它用户用分号\(;\)隔开|

**说明：** zookeeper.set.acl 用来设置 Kafka 在 ZooKeeper 中数据的权限，E-MapReduce 集群中已经设置为 true，所以此处不需要再添加该配置。该配置设置为 true 后，在 Kerberos 环境中，只有用户名称为 Kafka 且通过Kerberos 认证后才能执行 kafka-topics.sh 命令（kafka-topics.sh会直接读写/修改 ZooKeeper 中的数据）。

## 重启 Kafka 集群 {#section_vf1_m4c_bfb .section}

1.  在**集群管理**页面，在需要添加配置的 Kafka 集群后单击**详情**。
2.  在左侧导航栏中单击**集群与服务管理**页签，然后单击服务列表中的 Kafka 右侧的操作。
3.  在下拉菜单中选择 **RESTART All Components**，输入记录信息后单击**确定**。

## 授权（ACL） {#section_v3w_v4c_bfb .section}

-   基本概念

    Kafka 官方文档定义：

    ``` {#codeblock_ke0_yce_03m}
    Kafka acls are defined in the general format of "Principal P is [Allowed/Denied] Operation O From Host H On Resource R"
    ```

    即 ACL 过程涉及 Principal、Allowed/Denied、Operation、Host 和 Resource。

    -   Principal：用户名

        |安全协议|value|
        |----|-----|
        |PLAINTEXT|ANONYMOUS|
        |SSL|ANONYMOUS|
        |SASL\_PLAINTEXT|mechanism 为 PLAIN 时，用户名是 client\_jaas.conf 指定的用户名，mechanism 为 GSSAPI 时，用户名为 client\_jaas.conf 指定的 principal|
        |SASL\_SSL| |

    -   Allowed/Denied：允许/拒绝。
    -   Operation： 操作，包括 Read、Write、Create、DeleteAlter、Describe、ClusterAction、AlterConfigs、DescribeConfigs、IdempotentWrite 和 All。
    -   Host： 针对的机器。
    -   Resource： 权限作用的资源对象，包括 Topic、Group、Cluster 和 TransactionalId。
    Operation/Resource 的一些详细对应关系，如哪些 Resource 支持哪些 Operation 的授权，详见 [KIP-11 - Authorization Interface](https://cwiki.apache.org/confluence/display/KAFKA/KIP-11+-+Authorization+Interface)。

-   授权命令

    我们使用脚本 kafka-acls.sh （/usr/lib/kafka-current/bin/kafka-acls.sh） 进行Kafka授权，您可以直接执行`kafka-acls.sh --help`查看如何使用该命令。


## 操作示例 {#section_a4t_vpc_bfb .section}

在已经创建的 E-MapReduce 高安全 Kafka 集群的 master 节点上进行相关示例操作。

1.  新建用户 test，执行以下命令。

    `useradd test`

2.  创建 topic。

    第一节添加配置的备注中提到 zookeeper.set.acl=true，kafka-topics.sh 需要在 kafka 账号下执行，而且 kafka 账号下要通过 Kerberos 认证。

    ``` {#codeblock_tl5_tx5_51a}
    # kafka_client_jaas.conf中已经设置了kafka的Kerberos认证相关信息
    export KAFKA_HEAP_OPTS="-Djava.security.auth.login.config=/etc/ecm/kafka-conf/kafka_client_jaas.conf" 
    # zookeeper地址改成自己集群的对应地址(执行`hostnamed`后即可获取)
    kafka-topics.sh --create --zookeeper emr-header-1:2181/kafka-1.0.0 --replication-factor 3 --partitions 1 --topic test
    ```

3.  test 用户执行 kafka-console-producer.sh。
    1.  创建 test 用户的 keytab 文件，用于 zookeeper/kafka 的认证。

        ``` {#codeblock_e16_659_5tr}
        su root
        sh /usr/lib/has-current/bin/hadmin-local.sh /etc/ecm/has-conf -k /etc/ecm/has-conf/admin.keytab
        HadminLocalTool.local: #直接按回车可以看到一些命令的用法
        HadminLocalTool.local: addprinc #输入命令按回车可以看到具体命令的用法
        HadminLocalTool.local: addprinc -pw 123456 test #添加test的princippal,密码设置为123456
        HadminLocalTool.local: ktadd -k /home/test/test.keytab test #导出keytab文件，后续可使用该文件
        ```

    2.  添加 kafka\_client\_test.conf。

        如文件放到 /home/test/kafka\_client\_test.conf，内容如下:

        ``` {#codeblock_p60_h73_jt3}
        KafkaClient {
        com.sun.security.auth.module.Krb5LoginModule required
        useKeyTab=true
        storeKey=true
        serviceName="kafka"
        keyTab="/home/test/test.keytab"
        principal="test";
        };
        // Zookeeper client authentication
        Client {
        com.sun.security.auth.module.Krb5LoginModule required
        useKeyTab=true
        useTicketCache=false
        serviceName="zookeeper"
        keyTab="/home/test/test.keytab"
        principal="test";
        };
        ```

    3.  添加 producer.conf。

        如文件放到 `/home/test/producer.conf`，内容如下:

        ``` {#codeblock_6ks_47f_woe}
        security.protocol=SASL_PLAINTEXT
        sasl.mechanism=GSSAPI
        ```

    4.  执行 kafka-console-producer.sh。

        ``` {#codeblock_tkb_n2k_s6u}
        su test
        export KAFKA_HEAP_OPTS="-Djava.security.auth.login.config=/home/test/kafka_client_test.conf"
        kafka-console-producer.sh --producer.config /home/test/producer.conf --topic test --broker-list emr-worker-1:9092
        ```

        由于没有设置 ACL，所以上述会报错：

        ``` {#codeblock_z5r_ur9_2su}
        org.apache.kafka.common.errors.TopicAuthorizationException: Not authorized to access topics: [test]
        ```

    5.  设置 ACL。

        同样`kafka-acls.sh`也需要kafka账号执行。

        ``` {#codeblock_b1q_b4k_84q}
        su kafka
        export KAFKA_HEAP_OPTS="-Djava.security.auth.login.config=/etc/ecm/kafka-conf/kafka_client_jaas.conf" 
        kafka-acls.sh --authorizer-properties zookeeper.connect=emr-header-1:2181/kafka-1.0.0 --add --allow-principal User:test --operation Write --topic test
        ```

    6.  再执行 kafka-console-producer.sh。

        ``` {#codeblock_ic6_3bh_8qq}
        su test
        export KAFKA_HEAP_OPTS="-Djava.security.auth.login.config=/home/test/kafka_client_test.conf"
        kafka-console-producer.sh --producer.config /home/test/producer.conf --topic test --broker-list emr-worker-1:9092
        ```

        正常情况下显示如下：

        ``` {#codeblock_xdr_dw2_rth}
        [2018-02-28 22:25:36,178] INFO Kafka commitId : aaa7af6d4a11b29d (org.apache.kafka.common.utils.AppInfoParser)
        >alibaba
        >E-MapReduce
        >
        ```

4.  test 用户执行`kafka-console-consumer.sh`。

    上面成功执行 kafka-console-producer.sh，并往 topic 里面写入一些数据后，就可以执行`kafka-console-consumer.sh`进行消费测试.

    1.  添加 consumer.conf。

        如文件放到/home/test/consumer.conf，内容如下:

        ``` {#codeblock_jri_k1p_w1v}
        security.protocol=SASL_PLAINTEXT
        sasl.mechanism=GSSAPI
        ```

    2.  执行 kafka-console-consumer.sh。

        ``` {#codeblock_1u1_fy2_rgo}
        su test
        #kafka_client_test.conf跟上面producer使用的是一样的
        export KAFKA_HEAP_OPTS="-Djava.security.auth.login.config=/home/test/kafka_client_test.conf"
        kafka-console-consumer.sh --consumer.config consumer.conf  --topic test --bootstrap-server emr-worker-1:9092 --group test-group  --from-beginning
        ```

        由于未设置权限，会报以下错误：

        ``` {#codeblock_7tk_yh6_5lw}
        org.apache.kafka.common.errors.GroupAuthorizationException: Not authorized to access group: test-group
        ```

    3.  设置 ACL。

        ``` {#codeblock_bgl_f58_s7p}
        su kafka
        export KAFKA_HEAP_OPTS="-Djava.security.auth.login.config=/etc/ecm/kafka-conf/kafka_client_jaas.conf" 
        # test-group权限
        kafka-acls.sh --authorizer-properties zookeeper.connect=emr-header-1:2181/kafka-1.0.0 --add --allow-principal User:test --operation Read --group test-group
        # topic权限
        kafka-acls.sh --authorizer-properties zookeeper.connect=emr-header-1:2181/kafka-1.0.0 --add --allow-principal User:test --operation Read --topic test
        ```

    4.  再执行`kafka-console-consumer.sh`。

        ``` {#codeblock_463_dm4_m0v}
        su test
        # kafka_client_test.conf跟上面producer使用的是一样的
        export KAFKA_HEAP_OPTS="-Djava.security.auth.login.config=/home/test/kafka_client_test.conf"
        kafka-console-consumer.sh --consumer.config consumer.conf  --topic test --bootstrap-server emr-worker-1:9092 --group test-group  --from-beginning
        ```

        正常输出：

        ``` {#codeblock_qm3_969_c83}
        alibaba
        E-MapReduce
        ```


