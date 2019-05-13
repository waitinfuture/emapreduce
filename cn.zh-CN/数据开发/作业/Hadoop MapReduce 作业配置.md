# Hadoop MapReduce 作业配置 {#concept_rtn_r1p_y2b .concept}

本文介绍 Hadoop MapReduce 作业配置的操作步骤。

1.  通过主账号登录[阿里云 E-MapReduce 控制台](https://emr.console.aliyun.com/)。
2.  单击上方的数据开发页签，进入项目列表页面。
3.  单击对应项目右侧的**工作流设计**，在左侧导航栏中单击**作业编辑**进入作业编辑页面。
4.  在页面左侧，在需要操作的文件夹上单击右键，选择**新建作业**。
5.  填写作业名称，作业描述；选择 Hadoop 作业类型。表示创建的作业是一个 Hadoop Mapreduce 作业。这种类型的作业，其后台实际上是通过以下的方式提交的 Hadoop 作业。

    ```
    hadoop jar xxx.jar [MainClass] -Dxxx ....
    ```

6.  单击**确定**。

    **说明：** 您还可以通过在文件夹上单击右键，进行创建子文件夹、重命名文件夹和删除文件夹操作。

7.  在**作业内容**输入框中填写提交该作业需要提供的命令行参数。所填写的命令行参数需要自jadoop jar 命令后的第一个参数开始填写，即在输入框中首先填写运行该作业所需的 jar 包所在路径，再填写 \[MainClass\] 和其它您想要设置的命令行参数。

    例如，您想要提交一个 Hadoop 的 sleep 作业，该作业不读写任何数据、只提交一些 mapper 和 reducer task 到集群中，且每个 task 执行时需要 sleep 一段时间。在 Hadoop（以 hadoop-2.6.0版本为例）中，该作业处于 Hadoop 发行版的 hadoop-mapreduce-client-jobclient-2.6.0-tests.jar 包文件中。这种情况下，如果您通过命令行的方式提交该作业，需要执行以下命令：

    ```
    hadoop jar /path/to/hadoop-mapreduce-client-jobclient-2.6.0-tests.jar sleep -m 3 -r 3 -mt 100 -rt 100
    ```

    而在 E-MapReduce 中配置这个作业，则应在**作业内容**输入框中填写以下内容：

    ```
    /path/to/hadoop-mapreduce-client-jobclient-2.6.0-tests.jar sleep -m 3 -r 3 -mt 100 -rt 100
    ```

    **说明：** 

    这里用的 jar 包路径是 E-MapReduce 宿主机上的一个绝对路径，这种方式有一个问题，就是用户可能会将这些 jar 包放置在任何位置，而且随着集群的创建和释放，这些 jar 包也会跟着释放而变得不可用。所以，请使用以下方法上传 jar 包：

    1.  用户将自己的 jar 包上传到 OSS 的 bucket 中进行存储，当配置 Hadoop 的参数时，单击**选择 OSS 路径**，从 OSS 目录中进行选择要执行的 jar 包。系统会为用户自动补齐 jar 包所在的 OSS 地址。请务必将代码的 jar 的前缀切换为 ossref （单击**切换资源类型**），以保证这个 jar 包会被 E-MapReduce 正确下载。
    2.  单击**确定**，该 jar 包所在的 OSS 路径地址就会自动填充到**应用参数**选项框中。作业提交的时候，系统能够根据这个路径地址自动从 OSS 找到相应的 jar 包。
    3.  在该 OSS 的 jar 包路径后面，即可进一步填写作业运行的其他命令行参数。
8.  单击**保存**，作业配置即定义完成。

上面的例子中，sleep 作业并没有数据的输入输出，如果作业要读取数据，并输出处理结果（比如 wordcount），则需要指定数据的 input 路径和 output 路径。用户可以读写 E-MapReduce 集群 HDFS 上的数据，同样也可以读写 OSS 上的数据。如果需要读写 OSS 上的数据，只需要在填写 input 路径和 output 路径时，数据路径写成 OSS 上的路径地址即可，例如：

```
jar ossref://emr/checklist/jars/chengtao/hadoop/hadoop-mapreduce-examples-2.6.0.jar randomtextwriter -D mapreduce.randomtextwriter.totalbytes=320000 oss://emr/checklist/data/chengtao/hadoop/Wordcount/Input
```

