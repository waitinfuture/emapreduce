# Flume使用说明 {#concept_fxh_42r_zfb .concept}

E-MapReduce（以下简称 EMR） 从 3.16.0 版本开始支持 Apache Flume。本文介绍使用 Flume 将数据从 EMR Kafka 集群同步至 EMR Hadoop 集群的 HDFS、Hive和 HBase，以及阿里云的 OSS。

## 准备工作 {#section_wbr_dfr_zfb .section}

-   创建 Hadoop 集群时，在**可选服务**中选择 Flume。
-   创建 Kafka 集群，并创建名称为 flume-test 的 topic，用于生成数据。

**说明：** 

-   如果创建的是 Hadoop 高安全集群，消费标准 Kafka 集群的数据，参照[兼容 MIT Kerberos 认证](intl.zh-CN/用户指南/Kerberos认证/兼容MIT Kerberos认证.md#)在 Hadoop 集群配置 Kerberos 认证；
-   如果创建的是 Kafka 高安全集群，通过 Flume 将数据写入标准 Hadoop 集群，参见 [Kerberos Kafka Source](#section_l1j_3fs_zfb) 部分；
-   如果创建的 Hadoop 集群和 Kafka 集群都是高安全集群，参照[跨域互信](intl.zh-CN/用户指南/Kerberos认证/跨域互信.md#)进行配置，参见[跨域互信使用 Flume](#section_oft_fjs_zfb)；

## Kafka-\>HDFS {#section_fj5_tgr_zfb .section}

-   配置 Flume

    创建配置文件flume.properties，添加如下配置。其中，配置项a1.sources.source1.kafka.bootstrap.servers为 Kafka 集群 broker 的 host 和端口号，a1.sources.source1.kafka.topics为 Flume 消费 Kafka 数据的 topic，a1.sinks.k1.hdfs.path为 Flume 向 HDFS 写入数据的路径：

    ```
    a1.sources = source1
    a1.sinks = k1
    a1.channels = c1
    
    a1.sources.source1.type = org.apache.flume.source.kafka.KafkaSource
    a1.sources.source1.channels = c1
    a1.sources.source1.kafka.bootstrap.servers = kafka-host1:port1,kafka-host2:port2...
    a1.sources.source1.kafka.topics = flume-test
    a1.sources.source1.kafka.consumer.group.id = flume-test-group
    
    # Describe the sink
    a1.sinks.k1.type = hdfs
    a1.sinks.k1.hdfs.path = /tmp/flume/test-data
    a1.sinks.k1.hdfs.fileType=DataStream
    
    # Use a channel which buffers events in memory
    a1.channels.c1.type = memory
    a1.channels.c1.capacity = 100
    a1.channels.c1.transactionCapacity = 100
    
    # Bind the source and sink to the channel
    a1.sources.source1.channels = c1
    a1.sinks.k1.channel = c1
    ```

    **说明：** 配置项 a1.sinks.k1.hdfs.path 如果使用 URL 的形式，对于高可用集群，使用 hdfs://emr-cluster，例如：

    ```
    a1.sinks.k1.hdfs.path = hdfs://emr-cluster/tmp/flume/test-data
    ```

    对于标准集群，使用hdfs://emr-header-1:9000，例如：

    ```
    a1.sinks.k1.hdfs.path = hdfs://emr-header-1:9000/tmp/flume/test-data
    ```

-   启动服务

    Flume 的默认配置文件放在/etc/ecm/flume-conf下，使用该配置启动 Flume Agent：

    ```
    flume-ng agent --name a1 --conf /etc/ecm/flume-conf  --conf-file flume.properties
    ```

    启动 Agent 后，因为使用了/etc/ecm/flume-conf下的log4j.properties，会在当前路径下生成日志logs/flume.log，可根据实际使用对log4j.properties进行配置。

-   测试

    在 Kafka 集群使用kafka-console-producer.sh输入测试数据 abc

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/67072/155116757633579_zh-CN.png)

    Flume 会在 HDFS 中以当前时间的\(毫秒\)时间戳生成文件 FlumeData.xxxx，查看文件内容，会看到在 Kafka 中输入的数据

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/67072/155116757733580_zh-CN.png)


## Kafka-\>Hive {#section_xmd_bjr_zfb .section}

-   创建 Hive 表

    Flume 使用事务操作将数据写入 Hive，需要在创建 Hive 表时设置transactional属性，如创建 flume\_test 表：

    ```
    create table flume_test (id int, content string)
    clustered by (id) into 2 buckets
    stored as orc  TBLPROPERTIES('transactional'='true');
    ```

-   配置 Flume

    创建配置文件flume.properties，添加如下配置。其中，配置项a1.sources.source1.kafka.bootstrap.servers填写 Kafka 集群 broker 的 host 和端口号，a1.sinks.k1.hive.metastore为 Hive metastore URI，配置为hive-site.xml中配置项hive.metastore.uris的值:

    ```
    a1.sources = source1
    a1.sinks = k1
    a1.channels = c1
    
    a1.sources.source1.type = org.apache.flume.source.kafka.KafkaSource
    a1.sources.source1.channels = c1
    a1.sources.source1.kafka.bootstrap.servers = kafka-host1:port1,kafka-host2:port2...
    a1.sources.source1.kafka.topics = flume-test
    a1.sources.source1.kafka.consumer.group.id = flume-test-group
    
    # Describe the sink
    a1.sinks.k1.type = hive
    a1.sinks.k1.hive.metastore = thrift://xxxx:9083
    a1.sinks.k1.hive.database = default
    a1.sinks.k1.hive.table = flume_test
    a1.sinks.k1.serializer = DELIMITED
    a1.sinks.k1.serializer.delimiter = ","
    a1.sinks.k1.serializer.serdeSeparator = ','
    a1.sinks.k1.serializer.fieldnames =id,content
    
    a1.channels.c1.type = memory
    a1.channels.c1.capacity = 100
    a1.channels.c1.transactionCapacity = 100
    
    a1.sources.source1.channels = c1
    a1.sinks.k1.channel = c1
    ```

-   启动 Flume

    ```
    flume-ng agent --name a1 --conf /etc/ecm/flume-conf  --conf-file flume.properties
    ```

-   生成数据

    在 Kafka 集群中使用kafka-console-producer.sh，以逗号为分隔符输入测试数据 1,a

-   检测数据写入

    查询 Hive 事务表需要在客户端进行配置：

    ```
    hive.support.concurrency – true
    hive.exec.dynamic.partition.mode – nonstrict
    hive.txn.manager – org.apache.hadoop.hive.ql.lockmgr.DbTxnManager
    ```

    配置好后查询 flume\_test 表中的数据![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/67072/155116757733581_en-US.png)


## Kafka-\>HBase {#section_alr_zjr_zfb .section}

-   创建 HBase 表

    创建 HBase 表 flume\_test 及列簇 column

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/67072/155116757733582_zh-CN.png)

