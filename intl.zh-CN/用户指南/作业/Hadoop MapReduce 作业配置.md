# Hadoop MapReduce 作业配置 {#concept_rtn_r1p_y2b .concept}

Hadoop MapReduce 作业配置

操作步骤

1.  通过主账号登录[阿里云 E-MapReduce 控制台](https://emr.console.aliyun.com/)，进入集群列表页面。
2.  单击上方的数据开发页签，进入项目列表页面。
3.  单击对应项目右侧的**工作流设计**，进入作业编辑页面。
4.  在页面左侧，在需要操作的文件夹上单击右键，选择**新建作业**。
5.  填写作业名称，作业描述。
6.  选择 Hadoop 作业类型。表示创建的作业是一个 Hadoop Mapreduce 作业。这种类型的作业，其后台实际上是通过以下的方式提交的 Hadoop 作业。

    ```
    hadoop jar xxx.jar [MainClass] -Dxxx ....
    ```

7.  单击**确定**。

    **说明：** 您还可以通过在文件夹上单击右键，进行创建子文件夹、重命名文件夹和删除文件夹操作。

8.  在**作业内容**输入框中填写提交该 job 需要提供的命令行参数。这里需要说明的是，这个选项框中需要填写的内容从 hadoop jar 后面的第一个参数开始填写。也就是说，选项框中第一个要填写的是运行该作业需要提供的 jar 包所在地址，然后后面紧跟 \[MainClass\] 以及其他用户可以自行提供的命令行参数。

    举个例子，假设用户想要提交一个 Hadoop 的 sleep job，该 jo b不读写任何数据，只是提交一些 mapper 和 reducer task 到集群中，每个 task sleep 一段时间，然后 job 成功。在 Hadoop 中（hadoop-2.6.0 为例）以，该 job 被打包在 Hadoop 发行版的 hadoop-mapreduce-client-jobclient-2.6.0-tests.jar 中。那么，若是在命令行中提交该 job，则命令如下：

    ```
    hadoop jar /path/to/hadoop-mapreduce-client-jobclient-2.6.0-tests.jar sleep -m 3 -r 3 -mt 100 -rt 100
    ```

    要在 E-MapReduce 中配置这个作业，那么作业配置页面的**作业内容**输入框中，需要填写的内容即为：

    ```
    /path/to/hadoop-mapreduce-client-jobclient-2.6.0-tests.jar sleep -m 3 -r 3 -mt 100 -rt 100
    ```

    **说明：** 

    这里用的 jar 包路径是 E-MapReduce 宿主机上的一个绝对路径，这种方式有一个问题，就是用户可能会将这些 jar 包放置在任何位置，而且随着集群的创建和释放，这些 jar 包也会跟着释放而变得不可用。所以，请使用以下方法上传 jar 包：

    1.  用户将自己的 jar 包上传到 OSS 的 bucket 中进行存储，当配置 Hadoop 的参数时，单击**选择 OSS 路径**，从 OSS 目录中进行选择要执行的 jar 包。系统会为用户自动补齐 jar 包所在的 OSS 地址。请务必将代码的 jar 的前缀切换为 ossref （单击**切换资源类型**），以保证这个 jar 包会被 E-MapReduce 正确下载。
    2.  单击**确定**，该 jar 包所在的 OSS 路径地址就会自动填充到**应用参数**选项框中。作业提交的时候，系统能够根据这个路径地址自动从 OSS 找到相应的 jar 包。
    3.  在该 OSS 的 jar 包路径后面，即可进一步填写作业运行的其他命令行参数。
9.  单击**保存**，作业配置即定义完成。

上面的例子中，sleep job 并没有数据的输入输出，如果作业要读取数据，并输出处理结果（比如 wordcount），则需要指定数据的 input 路径和 output 路径。用户可以读写 E-MapReduce 集群 HDFS 上的数据，同样也可以读写 OSS 上的数据。如果需要读写 OSS 上的数据，只需要在填写 input 路径和 output 路径时，数据路径写成 OSS 上的路径地址即可，例如：

```
jar ossref://emr/checklist/jars/chengtao/hadoop/hadoop-mapreduce-examples-2.6.0.jar randomtextwriter -D mapreduce.randomtextwriter.totalbytes=320000 oss://emr/checklist/data/chengtao/hadoop/Wordcount/Input
```

