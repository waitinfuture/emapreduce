# 使用E-MapReduce处理离线作业 {#concept_bzz_zg1_dfb .concept}

本文介绍使用E-MapReduce产品从OSS服务上读取数据 ，并进行数据采集、清洗等一系列离线数据处理的使用场景。

## 前言 {#section_qlt_kh1_dfb .section}

E-MapReduce 集群适用场景很多。简单说来，Hadoop ecosystem 以及 Spark 能够支持的场景，E-MapReduce 都可以支持。因为 E-MapReduce 本质就是 Hadoop 和 Spark 的集群服务，您完全可以将EMR集群使用的阿里云 ECS 主机视为自己专属的物理主机。

大数据处理目前比较流行的有两种方法，一种是离线处理，一种是在线处理。

-   离线处理：只是希望得到数据的分析结果，对处理的时间要求不严格，例如批量数据处理，用户将数据传输到OSS服务，OSS服务作为EMR产品的输入输出，利用MapReduce,Hive,Pig,Spark处理离线数据。
-   在线处理：对于数据的分析结果在时间上有比较严格的要求，例如实时流式数据处理，使用Spark Streaming对消息数据进行处理，与Spark mllib/GrapX/SQL深度整合。

本文将介绍使用E-MapReduce产品运行一个"word count"离线作业。

## 基本架构 {#section_km4_qh1_dfb .section}

OSS -\> EMR -\> Hadoop MapReduce

上述链路主要包含2个过程：

1.  把数据存储到OSS服务里。
2.  通过EMR服务将OSS中的数据读取出来，进行分析。

## 环境准备 {#section_krn_vh1_dfb .section}

-   本文以Windows环境为例，请确保Git，Maven, Java已经安装并配置成功。
-   本文使用阿里云EMR服务自动化搭建Hadoop集群，详细过程请参考[创建集群](https://help.aliyun.com/document_detail/35223.html?spm=a2c4e.11153940.blogcont637482.18.3e1625a1TUjLXZ)。
    -   EMR版本: EMR-3.12.1
    -   集群类型: HADOOP
    -   软件信息: HDFS2.7.2 / YARN2.7.2 / Hive2.3.3 / Ganglia3.7.2 / Spark2.3.1 / HUE4.1.0 / Zeppelin0.8.0 / Tez0.9.1 / Sqoop1.4.7 / Pig0.14.0 / ApacheDS2.0.0 / Knox0.13.0
    -   Hadoop集群使用专有网络，区域为华东1（杭州），主实例组ECS计算资源配置公网及内网IP，高可用选择为否（非HA模式），具体配置如下所示。

## 操作步骤 {#section_f2w_y31_dfb .section}

1.  下载示例代码到本地

    在本地打开git bash运行clone命令：

    ```
    git clone https://github.com/aliyun/aliyun-emapreduce-demo.git
    ```

    执行`mvn install`进行编译。

2.  创建存储空间，详细过程请参考[创建存储空间](https://help.aliyun.com/document_detail/31885.html?spm=a2c4g.11186623.6.562.ebe065d3fktE5s)

    **说明：** Bucket应与E-MapReduce集群在同一个区域。

3.  上传jar包和资源文件
    1.  登录[OSS 管理控制台](https://oss.console.aliyun.com/?spm=a2c4g.11186623.2.1.3bd46ab3PloX1H)，单击**文件管理**。
    2.  单击**上传文件**，上传aliyun-emapreduce-demo/resources目录下的资源文件以及aliyun-emapreduce-demo/target目录下的jar文件。
4.  创建工作流项目

    详细过程请参考[工作流项目管理](https://help.aliyun.com/document_detail/85392.html?spm=a2c4g.11186623.6.586.17f5400cQ1U2Tw)。

5.  创建作业

    详细过程请参考[作业编辑](https://help.aliyun.com/document_detail/85446.html?spm=a2c4g.11186623.6.587.627668cdngyU2m)，这里我们以MapReduce为例。

6.  配置作业内容，点击**运行**。如图所示：

    -   关于OSS使用，请参考[OSS 参考使用说明](https://www.alibabacloud.com/help/zh/doc-detail/42799.html?spm=a2c5t.11065259.1996646101.searchclickresult.63fc71f3FgiO9g)
    -   关于各类作业的具体开发，请参见EMR用户指南[作业开发](https://help.aliyun.com/document_detail/28095.html)部分。
    **说明：** 

    -   OSS输出路径如果已经存在，在执行作业时会报错。
    -   插入OSS路径时，如果选择OSSREF文件前缀，系统会把OSS文件下载到集群本地，并添加到classpath中。
    -   当前所有操作都只支持标准存储类型的OSS。 

## 查看日志 {#section_drj_5k1_dfb .section}

关于查看执行计划日志的详细步骤，请参考[SSH 登录集群](https://help.aliyun.com/document_detail/28187.html?spm=a2c4g.11186623.6.640.24b454c4CAFUqC)。

