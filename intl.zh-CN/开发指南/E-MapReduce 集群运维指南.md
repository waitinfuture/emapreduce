# E-MapReduce 集群运维指南 {#concept_f2p_134_hfb .concept}

本篇主要旨在说明 E-MapReduce 集群的一些运维的手段，方便大家在使用的时候可以自主的进行服务的运维。

**说明：** 在较新的集群版本中（3.2 以上版本），所有的服务操作都可以通过集群的配置管理功能来完成。推荐优先使用 Web 页面的管理方式。

## 一些通用的环境变量 {#section_lg5_hj4_hfb .section}

在集群上，输入 env 命令可以看到类似如下的环境变量配置（以集群上最新的配置为准，此处只做参考）：

```
JAVA_HOME=/usr/lib/jvm/java
HADOOP_HOME=/usr/lib/hadoop-current
HADOOP_CLASSPATH=/usr/lib/hbase-current/lib/*:/usr/lib/tez-current/*:/usr/lib/tez-current/lib/*:/etc/emr/tez-conf:/usr/lib/hbase-current/lib/*:/usr/lib/tez-current/*:/usr/lib/tez-current/lib/*:/etc/emr/tez-conf:/opt/apps/extra-jars/*:/opt/apps/extra-jars/*
HADOOP_CONF_DIR=/etc/emr/hadoop-conf
SPARK_HOME=/usr/lib/spark-current
SPARK_CONF_DIR=/etc/emr/spark-conf
HBASE_HOME=/usr/lib/hbase-current
HBASE_CONF_DIR=/etc/emr/hbase-conf
HIVE_HOME=/usr/lib/hive-current
HIVE_CONF_DIR=/etc/emr/hive-conf
PIG_HOME=/usr/lib/pig-current
PIG_CONF_DIR=/etc/emr/pig-conf
TEZ_HOME=/usr/lib/tez-current
TEZ_CONF_DIR=/etc/emr/tez-conf
ZEPPELIN_HOME=/usr/lib/zeppelin-current
ZEPPELIN_CONF_DIR=/etc/emr/zeppelin-conf
HUE_HOME=/usr/lib/hue-current
HUE_CONF_DIR=/etc/emr/hue-conf
PRESTO_HOME=/usr/lib/presto-current
PRESTO_CONF_DIR=/etc/emr/presot-conf
```

## 启停服务进程 {#section_jwd_zj4_hfb .section}

您可以在 E-MapReduce 控制台，对指定 ECS 实例上运行的服务进程进行启动、停止和重启操作。各个服务进程的操作类似，下面以 HDFS 为例，介绍如何启动、停止和重启 emr-worker-1 实例上的 DataNode 进程，操作步骤如下：

1.  在集群管理页面，在要进行操作的集群后的**操作**列单击**管理**。
2.  在集群与服务管理页面，单击服务列表中的 **HDFS** 链接，进入 HDFS 服务页面。
3.  单击部署拓扑页签，可以看到该集群中所有实例上运行的服务进程列表。
4.  单击 emr-worker-1 主机上运行的 DataNode 进程的**操作**列中的**启动**，在提示框中输入记录信息后，单击**确定**。 等待约 10 秒后，刷新页面，可以**看到组件**状态列从 **STOPPED** 变为 **STARTED**。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17993/155868238713238_zh-CN.png)

5.  该进程被启动后，单击**操作**列的**重启**，在提示框中输入记录信息后，单击**确定**。

    启动过程中刷新页面，等待约 40 秒后，可以看到组件状态从列 **STOPPED** 变为 **STARTED**。

6.  单击**操作**列的**停止**，在提示框中输入记录信息后，单击**确定**。

    等待约 10 秒后，刷新页面，可以看到**组件状态**列从**STARTED**变为 **STOPPED**。


## 批量操作服务进程 {#section_ggn_m44_hfb .section}

除了可以操作指定 ECS 实例上运行的服务进程外，您还可以进行批量操作。下面以 HDFS 为例，介绍如何启重启所有实例上的 DataNode 进程，操作步骤如下：

1.  在集群管理页面，在要进行操作的集群后的**操作**列单击**配管理**。
2.  在集群与服务管理页面，单击服务列表中 **HDFS** 后对应的**操作**。
3.  在菜单中选择 **RESTART DataNode**，在提示框中输入记录信息后，单击**确定**。

    您可以单击 **HDFS**，进入部署拓扑页面查看各个进程的启动状态。

    **说明：** 执行完滚动重启后，不能再执行非滚动重启，否则系统会报错。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17993/155868238713240_zh-CN.png)


## 通过命令行方式启停服务进程 {#section_aps_pr4_hfb .section}

