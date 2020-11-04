# 使用E-MapReduce处理离线作业

本文介绍使用E-MapReduce（以下简称EMR） 产品从OSS服务上读取数据 ，并进行数据采集、分析等一系列离线数据处理的使用场景。

## 背景信息

EMR集群适用于多种场景。EMR本质就是Hadoop和Spark的集群服务，所以Hadoop ecosystem以及Spark能够支持的场景，EMR都可以支持。您完全可以将EMR集群使用的阿里云ECS主机视为自己专属的物理主机。

大数据处理目前比较常见的有两种方法：

-   离线处理：只是希望得到数据的分析结果，对处理的时间要求不严格，例如批量数据处理，用户将数据传输到OSS服务，OSS服务作为EMR产品的输入输出，利用MapReduce、Hive、Pig、Spark处理离线数据。
-   在线处理：对于数据的分析结果在时间上有比较严格的要求，例如实时流式数据处理，使用Spark Streaming对消息数据进行处理，与Spark mllib、GrapX、SQL深度整合。

下面将使用EMR产品运行一个word count离线作业。

## 基本架构

OSS -\> EMR -\> Hadoop MapReduce

上述链路主要包含两个过程：

1.  把数据存储到OSS服务中。
2.  通过EMR服务将OSS中的数据读取出来，进行分析。

## 环境准备

-   本文以Windows环境为例，请确保Git、Maven、 Java已经安装并配置成功。
-   本文使用阿里云EMR服务自动化搭建Hadoop集群，详细步骤请参见[创建集群](/intl.zh-CN/快速入门/创建集群.md)。

    -   EMR版本：EMR-3.12.1
    -   集群类型：Hadoop
    -   软件信息：HDFS 2.7.2、YARN 2.7.2、Hive 2.3.3、Ganglia 3.7.2、Spark 2.3.1、HUE 4.1.0、Zeppelin 0.8.0、Tez 0.9.1、Sqoop 1.4.7、Pig 0.14.0、ApacheDS 2.0.0、Knox 0.13.0
    -   Hadoop集群使用专有网络，区域为华东 1（杭州），主实例组ECS计算资源配置公网及内网IP，**高可用**选择为**否**（非HA模式）。
    ![hadoop](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/2832598951/p11874.png)


## 操作步骤

1.  下载示例代码到本地 。

    在本地打开git bash运行clone 命令：

    ```
    git clone https://github.com/aliyun/aliyun-emapreduce-demo.git
    ```

    执行`mvn install`进行编译。

2.  创建OSS存储空间，详细步骤请参见[创建存储空间](/intl.zh-CN/快速入门/创建存储空间.md)。

    **说明：** Bucket应与E-MapReduce集群在同一个区域。

3.  上传JAR包和资源文件。
    1.  登录 [OSS管理控制台](https://oss.console.aliyun.com)，单击**文件管理**。
    2.  单击**上传文件**，上传aliyun-emapreduce-demo/resources目录下的资源文件以及aliyun-emapreduce-demo/target目录下的JAR文件。
4.  创建工作流项目。

    详细步骤请参见[工作流项目管理](/intl.zh-CN/数据开发/项目管理.md)。

5.  创建作业。

    详细步骤请参见[作业编辑](/intl.zh-CN/数据开发/作业编辑.md)，这里我们以MapReduce为例，分别填写所属项目、作业名称、作业描述、并选择作业类型MR。

    ![create_job](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/2832598951/p11891.jpg)

6.  配置作业内容，单击**运行**。

    ![run](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/2832598951/p11892.png)

    -   关于OSS使用，请参见[OSS 参考使用说明](/intl.zh-CN/开发指南/准备/OSS 参考使用说明.md)。
    -   关于各类作业的具体开发，请参见[作业](/intl.zh-CN/数据开发/作业/Shell作业配置.md)部分。
    **说明：**

    -   OSS输出路径如果已经存在，在执行作业时会报错。
    -   插入OSS路径时，如果选择OSSREF文件前缀，系统会把OSS文件下载到集群本地，并添加到classpath中。
    -   当前所有操作都只支持标准存储类型的OSS。

## 查看日志

作业运行成功后，您可以在页面下方的运行记录页签中查看作业的运行日志。单击**详情**跳转到运行记录中该作业的详细日志页面，可以查看作业的提交日志、YARN Container日志。您也可以通过SSH方式登录到集群查看相关日志，详细步骤请参见[使用SSH连接主节点](/intl.zh-CN/集群管理/集群配置/连接集群/使用SSH连接主节点.md)。

## 总结

至此，我们成功在E-MapReduce上部署一套Hadoop集群，将OSS服务作为数据源输入，并运行MapReduce作业消费OSS上数据。E-MapReduce支持多种类型作业的开发，例如，Storm作业消费Kafka数据， Spark Streaming和Flink组件，同样可以方便地在Hadoop集群上运行，处理Kafka数据。

