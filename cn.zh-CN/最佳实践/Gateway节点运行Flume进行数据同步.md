# Gateway节点运行Flume进行数据同步 {#concept_fwf_jtm_4gb .concept}

本文以阿里云 E-MapReduce 3.17.0 及以后的版本特性介绍使用Gateway节点运行 Flume 从而进行数据同步操作。

## 背景信息 {#section_o5c_ntm_4gb .section}

E-MapReduce从 3.16.0 版本开始支持Apache Flume，从 3.17.0 版本开始提供默认监控等特性。

-   基本数据流

    在 Gateway 节点运行 Flume 可以避免对 EMR Hadoop 集群产生影响。使用 Gateway节点部署 Flume Agent 的基本数据流如下图所示：

    ![基本数据流](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/120368/154840867038178_zh-CN.png)


## 环境准备 {#section_mjt_c5n_4gb .section}

本文选择在杭州 Region 进行测试，版本选择 EMR- 3.17.0，本次测试需要的组件版本有：

-   Flume：1.8.0

本文使用阿里云 EMR 服务自动化搭建 Hadoop 集群，详细过程请参考[创建集群](../../../../../intl.zh-CN/快速入门/创建 E-MapReduce/创建集群.md#)。

-   创建 Hadoop 集群，在**可选服务**中选择 Flume。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/120368/154840867038191_zh-CN.png)

-   创建Gateway节点，关联上一步已经创建好的Hadoop集群。

## 实施步骤 {#section_sbj_bzn_4gb .section}

-   运行Flume

    -   Flume的默认配置路径为/etc/ecm/flume-conf。参考[Flume使用说明](../../../../../intl.zh-CN/用户指南/开源组件介绍/Flume使用说明.md#)创建agent配置文件flume.properties后，可以使用以下命令运行一个Flume Agent：

        ```
        nohup flume-ng agent -n a1 -f flume.properties &
        ```

    -   如果想使用自定义的配置目录，可以使用 `-c`或者`--conf` 覆盖默认的配置，例如：

        ```
        nohup flume-ng agent -n a1 -f flume.properties -c path-to-flume-conf &
        ```

    **说明：** 参考[Flume使用说明](../../../../../intl.zh-CN/用户指南/开源组件介绍/Flume使用说明.md#)在 Gateway 节点配置 Flume 时，如果 Sink 到 HBase，需要在配置文件 flume.properties 添加配置项 zookeeperQuorum，例如：

    ```
    a1.sinks.k1.zookeeperQuorum=emr-header-1.cluster-46349:2181
    ```

    其中，Zookeeper的主机地址`emr-header-1.cluster-46349`可以在/etc/ecm/hbase-conf/hbase-site.xml的hbase.zookeeper.quorum 配置项中找到。

-   查看监控信息

    默认情况下，集群的 Console 页面提供了 Flume Agent 的监控信息。通过在集群与服务管理页面单击 **Flume**服务进行访问，如下图所示：

    ![FLUME服务页面](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/120368/154840867138198_zh-CN.png)

    **说明：** 

    监控数据以 agent 组件\(source、channel 或 sink\)的名称命名，例如CHANNEL.channel1 表示名称为 channel1 的 channel 组件的监控指标，所以在配置不同的 agent 时请避免使用相同的组件名称。

    如果想通过 Ganglia 等方式查看 Flume Agent 的监控数据，可参考 Flume 官网进行配置。此时 Console 页面将不会显示 Flume Agent 的监控数据。

-   查看日志

    默认情况下，Flume Agent 日志的存放路径为/mnt/disk1/log/flume/$\{flume-agent-name\}/flume.log。可以通过修改/etc/ecm/flume-conf/log4j.properties进行配置\(不建议修改日志路径\)。

    **说明：** 日志路径包含了 Flume Agent 的名称，所以配置不同的 agent 时请勿使用相同的 agent 名称，以免日志混在一起。