-   配置 Flume

    创建配置文件flume.properties，添加如下配置。其中，配置项a1.sources.source1.kafka.bootstrap.servers为 Kafka 集群 broker 的 host 和端口号，a1.sinks.k1.table为 HBase 表名，a1.sinks.k1.columnFamily为列簇名：

    ```
    a1.sources = source1
    a1.sinks = k1
    a1.channels = c1
    
    a1.sources.source1.type = org.apache.flume.source.kafka.KafkaSource
    a1.sources.source1.channels = c1
    a1.sources.source1.kafka.bootstrap.servers = kafka-host1:port1,kafka-host2:port2...
    a1.sources.source1.kafka.topics = flume-test
    a1.sources.source1.kafka.consumer.group.id = flume-test-group
    
    a1.sinks.k1.type = hbase
    a1.sinks.k1.table = flume_test
    a1.sinks.k1.columnFamily = column
    
    
    # Use a channel which buffers events in memory
    a1.channels.c1.type = memory
    a1.channels.c1.capacity = 1000
    a1.channels.c1.transactionCapacity = 100
    
    # Bind the source and sink to the channel
    a1.sources.source1.channels = c1
    a1.sinks.k1.channel = c1
    ```

-   启动服务

    ```
    flume-ng agent --name a1 --conf /etc/ecm/flume-conf  --conf-file flume.properties
    ```

