# E-MapReduce 快速开始 {#concept_jkh_sb4_y2b .concept}

通过本教程，用户能够基本了解E-MapReduce中集群、作业的作用和使用方法。能够创建一个Spark Pi的作业在集群上运行成功，并最后在控制台页面上看到圆周率Pi的近似计算结果。

**说明：** 请确认您已经完成了必选的[准备工作](intl.zh-CN/快速入门/准备工作.md#)。

1.  创建集群。
    1.  在[阿里云 E-MapReduce 控制台](https://emr.console.aliyun.com/)单击上方的集群管理页签，进入集群列表页面，并单击右上**创建集群**。
    2.  软件配置。
        1.  选择最新的EMR产品版本，比如**EMR-3.13.0**。
        2.  使用默认软件配置。
    3.  硬件配置
        1.  选择**按量付费**。
        2.  若没有安全组，请输入新的安全组名称新建。
        3.  选择 Master 4核8G。
        4.  选择 Core 4核8G， 两台。
        5.  其他保持默认。
    4.  基础配置。
        1.  填写集群名称。
        2.  选择日志路径保存作业日志，请务必开启运行日志服务。在集群对应的地域，[创建OSS的Bucket](../../../../intl.zh-CN/快速入门/创建存储空间.md#)。
        3.  填写密码。
    5.  确认。
2.  创建作业。
    1.  单击上方的数据开发页签，进入项目列表页面，并单击右上**新建项目**。
    2.  在新建项目对话框输入项目名称和项目描述，单击**创建**。
    3.  单击对应项目右侧的**工作流设计**，进入**作业编辑**页面。
    4.  在页面左侧，在需要操作的文件夹上单击右键，选择**新建作业**。
    5.  填写作业名称和作业描述。
    6.  选择Spark类型。
    7.  单击**确定**。

        **说明：** 您还可以通过在文件夹上单击右键，进行创建子文件夹、重命名文件夹和删除文件夹操作。

    8.  参数填写，使用如下。

        ```
        --class org.apache.spark.examples.SparkPi --master yarn-client --driver-memory 512m --num-executors 1 --executor-memory 1g --executor-cores 2 /usr/lib/spark-current/examples/jars/spark-examples_2.11-2.1.1.jar 10
        ```

        **说明：** 这个`/usr/lib/spark-current/examples/jars/spark-examples_2.11-2.1.1.jar`, 需要根据实际集群中的 Spark 版本来修改这个jar包，比如 Spark 是2.1.1的， 那么就是`spark-examples_2.11-2.1.1.jar`,如果是2.2.0的，那么就是`spark-examples_2.11-2.2.0.jar`。

    9.  单击**运行**。
3.  查看作业日志并确认结果。

    作业运行后，您可以在页面下方的运行记录页签中查看作业的运行日志。单击**详情**跳转到运行记录中该作业的详细日志页面，可以看到作业的提交日志、YARN Container日志。


