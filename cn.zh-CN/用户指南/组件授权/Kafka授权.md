# Kafka授权 {#concept_zys_5hc_bfb .concept}

## Kafka授权 {#section_wnf_33c_bfb .section}

如果没有开启Kafka认证\(如Kerberos认证或者简单的用户名密码\)，即使开启了Kafka授权，用户也可以伪造身份访问服务。所以建议创建高安全模式\(即支持Kerberos\)的Kafka集群，详见[Kerberos安全文档](https://help.aliyun.com/document_detail/57730.html?spm=5176.product28066.6.623.a91nzk)。

**说明：** 本文的权限配置只针对E-MapReduce的高安全模式集群，即Kafka以Kerberos的方式启动。

## 添加配置 {#section_nj3_djc_bfb .section}

1.  在**集群列表**页面，在需要添加配置的Kafka集群后单击**详情**。
2.  在左侧导航栏中单击**集群与服务管理**页签，然后单击服务列表中的**Kafka**。
3.  在上方单击**配置**页签。
4.  在**服务配置**列表的右上角单击自定义配置，添加如下几个参数:

|key|value|备注|
|---|-----|--|
|authorizer.class.name|kafka.security.auth.SimpleAclAuthorizer|无|
|super.users|User:kafka|User:kafka是必须的，可添加其它用户用分号\(;\)隔开|

**说明：** zookeeper.set.acl用来设置Kafka在ZooKeeper中数据的权限，E-MapReduce集群中已经设置为true，所以此处不需要再添加该配置。该配置设置为true后，在Kerberos环境中，只有用户名称为kafka且通过Kerberos认证后才能执行kafka-topics.sh命令\(kafka-topics.sh会直接读写/修改ZooKeeper中的数据\)。

## 重启Kafka集群 {#section_vf1_m4c_bfb .section}

1.  在**集群列表**页面，在需要添加配置的Kafka集群后单击**详情**。
2.  在左侧导航栏中单击**集群与服务管理**页签，然后单击服务列表中的Kafka右侧的操作。
3.  在下拉菜单中选择**RESTART All Components**，输入记录信息后单击**确定**。

## 授权\(ACL\) {#section_v3w_v4c_bfb .section}

-   基本概念

    Kafka官方文档定义:

    ```
    Kafka acls are defined in the general format of "Principal P is [Allowed/Denied] Operation O From Host H On Resource R"
    ```

    即ACL过程涉及Principal、Allowed/Denied、Operation、Host和Resource。

    -   Principal：用户名

        |安全协议|value|
        |----|-----|
        |PLAINTEXT|ANONYMOUS|
        |SSL|ANONYMOUS|
        |SASL\_PLAINTEXT|mechanism为PLAIN时，用户名是client\_jaas.conf指定的用户名，mechanism为GSSAPI时，用户名为client\_jaas.conf指定的principal|
        |SASL\_SSL| |

    -   Allowed/Denied：允许/拒绝。
    -   Operation： 操作，包括Read、Write、Create、DeleteAlter、Describe、ClusterAction、AlterConfigs、DescribeConfigs、IdempotentWrite和All。
    -   Host： 针对的机器。
    -   Resource： 权限作用的资源对象，包括Topic、Group、Cluster和TransactionalId。
    Operation/Resource的一些详细对应关系，如哪些Resource支持哪些Operation的授权，详见[KIP-11 - Authorization Interface](https://cwiki.apache.org/confluence/display/KAFKA/KIP-11+-+Authorization+Interface)。

-   授权命令

    我们使用脚本kafka-acls.sh \(/usr/lib/kafka-current/bin/kafka-acls.sh\) 进行Kafka授权，您可以直接执行`kafka-acls.sh --help`查看如何使用该命令。


## 操作示例 {#section_a4t_vpc_bfb .section}

在已经创建的E-MapReduce高安全Kafka集群的master节点上进行相关示例操作。

1.  新建用户test，执行以下命令。

    `useradd test`

2.  创建topic。

    第一节添加配置的备注中提到zookeeper.set.acl=true，kafka-topics.sh需要在kafka账号下执行，而且kafka账号下要通过Kerberos认证。

    ```
    # kafka_client_jaas.conf中已经设置了kafka的Kerberos认证相关信息
    export KAFKA_HEAP_OPTS="-Djava.security.auth.login.config=/etc/ecm/kafka-conf/kafka_client_jaas.conf" 
    # zookeeper地址改成自己集群的对应地址(执行`hostnamed`后即可获取)
    kafka-topics.sh --create --zookeeper emr-header-1:2181/kafka-1.0.0 --replication-factor 3 --partitions 1 --topic test
    ```

3.  test用户执行kafka-console-producer.sh。
    1.  创建test用户的keytab文件，用于zookeeper/kafka的认证。

        ```
        su root
        sh /usr/lib/has-current/bin/hadmin-local.sh /etc/ecm/has-conf -k /etc/ecm/has-conf/admin.keytab
        HadminLocalTool.local: #直接按回车可以看到一些命令的用法
        HadminLocalTool.local: addprinc #输入命令按回车可以看到具体命令的用法
        HadminLocalTool.local: addprinc -pw 123456 test #添加test的princippal,密码设置为123456
        HadminLocalTool.local: ktadd -k /home/test/test.keytab test #导出keytab文件，后续可使用该文件
        ```

    2.  添加kafka\_client\_test.conf。

        如文件放到/home/test/kafka\_client\_test.conf，内容如下:

        ```
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

    3.  添加producer.conf。

        如文件放到`/home/test/producer.conf`，内容如下:

        ```
        security.protocol=SASL_PLAINTEXT
        sasl.mechanism=GSSAPI
        ```

    4.  执行kafka-console-producer.sh。

        ```
        su test
        export KAFKA_HEAP_OPTS="-Djava.security.auth.login.config=/home/test/kafka_client_test.conf"
        kafka-console-producer.sh --producer.config /home/test/producer.conf --topic test --broker-list emr-worker-1:9092
        ```

        由于没有设置ACL，所以上述会报错:

        ```
        org.apache.kafka.common.errors.TopicAuthorizationException: Not authorized to access topics: [test]
        ```

    5.  设置ACL。

        同样`kafka-acls.sh`也需要kafka账号执行。

        ```
        su kafka
        export KAFKA_HEAP_OPTS="-Djava.security.auth.login.config=/etc/ecm/kafka-conf/kafka_client_jaas.conf" 
        kafka-acls.sh --authorizer-properties zookeeper.connect=emr-header-1:2181/kafka-1.0.0 --add --allow-principal User:test --operation Write --topic test
        ```

    6.  再执行kafka-console-producer.sh。

        ```
        su test
        export KAFKA_HEAP_OPTS="-Djava.security.auth.login.config=/home/test/kafka_client_test.conf"
        kafka-console-producer.sh --producer.config /home/test/producer.conf --topic test --broker-list emr-worker-1:9092
        ```

        正常情况下显示如下:

        ```
        [2018-02-28 22:25:36,178] INFO Kafka commitId : aaa7af6d4a11b29d (org.apache.kafka.common.utils.AppInfoParser)
        >alibaba
        >E-MapReduce
        >
        ```

4.  test用户执行`kafka-console-consumer.sh`。

    上面成功执行kafka-console-producer.sh,并往topic里面写入一些数据后，就可以执行`kafka-console-consumer.sh`进行消费测试.

    1.  添加consumer.conf。

        如文件放到/home/test/consumer.conf，内容如下:

        ```
        security.protocol=SASL_PLAINTEXT
        sasl.mechanism=GSSAPI
        ```

    2.  执行kafka-console-consumer.sh。

        ```
        su test
        #kafka_client_test.conf跟上面producer使用的是一样的
        export KAFKA_HEAP_OPTS="-Djava.security.auth.login.config=/home/test/kafka_client_test.conf"
        kafka-console-consumer.sh --consumer.config consumer.conf  --topic test --bootstrap-server emr-worker-1:9092 --group test-group  --from-beginning
        ```

        由于未设置权限，会报错:

        ```
        org.apache.kafka.common.errors.GroupAuthorizationException: Not authorized to access group: test-group
        ```

    3.  设置ACL。

        ```
        su kafka
        export KAFKA_HEAP_OPTS="-Djava.security.auth.login.config=/etc/ecm/kafka-conf/kafka_client_jaas.conf" 
        # test-group权限
        kafka-acls.sh --authorizer-properties zookeeper.connect=emr-header-1:2181/kafka-1.0.0 --add --allow-principal User:test --operation Read --group test-group
        # topic权限
        kafka-acls.sh --authorizer-properties zookeeper.connect=emr-header-1:2181/kafka-1.0.0 --add --allow-principal User:test --operation Read --topic test
        ```

    4.  再执行`kafka-console-consumer.sh`。

        ```
        su test
        # kafka_client_test.conf跟上面producer使用的是一样的
        export KAFKA_HEAP_OPTS="-Djava.security.auth.login.config=/home/test/kafka_client_test.conf"
        kafka-console-consumer.sh --consumer.config consumer.conf  --topic test --bootstrap-server emr-worker-1:9092 --group test-group  --from-beginning
        ```

        正常输出:

        ```
        alibaba
        E-MapReduce
        ```