-   测试

    在 Kafka 集群使用kafka-console-producer.sh生成数据后，就可以在 HBase 查到数据

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/67072/155116757733583_zh-CN.png)


## Kafka-\>OSS {#section_cys_5kr_zfb .section}

-   创建 OSS 路径

    创建 OSS Bucket 及目录，如oss://flume-test/result

-   配置 Flume

    Flume 向 OSS 写入数据时，需要占用较大的 JVM 内存，可以改小 OSS 缓存或者增大 Flume Agent 的 Xmx：

    -   修改 OSS 缓存大小

        将hdfs-site.xml配置文件从/etc/ecm/hadoop-conf拷贝至/etc/ecm/flume-conf，改小配置项smartdata.cache.buffer.size的值，如修改为 1048576。

    -   修改 Xmx

        在 Flume 的配置路径/etc/ecm/flume-conf下，复制配置文件flume-env.sh.template并重命名为flume-env.sh，设置 Xmx，如设置为 1 G ：

        ```
        export JAVA_OPTS="-Xmx1g"
        ```

    创建配置文件flume.properties，添加如下配置。其中，配置项a1.sources.source1.kafka.bootstrap.servers填写 Kafka 集群 broker 的 host 和端口号，a1.sinks.k1.hdfs.path为 OSS 路径：

    ```
    a1.sources = source1
    a1.sinks = k1
    a1.channels = c1
    
    a1.sources.source1.type = org.apache.flume.source.kafka.KafkaSource
    a1.sources.source1.channels = c1
    a1.sources.source1.kafka.bootstrap.servers = kafka-host1:port1,kafka-host2:port2...
    a1.sources.source1.kafka.topics = flume-test
    a1.sources.source1.kafka.consumer.group.id = flume-test-group
    
    a1.sinks.k1.type = hdfs
    a1.sinks.k1.hdfs.path = oss://flume-test/result
    a1.sinks.k1.hdfs.fileType=DataStream
    
    # Use a channel which buffers events in memory
    a1.channels.c1.type = memory
    a1.channels.c1.capacity = 100
    a1.channels.c1.transactionCapacity = 100
    
    # Bind the source and sink to the channel
    a1.sources.source1.channels = c1
    a1.sinks.k1.channel = c1
    ```

-   启动 Flume

    如果配置 Flume 时修改了 OSS 缓存大小，使用 classpath 参数传入 OSS 相关依赖和配置：

    ```
    flume-ng agent --name a1 --conf /etc/ecm/flume-conf  --conf-file flume.properties  --classpath "/opt/apps/extra-jars/*:/etc/ecm/flume-conf/hdfs-site.xml"
    ```

    如果修改了 Flume Agent 的 Xmx，只需传入 OSS 相关依赖：

    ```
    flume-ng agent --name a1 --conf /etc/ecm/flume-conf  --conf-file flume.properties  --classpath "/opt/apps/extra-jars/*"
    ```

-   测试

    在 Kafka 集群使用kafka-console-producer.sh生成数据后，在 OSS 的oss://flume-test/result路径下会以当前时间的\(毫秒\)时间戳为后缀生成文件FlumeData.xxxx


## Kerberos Kafka source {#section_l1j_3fs_zfb .section}

消费高安全 Kafka 集群的数据时，需要做额外的配置：

