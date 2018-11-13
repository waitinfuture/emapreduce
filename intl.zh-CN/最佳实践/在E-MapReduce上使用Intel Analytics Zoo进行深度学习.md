# 在E-MapReduce上使用Intel Analytics Zoo进行深度学习 {#concept_awn_wmp_kfb .concept}

Analytics Zoo是由Intel开源,基于Apache Spark和Inte BigDL的大数据分析和AI平台，方便用户开发基于大数据、端到端的深度学习应用。本文简单介绍了如何在Aliyun EMR使用Analytics Zoo来进行深度学习。

## 简介 {#section_nfv_bnp_kfb .section}

Analytics Zoo是由Intel开源,基于Apache Spark和Inte BigDL的大数据分析和AI平台，方便用户开发基于大数据、端到端的深度学习应用。

## 系统要求 {#section_mkn_2np_kfb .section}

-   JDK 8
-   Spark 集群\(推荐使用EMR支持的Spark 2.x\)
-   python-2.7\(python 3.5,3.6也支持\), pip

## 安装Analytics Zoo {#section_iqp_hnp_kfb .section}

-   Analytics Zoo 最新的release版本是0.2.0
-   Scala安装
    -   下载pre-build版本

        可以从github，analytics主页下载到[pre-build版本](https://analytics-zoo.github.io/master/#release-download/)

    -   通过script build

        安装Apache Maven,设置Maven环境：

        ```
        export MAVEN_OPTS="-Xmx2g -XX:ReservedCodeCacheSize=512m"
        ```

        如果使用ECS机器进行编译，推荐修改Maven仓库mirror：

        ```
        <mirror>
            <id>nexus-aliyun</id>
            <mirrorOf>central</mirrorOf>
            <name>Nexus aliyun</name>
            <url>http://maven.aliyun.com/nexus/content/groups/public</url>
        </mirror>
        ```

        下载[Analytics Zoo release版本](https://github.com/intel-analytics/analytics-zoo),解压后在目录下运行：

        ```
        bash make-dist.sh
        ```

        build结束后，在dist目录中包含了所有的运行环境。将dist目录放到EMR软件栈运行时统一目录：

        ```
        cp -r dist/ /usr/lib/analytics_zoo
        ```

-   python 安装

    Analytics Zoo支持pip安装和非pip安装，pip安装会安装pyspark，bigdl等，由于EMR集群已经安装了pyspark，通过pip安装有可能引起冲突，所以采用非pip安装。

    -   非Pip安装

        首先要运行：

        ```
        bash make-dist.sh
        ```

        进入pyzoo目录，安装analytcis zoo：

        ```
        python setup.py install
        ```

-   设置环境变量

    在scala安装结束后将dist目录放到了EMR软件栈统一目录，然后设置环境变量。编辑/etc/profile.d/analytics\_zoo.sh，加入：

    ```
    export ANALYTICS_ZOO_HOME=/usr/lib/analytics_zoo
    export PATH=$ANALYTICS_ZOO_HOME/bin:$PATH
    ```

    EMR已经设置了SPARK\_HOME，所以无需再次设置。


## 使用Analytics Zoo {#section_wlv_nqp_kfb .section}

-   使用Spark来训练和测试深度学习模型
    -   使用Analytics Zoo来做文本分类，代码和说明在[github](https://github.com/intel-analytics/analytics-zoo/tree/master/zoo/src/main/scala/com/intel/analytics/zoo/examples/textclassification)。根据说明下载必须的数据。提交命令：

```
spark-submit --master yarn \
--deploy-mode cluster --driver-memory 8g \
--executor-memory 20g --class com.intel.analytics.zoo.examples.textclassification.TextClassification \
/usr/lib/analytics_zoo/lib/analytics-zoo-bigdl_0.6.0-spark_2.1.0-0.2.0-jar-with-dependencies.jar --baseDir /news
```

    -   通过[ssh proxy](../../../../intl.zh-CN/用户指南/SSH 登录集群.md#)来查看Spark运行详情页面。

        同时查看日志，能够看到每个epoch的accuracy信息等。

        ```
        INFO optim.DistriOptimizer$: [Epoch 2 9600/15107][Iteration 194][Wall Clock 193.266637037s] Trained 128 records in 0.958591653 seconds. Throughput is 133.52922 records/second. Loss is 0.74216986.
        INFO optim.DistriOptimizer$: [Epoch 2 9728/15107][Iteration 195][Wall Clock 194.224064816s] Trained 128 records in 0.957427779 seconds. Throughput is 133.69154 records/second. Loss is 0.51025534.
        INFO optim.DistriOptimizer$: [Epoch 2 9856/15107][Iteration 196][Wall Clock 195.189488678s] Trained 128 records in 0.965423862 seconds. Throughput is 132.58424 records/second. Loss is 0.553785.
        INFO optim.DistriOptimizer$: [Epoch 2 9984/15107][Iteration 197][Wall Clock 196.164318688s] Trained 128 records in 0.97483001 seconds. Throughput is 131.30495 records/second. Loss is 0.5517549.
        ```

-   在Analytics Zoo中使用pyspark和Jupyter来进行深度学习训练
    -   安装jupyter

        ```
        pip install jupyter
        ```

    -   使用以下命令启动：

        ```
        jupyter-with-zoo.sh
        ```

    -   使用Analytics Zoo,推荐采用内置的Wide And Deep 模型来进行，相关内容可参考[github](https://yq.aliyun.com/articles/alytics/analytics-zoo/tree/master/apps/recommendation-wide-n-deep)。
        1.  导入数据
        2.  定义模型和优化器
        3.  进行训练
        4.  查看训练结果

