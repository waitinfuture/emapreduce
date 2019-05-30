# 使用 E-MapReduce 处理离线作业 {#concept_bzz_zg1_dfb .concept}

本文介绍使用 E-MapReduce（以下简称 EMR） 产品从 OSS 服务上读取数据 ，并进行数据采集、分析等一系列离线数据处理的使用场景。

## 背景信息 {#section_qlt_kh1_dfb .section}

EMR 集群适用场景很多。简单说来，Hadoop ecosystem 以及 Spark 能够支持的场景，EMR 都可以支持。因为 EMR 本质就是 Hadoop 和 Spark 的集群服务，您完全可以将 EMR 集群使用的阿里云 ECS 主机视为自己专属的物理主机。

大数据处理目前比较流行的有两种方法，一种是离线处理，一种是在线处理。

-   离线处理：只是希望得到数据的分析结果，对处理的时间要求不严格，例如批量数据处理，用户将数据传输到 OSS 服务，OSS 服务作为 EMR 产品的输入输出，利用 MapReduce，Hive，Pig，Spark 处理离线数据。
-   在线处理：对于数据的分析结果在时间上有比较严格的要求，例如实时流式数据处理，使用 Spark Streaming 对消息数据进行处理，与 Spark mllib，GrapX，SQL 深度整合。

下面将使用 EMR 产品运行一个 "word count" 离线作业。

## 基本架构 {#section_km4_qh1_dfb .section}

OSS -\> EMR -\> Hadoop MapReduce

上述链路主要包含 2 个过程：

1.  把数据存储到 OSS 服务里。
2.  通过 EMR 服务将 OSS 中的数据读取出来，进行分析。

## 环境准备 {#section_krn_vh1_dfb .section}

-   本文以 Windows 环境为例，请确保 Git，Maven, Java 已经安装并配置成功。
-   本文使用阿里云 EMR 服务自动化搭建 Hadoop 集群，详细步骤请参见[创建集群](../../../../intl.zh-CN/快速入门/步骤三：创建集群.md#)。
    -   EMR 版本：EMR-3.12.1
    -   集群类型：Hadoop
    -   软件信息：HDFS 2.7.2/YARN 2.7.2/Hive 2.3.3/Ganglia 3.7.2/Spark 2.3.1/HUE 4.1.0/Zeppelin 0.8.0/Tez 0.9.1/Sqoop 1.4.7/Pig 0.14.0/ApacheDS 2.0.0/Knox 0.13.0
    -   Hadoop 集群使用专有网络，区域为华东 1（杭州），主实例组 ECS 计算资源配置公网及内网 IP，高可用选择为否（非 HA 模式），具体配置如下所示。

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/21330/155918565311874_zh-CN.png)


## 操作步骤 {#section_f2w_y31_dfb .section}

1.  下载示例代码到本地 。

    在本地打开 git bash 运行 clone 命令：

    ```
    git clone https://github.com/aliyun/aliyun-emapreduce-demo.git
    ```

    执行 `mvn install` 进行编译。

2.  创建 OSS 存储空间，详细步骤请参见[创建存储空间](../../../../intl.zh-CN/快速入门/创建存储空间.md#)。

    **说明：** Bucket 应与 E-MapReduce 集群在同一个区域。

3.  上传 jar 包和资源文件。
    1.  登录 [OSS 管理控制台](https://oss.console.aliyun.com)，单击**文件管理**。
    2.  单击**上传文件**，上传 aliyun-emapreduce-demo/resources 目录下的资源文件以及 aliyun-emapreduce-demo/target 目录下的 jar 文件。
4.  创建工作流项目。

    详细过程请参考[工作流项目管理](../../../../intl.zh-CN/数据开发/项目管理.md#)。

5.  创建作业。

    详细步骤请参见[作业编辑](../../../../intl.zh-CN/数据开发/作业编辑.md#)，这里我们以 MapReduce 为例，分别填写所属项目、作业名称、作业描述，并选择作业类型 MR。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/21330/155918565311891_zh-CN.jpg)

6.  配置作业内容，单击**运行**。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/21330/155918565311892_zh-CN.png)

    -   关于 OSS 使用，请参考 [OSS 参考使用说明](../../../../intl.zh-CN/开发指南/准备/OSS 参考使用说明.md#)
    -   关于各类作业的具体开发，请参见 EMR 用户指南 *作业* 部分。
    **说明：** 

    -   OSS 输出路径如果已经存在，在执行作业时会报错。
    -   插入 OSS 路径时，如果选择 OSSREF 文件前缀，系统会把 OSS 文件下载到集群本地，并添加到 classpath 中。
    -   当前所有操作都只支持标准存储类型的 OSS。

## 日志查看 {#section_drj_5k1_dfb .section}

作业运行成功后，您可以在页面下方的运行记录页签中查看作业的运行日志。单击**详情**跳转到运行记录中该作业的详细日志页面，可以查看作业的提交日志、YARN Container 日志。您也可以通过 SSH 方式登录到集群查看相关日志，详细步骤请参见 [SSH 登录集群](../../../../intl.zh-CN/集群规划与配置/集群配置/SSH 登录集群.md#)。

## 总结 {#section_bxc_lqj_gfb .section}

至此，我们成功在 E-MapReduce 上部署一套 Hadoop 集群，将 OSS 服务作为数据源输入，并运行 MapReduce 作业消费 OSS 上数据。E-MapReduce 支持多种类型作业的开发，例如 Storm 作业消费 Kafka 数据， Spark Streaming 和 Flink 组件，同样可以方便地在 Hadoop 集群上运行，处理 Kafka 数据。

