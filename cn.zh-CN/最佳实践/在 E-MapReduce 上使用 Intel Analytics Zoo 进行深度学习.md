# 在 E-MapReduce 上使用 Intel Analytics Zoo 进行深度学习 {#concept_awn_wmp_kfb .concept}

本文简单介绍了如何在阿里云 E-MapReduce 使用 Analytics Zoo 来进行深度学习。

## 简介 {#section_nfv_bnp_kfb .section}

Analytics Zoo 是由 Intel 开源，基于Apache Spark 和 Inte BigDL 的大数据分析和 AI 平台，方便用户开发基于大数据、端到端的深度学习应用。

## 系统要求 {#section_mkn_2np_kfb .section}

-   JDK 8
-   Spark 集群\(推荐使用EMR支持的 Spark 2.x\)
-   python-2.7\(python 3.5，3.6 也支持\)，pip

## 安装Analytics Zoo {#section_iqp_hnp_kfb .section}

-   Analytics Zoo 最新的 release 版本是 0.2.0
-   Scala 安装
    -   下载 pre-build 版本

        可以从 github，analytics 主页下载到[pre-build 版本](https://analytics-zoo.github.io/master/#release-download/)

    -   通过 script build

        安装 Apache Maven，设置 Maven 环境：

        ```
        export MAVEN_OPTS="-Xmx2g -XX:ReservedCodeCacheSize=512m"
        ```

        如果使用 ECS 机器进行编译，推荐修改 Maven 仓库 mirror：

        ```
        <mirror>
            <id>nexus-aliyun</id>
            <mirrorOf>central</mirrorOf>
            <name>Nexus aliyun</name>
            <url>http://maven.aliyun.com/nexus/content/groups/public</url>
        </mirror>
        ```

        下载 [Analytics Zoo release版本](https://github.com/intel-analytics/analytics-zoo),解压后在目录下运行：

        ```
        bash make-dist.sh
        ```

        build 结束后，在 dist 目录中包含了所有的运行环境。将 dist 目录放到 EMR 软件栈运行时统一目录：

        ```
        cp -r dist/ /usr/lib/analytics_zoo
        ```

-   python 安装

    Analytics Zoo 支持 pip 安装和非 pip 安装，pip 安装会安装 pyspark，bigdl等，由于EMR 集群已经安装了 pyspark，通过 pip 安装有可能引起冲突，所以采用非 pip 安装。

    -   非 pip 安装

        首先要运行：

        ```
        bash make-dist.sh
        ```

        进入 pyzoo 目录，安装 analytcis zoo：

        ```
        python setup.py install
        ```

-   设置环境变量

    在 scala 安装结束后将 dist 目录放到了 EMR 软件栈统一目录，然后设置环境变量。编辑 /etc/profile.d/analytics\_zoo.sh，加入：

    ```
    export ANALYTICS_ZOO_HOME=/usr/lib/analytics_zoo
    export PATH=$ANALYTICS_ZOO_HOME/bin:$PATH
    ```

    EMR 已经设置了 SPARK\_HOME，所以无需再次设置。


## 使用Analytics Zoo {#section_wlv_nqp_kfb .section}

-   使用 Spark 来训练和测试深度学习模型
    -   使用 Analytics Zoo 来做文本分类，代码和说明在[github](https://github.com/intel-analytics/analytics-zoo/tree/master/zoo/src/main/scala/com/intel/analytics/zoo/examples/textclassification)。根据说明下载必须的数据。提交命令：

```
spark-submit --master yarn \
--deploy-mode cluster --driver-memory 8g \
--executor-memory 20g --class com.intel.analytics.zoo.examples.textclassification.TextClassification \
/usr/lib/analytics_zoo/lib/analytics-zoo-bigdl_0.6.0-spark_2.1.0-0.2.0-jar-with-dependencies.jar --baseDir /news
```

    -   通过 [ssh proxy](../../../../../intl.zh-CN/快速入门/SSH 登录集群.md#)来查看Spark运行详情页面。

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/23346/155350392913534_zh-CN.png)

        同时查看日志，能够看到每个epoch的accuracy信息等。

        ```
        INFO optim.DistriOptimizer$: [Epoch 2 9600/15107][Iteration 194][Wall Clock 193.266637037s] Trained 128 records in 0.958591653 seconds. Throughput is 133.52922 records/second. Loss is 0.74216986.
        INFO optim.DistriOptimizer$: [Epoch 2 9728/15107][Iteration 195][Wall Clock 194.224064816s] Trained 128 records in 0.957427779 seconds. Throughput is 133.69154 records/second. Loss is 0.51025534.
        INFO optim.DistriOptimizer$: [Epoch 2 9856/15107][Iteration 196][Wall Clock 195.189488678s] Trained 128 records in 0.965423862 seconds. Throughput is 132.58424 records/second. Loss is 0.553785.
        INFO optim.DistriOptimizer$: [Epoch 2 9984/15107][Iteration 197][Wall Clock 196.164318688s] Trained 128 records in 0.97483001 seconds. Throughput is 131.30495 records/second. Loss is 0.5517549.
        ```

-   在 Analytics Zoo 中使用pyspark 和 Jupyter 来进行深度学习训练
    -   安装 Jupyter

        ```
        pip install jupyter
        ```

    -   使用以下命令启动：

        ```
        jupyter-with-zoo.sh
        ```

    -   使用 Analytics Zoo，推荐采用内置的 Wide And Deep 模型来进行。
        1.  导入数据

            ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/23346/155350392913535_zh-CN.png)

        2.  定义模型和优化器

            ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/23346/155350392913536_zh-CN.png)

        3.  进行训练

            ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/23346/155350392913537_zh-CN.png)

        4.  查看训练结果

            ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/23346/155350392913538_zh-CN.png)

            ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/23346/155350393013539_zh-CN.png)


