# Spark作业配置

本文介绍如何配置Spark类型的作业。

已创建好项目，详情请参见[项目管理](/cn.zh-CN/数据开发/项目管理.md)。

## 操作步骤

1.  通过主账号登录[阿里云E-MapReduce控制台](https://emr.console.aliyun.com)。

2.  在顶部菜单栏处，根据实际情况选择地域（Region）和资源组。

3.  单击上方的**数据开发**页签。

4.  在**项目列表**页面，单击待编辑项目所在行的**作业编辑**。

5.  在**作业编辑**区域，在需要操作的文件夹上，右键选择**新建作业**。

6.  输入**作业名称**、**作业描述**，在**作业类型**下拉列表中选择**Spark**作业类型。

    表示创建的作业是一个Spark作业。这种类型的作业，其运行实际是通过以下方式提交的Spark作业。

    ```
    spark-submit [options] --class [MainClass] xxx.jar args
    ```

7.  单击**确定**。

8.  在**作业内容**中，填写提交该作业需要提供的命令行参数。

    只需要填写spark-submit之后的参数即可。

    以下分别展示如何填写创建Spark作业和Pyspark作业的参数：

    -   创建Spark作业 。

        新建一个Spark WordCount作业。

        -   作业名称： Wordcount。
        -   类型：选择Spark。
        -   应用参数：
            -   在命令行下提交完整的命令。

                ```
                spark-submit --master yarn-client --driver-memory 7G --executor-memory 5G --executor-cores 1 --num-executors 32 --class com.aliyun.emr.checklist.benchmark.SparkWordCount emr-checklist_2.10-0.1.0.jar oss://emr/checklist/data/wc oss://emr/checklist/data/wc-counts 32
                ```

            -   在E-MapReduce作业的**作业内容**输入框中填写如下命令。

                ```
                --master yarn-client --driver-memory 7G --executor-memory 5G --executor-cores 1 --num-executors 32 --class com.aliyun.emr.checklist.benchmark.SparkWordCount ossref://emr/checklist/jars/emr-checklist_2.10-0.1.0.jar oss://emr/checklist/data/wc oss://emr/checklist/data/wc-counts 32
                ```

                **说明：** JAR包保存在OSS中，引用这个JAR包的方式是ossref://emr/checklist/jars/emr-checklist\_2.10-0.1.0.jar。您可以单击下方的**+插入OSS路径**，**文件前缀**选择**OSSREF**，从**文件路径**中进行浏览和选择，系统会自动补齐OSS上Spark脚本的路径。

    -   创建Pyspark作业。

        E-MapReduce除了支持Scala或者Java类型作业外，还支持Python类型Spark作业。以下是新建一个Python脚本的Spark Kmeans作业。

        -   作业名称：Python-Kmeans。
        -   类型：Spark。
        -   应用参数。

            ```
            --master yarn-client --driver-memory 7g --num-executors 10 --executor-memory 5g --executor-cores 1  ossref://emr/checklist/python/kmeans.py oss://emr/checklist/data/kddb 5 32
            ```

            -   支持Python脚本资源的引用，同样使用ossref协议。
            -   Pyspark目前不支持在线安装Python工具包。
9.  单击**保存**，作业配置即定义完成。


## 问题反馈

如果您在使用阿里云E-MapReduce过程中有任何疑问，欢迎您扫描下面的二维码加入钉钉群进行反馈。

![emr_dingding](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/2440659951/p81620.png)