-   参照[兼容 MIT Kerberos 认证](intl.zh-CN/用户指南/Kerberos认证/兼容MIT Kerberos认证.md#)在 Kafka 集群配置 Kerberos 认证，将生成的 keytab 文件test.keytab拷贝至 Hadoop 集群的/etc/ecm/flume-conf路径下；将 Kafka集群的/etc/ecm/has-conf/krb5.conf文件拷贝至 Hadoop 集群的/etc/ecm/flume-conf路径下。
-   配置flume.properties

    在flume.properties中添加如下配置：

    ```
    a1.sources.source1.kafka.consumer.security.protocol = SASL_PLAINTEXT
    a1.sources.source1.kafka.consumer.sasl.mechanism = GSSAPI
    a1.sources.source1.kafka.consumer.sasl.kerberos.service.name = kafka
    ```

-   配置 Kafka client
    -   在/etc/ecm/flume-conf下创建文件flume\\\_jaas.conf，内容如下：

        ```
        KafkaClient {
          com.sun.security.auth.module.Krb5LoginModule required
          useKeyTab=true
          storeKey=true
          keyTab="/etc/ecm/flume-conf/test.keytab"
          serviceName="kafka"
          principal="test@EMR.${realm}.COM";
        };
        ```

        其中，$\{realm\}替换为 Kafka 集群的 Kerberos realm。获取方式为，在 Kafka 集群执行命令hostname，得到形式为emr-header-1.cluster-xxx的主机名，如emr-header-1.cluster-123456，最后的数字串 123456 即为 realm

    -   修改/etc/ecm/flume-conf/flume-env.sh

        初始情况下，/etc/ecm/flume-conf/下没有flume-env.sh文件，需要拷贝flume-env.sh.template并重命名为flume-env.sh。添加如下内容：

        ```
        export JAVA_OPTS="$JAVA_OPTS -Djava.security.krb5.conf=/etc/ecm/flume-conf/krb5.conf"
        export JAVA_OPTS="$JAVA_OPTS -Djava.security.auth.login.config=/etc/ecm/flume-conf/flume_jaas.conf"
        ```

-   设置域名

    将 Kafka 集群各节点的长域名和 IP 的绑定信息添加到 Hadoop 集群的/etc/hosts。长域名的形式例如emr-header-1.cluster-123456

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/67072/155116757733590_zh-CN.png)


## 跨域互信使用 Flume {#section_oft_fjs_zfb .section}

在配置了跨域互信后，其他配置如下：

-   参照[兼容 MIT Kerberos 认证](intl.zh-CN/用户指南/Kerberos认证/兼容MIT Kerberos认证.md#)在 Kafka 集群配置 Kerberos 认证，将生成的 keytab 文件test.keytab拷贝至 Hadoop 集群的/etc/ecm/flume-conf路径下。
-   配置flume.properties

    在flume.properties中添加如下配置：

    ```
    a1.sources.source1.kafka.consumer.security.protocol = SASL_PLAINTEXT
    a1.sources.source1.kafka.consumer.sasl.mechanism = GSSAPI
    a1.sources.source1.kafka.consumer.sasl.kerberos.service.name = kafka
    ```

-   配置 Kafka client
    -   在/etc/ecm/flume-conf下创建文件flume\\\_jaas.conf，内容如下：

```
KafkaClient {
  com.sun.security.auth.module.Krb5LoginModule required
  useKeyTab=true
  storeKey=true
  keyTab="/etc/ecm/flume-conf/test.keytab"
  serviceName="kafka"
  principal="test@EMR.${realm}.COM";
};
```

        其中，$\{realm\}替换为 Kafka 集群的 Kerberos realm。获取方式为，在 Kafka 集群执行命令hostname，得到形式为emr-header-1.cluster-xxx的主机名，如emr-header-1.cluster-123456，最后的数字串 123456 即为 realm。

    -   修改/etc/ecm/flume-conf/flume-env.sh

        初始情况下，/etc/ecm/flume-conf/下没有flume-env.sh文件，需要拷贝flume-env.sh.template并重命名为flume-env.sh。添加如下内容：

        ```
        export JAVA_OPTS="$JAVA_OPTS -Djava.security.auth.login.config=/etc/ecm/flume-conf/flume_jaas.conf""
        ```


