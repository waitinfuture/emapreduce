# Spark 作业配置 {#concept_g1f_4hp_y2b .concept}

Spark 作业配置

## 操作步骤 {#section_p4m_rhp_y2b .section}

1.  通过主账号登录[阿里云 E-MapReduce 控制台](https://emr.console.aliyun.com/)，进入集群列表页面。
2.  单击上方的数据开发页签，进入项目列表页面。
3.  单击对应项目右侧的**工作流设计**，进入作业编辑页面。
4.  在页面左侧，在需要操作的文件夹上单击右键，选择**新建作业**。
5.  填写作业名称，作业描述。
6.  选择 Spark 作业类型，表示创建的作业是一个 Spark 作业。Spark 作业在 E-MapReduce 后台使用以下的方式提交：

    ```
    spark-submit [options] --class [MainClass] xxx.jar args
    ```

7.  单击**确定**。

    **说明：** 您还可以通过在文件夹上单击右键，进行创建子文件夹、重命名文件夹和删除文件夹操作。

8.  在**作业内容**输入框中填写提交该 Spark 作业需要的命令行参数。请注意，应用参数框中只需要填写spark-submit之后的参数即可。以下分别示例如何填写创建 Spark 作业和 pyspark 作业的参数。
    -   创建 Spark 作业

        新建一个 Spark WordCount 作业。

        -   作业名称： Wordcount
        -   类型：选择 Spark
        -   应用参数：

            -   在命令行下完整的提交命令是：

                ```
                spark-submit --master yarn-client --driver-memory 7G --executor-memory 5G --executor-cores 1 --num-executors 32 --class com.aliyun.emr.checklist.benchmark.SparkWordCount emr-checklist_2.10-0.1.0.jar oss://emr/checklist/data/wc oss://emr/checklist/data/wc-counts 32
                ```

            -   在 E-MapReduce 作业的应用参数框中只需要填写：

                ```
                --master yarn-client --driver-memory 7G --executor-memory 5G --executor-cores 1 --num-executors 32 --class com.aliyun.emr.checklist.benchmark.SparkWordCount ossref://emr/checklist/jars/emr-checklist_2.10-0.1.0.jar oss://emr/checklist/data/wc oss://emr/checklist/data/wc-counts 32
                ```

            **说明：** 作业 Jar 包保存在 OSS 中，引用这个 Jar 包的方式是 ossref://emr/checklist/jars/emr-checklist\_2.10-0.1.0.jar。您可以单击**选择 OSS 路径**，从 OSS 中进行浏览和选择，系统会自动补齐 OSS 上 Spark 脚本的绝对路径。请务必将默认的 oss 协议切换成 ossref 协议。

    -   创建 pyspark 作业

        E-MapReduce 除了支持 Scala 或者 Java 类型作业外，还支持 python 类型 Spark 作业。以下新建一个 python 脚本的 Spark Kmeans 作业。

        -   作业名称：Python-Kmeans
        -   类型：Spark
        -   应用参数：

            ```
            --master yarn-client --driver-memory 7g --num-executors 10 --executor-memory 5g --executor-cores 1  ossref://emr/checklist/python/kmeans.py oss://emr/checklist/data/kddb 5 32
            ```

        -   支持 Python 脚本资源的引用，同样使用 ossref 协议。
        -   pyspark 目前不支持在线安装 Python 工具包。
9.  单击**保存**，Spark 作业即定义完成。

