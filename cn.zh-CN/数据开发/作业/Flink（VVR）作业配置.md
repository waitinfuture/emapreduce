# Flink（VVR）作业配置

EMR-3.27.x及之前版本使用Flink社区开源版本，EMR-3.27.x之后版本使用完全兼容开源Flink的企业版（VVR）。本文介绍如何配置Flink（VVR）类型的作业。

-   已创建Hadoop集群，详情请参见[创建集群](/cn.zh-CN/集群管理/集群配置/创建集群.md)。

    **说明：** 如果您创建的是Dataflow集群，目前仅支持在Flink-Vvp控制台创建Flink SQL和Flink DataStream两种作业，详情如下：

    -   [作业开发（Flink SQL）](/cn.zh-CN/集群类型/Dataflow集群/Flink-Vvp/Flink SQL作业/作业开发.md)
    -   [作业开发（Flink DataStream）](/cn.zh-CN/集群类型/Dataflow集群/Flink-Vvp/Flink Datastream作业/作业开发.md)
-   已创建项目，详情请参见[项目管理](/cn.zh-CN/数据开发/项目管理.md)。
-   已获取作业所需的资源，以及作业需要处理的数据文件，例如，JAR包、数据文件名称及其保存路径。

Flink企业版由Apache Flink创始团队官方出品，拥有全球统一商业化品牌。

VVR提供企业版StateBackend，性能是开源版本的3~5倍。在EMR Hadoop集群中，您可使用VVR引擎和EMR数据开发功能提交作业。VVR支持开源Flink 1.10版本，默认使用商业GeminiStateBackend，具备以下特性：

-   采用创新的数据结构，提高随机查询、降低读磁盘I/O的性能。
-   优化Cache策略，内存充足情况下热数据不落盘，并且Compaction后Cache不会失效。
-   完全使用Java实现，消除RocksDB的JNI开销。
-   使用堆外内存，并基于GeminiDB的特点实现高效的内存分配器，消除JVM GC带来的影响。
-   支持异步增量Checkpoint，同步阶段只进行内存索引的拷贝，相较于RocksDB可以避免I/O带来的抖动。
-   支持Local Recovery和Timer落盘。

**说明：** 如果您想使用GeminiStateBackend，请不要在代码中指定StateBackend类型。使用GeminiStateBackend启动时，TM的内存不少于1728 MB。

Flink中Checkpoint和StateBackend的基础配置同样适用于GeminiStateBackend，具体请参见[Configuration](https://ci.apache.org/projects/flink/flink-docs-release-1.10/ops/config.html#checkpoints-and-state-backends)。

您可以根据具体需求配置参数，部分特殊参数设置如下。

|参数|说明|
|--|--|
|**state.backend.gemini.memory.managed**|默认值为true，表示将自动根据Managed Memory以及Task Slot数计算每个Backend的内存，包括： -   true
-   false |
|**state.backend.gemini.offheap.size**|默认值为2GB，当**state.backend.gemini.memory.managed**为false时，设置每个Backend的内存。|
|**state.backend.gemini.local.dir**|表示GeminiDB本地数据文件的存放目录。|
|**state.backend.gemini.timer-service.factory**|默认值为HEAP，表示timer-service state的存储位置，包括： -   HEAP
-   GEMINI |

**说明：** 参数配置方法请参见[组件参数配置](/cn.zh-CN/集群管理/集群配置/组件参数配置.md)。

## 操作步骤

1.  通过主账号登录[阿里云E-MapReduce控制台](https://emr.console.aliyun.com)。

2.  在顶部菜单栏处，根据实际情况选择地域（Region）和资源组。

3.  单击上方的**数据开发**页签。

4.  在**项目列表**页面，单击待编辑项目所在行的**作业编辑**。

5.  在**作业编辑**区域，在需要操作的文件夹上，右键选择**新建作业**。

6.  输入**作业名称**、**作业描述**，在**作业类型**下拉列表中选择**Flink**作业类型。

7.  单击**确定**。

8.  在**作业内容**中，填写提交该作业需要提供的命令行参数。

    -   Flink类型作业可以支持以JAR包形式的Flink Datastream、Table和SQL作业，示例如下。

        ```
        run -m yarn-cluster -yjm 1024 -ytm 2048 ossref://path/to/oss/of/WordCount.jar --input oss://path/to/oss/to/data --output oss://path/to/oss/to/result
        ```

    -   EMR-3.x版本自EMR-3.28.2版本开始，Flink类型作业同时支持PyFlink作业，示例如下。

        ```
        run -m yarn-cluster -yjm 1024 -ytm 2048 -py ossref://path/to/oss/of/word_count.py
        ```

        PyFlink作业其它可用参数详情请参见[Apache Flink官方文档](https://ci.apache.org/projects/flink/flink-docs-release-1.10/ops/cli.html#usage)。

9.  单击**保存**。

    您可以根据集群的版本来访问Flink的Web UI：

    -   EMR-3.29.0之前版本

        仅支持通过SSH隧道方式访问Web UI时，请参见[通过SSH隧道方式访问开源组件Web UI](/cn.zh-CN/集群管理/集群配置/连接集群/通过SSH隧道方式访问开源组件Web UI.md)。

    -   EMR-3.29.0及后续版本
        -   （推荐）您可以通过EMR控制台的方式访问Web UI时，请参见[访问链接与端口](/cn.zh-CN/集群管理/集群配置/访问链接与端口.md)。
        -   您可以通过SSH隧道方式访问Web UI时，请参见[通过SSH隧道方式访问开源组件Web UI](/cn.zh-CN/集群管理/集群配置/连接集群/通过SSH隧道方式访问开源组件Web UI.md)。

## 问题反馈

如果您在使用阿里云E-MapReduce过程中有任何疑问，欢迎您扫描下面的二维码加入钉钉群进行反馈。

![emr_dingding](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/2440659951/p81620.png)