-   YARN

    操作用账号：hadoop

    -   ResourceManager （Master 节点）

        ```
        // 启动
        /usr/lib/hadoop-current/sbin/yarn-daemon.sh start resourcemanager
        // 停止
        /usr/lib/hadoop-current/sbin/yarn-daemon.sh stop resourcemanager
        ```

    -   NodeManager （Core 节点）

        ```
        // 启动
        /usr/lib/hadoop-current/sbin/yarn-daemon.sh start nodemanager
        // 停止
        /usr/lib/hadoop-current/sbin/yarn-daemon.sh stop nodemanager
        ```

    -   JobHistoryServer （Master 节点）

        ```
        // 启动
        /usr/lib/hadoop-current/sbin/mr-jobhistory-daemon.sh start historyserver
        // 停止
        /usr/lib/hadoop-current/sbin/mr-jobhistory-daemon.sh stop historyserver
        ```

    -   JobHistoryServer （Master节点）

        ```
        // 启动
        /usr/lib/hadoop-current/sbin/mr-jobhistory-daemon.sh start historyserver
        // 停止
        /usr/lib/hadoop-current/sbin/mr-jobhistory-daemon.sh stop historyserver
        ```

    -   WebProxyServer （Master 节点）

        ```
        // 启动
        /usr/lib/hadoop-current/sbin/yarn-daemon.sh start proxyserver
        // 停止
        /usr/lib/hadoop-current/sbin/yarn-daemon.sh stop proxyserver
        ```

-   HDFS

    操作用账号：hdfs

    -   NameNode （Master 节点）

        ```
        // 启动
        /usr/lib/hadoop-current/sbin/hadoop-daemon.sh start namenode
        // 停止
        /usr/lib/hadoop-current/sbin/hadoop-daemon.sh stop namenode
        ```

    -   DataNode （Core 节点）

        ```
        // 启动
        /usr/lib/hadoop-current/sbin/hadoop-daemon.sh start datanode
        // 停止
        /usr/lib/hadoop-current/sbin/hadoop-daemon.sh stop datanode
        ```

-   Hive

    操作用账号：hadoop

    -   MetaStore （Master 节点）

        ```
        // 启动，这里的内存可以根据需要扩大
        HADOOP_HEAPSIZE=512 /usr/lib/hive-current/bin/hive --service metastore >/var/log/hive/metastore.log 2>&1 &
        ```

    -   HiveServer2 （Master 节点）

        ```
        // 启动
        HADOOP_HEAPSIZE=512 /usr/lib/hive-current/bin/hive --service hiveserver2 >/var/log/hive/hiveserver2.log 2>&1 &
        ```

-   HBase

    操作用账号：hdfs

    需要注意的，需要在组件中选择了 HBase 以后才能使用如下的方式来启动，否则对应的配置未完成，启动的时候会有错误。

    -   HMaster （Master 节点）

        ```
        // 启动
        /usr/lib/hbase-current/bin/hbase-daemon.sh start master
        // 重启
        /usr/lib/hbase-current/bin/hbase-daemon.sh restart master
        // 停止
        /usr/lib/hbase-current/bin/hbase-daemon.sh stop master
        ```

    -   HRegionServer （Core 节点）

        ```
        // 启动
        /usr/lib/hbase-current/bin/hbase-daemon.sh start regionserver
        // 重启
        /usr/lib/hbase-current/bin/hbase-daemon.sh restart regionserver
        // 停止
        /usr/lib/hbase-current/bin/hbase-daemon.sh stop regionserver
        ```

    -   ThriftServer （Master 节点）

        ```
        // 启动
        /usr/lib/hbase-current/bin/hbase-daemon.sh start thrift -p 9099 >/var/log/hive/thriftserver.log 2>&1 &
        // 停止
        /usr/lib/hbase-current/bin/hbase-daemon.sh stop thrift
        ```

-   Hue

    操作用账号：hadoop

    ```
    // 启动
    su -l root -c "${HUE_HOME}/build/env/bin/supervisor >/dev/null 2>&1 &"
    // 停止
    ps aux | grep hue     // 查找到所有的hue进程
    kill -9 huepid        // 杀掉查找到的对应hue进程
    ```

-   Zeppelin

    操作用账号：hadoop

    ```
    // 启动， 这里的内存可以根据需要扩大
    su -l root -c "ZEPPELIN_MEM=\"-Xmx512m -Xms512m\" ${ZEPPELIN_HOME}/bin/zeppelin-daemon.sh start"
    // 停止
    su -l root -c "${ZEPPELIN_HOME}/bin/zeppelin-daemon.sh stop"
    ```

-   Presto

    操作用账号：hdfs

    -   PrestoServer （Master 节点）

        ```
        // 启动
        /usr/lib/presto-current/bin/launcher --config=/usr/lib/presto-current/etc/coordinator-config.properties start
        // 停止
        /usr/lib/presto-current/bin/launcher --config=/usr/lib/presto-current/etc/coordinator-config.properties stop
        ```

    -   （Core 节点）

        ```
        // 启动
        /usr/lib/presto-current/bin/launcher --config=/usr/lib/presto-current/etc/worker-config.properties start
        // 停止
        /usr/lib/presto-current/bin/launcher --config=/usr/lib/presto-current/etc/worker-config.properties stop
        ```


## 通过命令行方式批量操作 {#section_nvb_zs4_hfb .section}

当需要对 worker\(Core\) 节点做统一操作时，可以写脚本命令，一键轻松解决。在 EMR 集群中，Master 和所有worker 节点在 hadoop 和 hdfs 账号下是 SSH 互通的。

例如，需要对所有 worker 节点的 NodeManager 做停止操作，假设有 10 个 worker 节点，则可以通过以下命令操作：

```
for i in `seq 1 10`;do ssh emr-worker-$i /usr/lib/hadoop-current/sbin/yarn-daemon.sh stop nodemanager;done
```

