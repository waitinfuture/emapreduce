# Deep learning with Analytics Zoo on E-MapReduce {#concept_awn_wmp_kfb .concept}

Analytics Zoo is an analytics and AI platform that unites Apache Spark and Intel BigDL into an integrated pipeline. It helps users develop deep learning applications based on big data and end-to-end pipelines. This topic describes how to use Analytics Zoo to develop deep learning applications on Alibaba Cloud E-MapReduce.

## Introduction {#section_nfv_bnp_kfb .section}

Analytics Zoo is an analytics and AI platform that unites Apache Spark and Intel BigDL into an integrated pipeline. It helps users develop deep learning applications based on big data and end-to-end pipelines.

## System requirements {#section_mkn_2np_kfb .section}

-   JDK 8
-   Spark cluster \(Spark 2.x supported by EMR is recommended\)
-   Python 2.7\(also Python 3.5 or Python 3.6\), pip

## Installation of Analytics Zoo {#section_iqp_hnp_kfb .section}

-   The latest release of Analytics Zoo is 0.2.0.
-   Installation for Scala users
    -   Download the pre-build version.

        You can download the [Pre-build version](https://analytics-zoo.github.io/master/#release-download/) from the Analytics Zoo page on GitHub.

    -   Build Analytics Zoo using the make-dist.sh script.

        Install Apache Maven and set the environment variable MAVEN\_OPTS as follows:

        ```
        export MAVEN_OPTS="-Xmx2g -XX:ReservedCodeCacheSize=512m"
        ```

        If you use ECS instances to compile code, we recommend that you modify the mirror of the Maven repository.

        ```
        <mirror>
            <id>nexus-aliyun</id>
            <mirrorOf>central</mirrorOf>
            <name>Nexus aliyun</name>
            <url>http://maven.aliyun.com/nexus/content/groups/public</url>
        </mirror>
        ```

        Download an [Analytics Zoo release](https://github.com/intel-analytics/analytics-zoo). Extract the file, move to the corresponding directory, and run the following command:

        ```
        bash make-dist.sh
        ```

        After building Analytics Zoo, you can find a dist directory, which contains all the needed files to run an Analytics Zoo program. Use the following command to copy the files in the dist directory to the directory of the EMR software stack:

        ```
        cp -r dist/ /usr/lib/analytics_zoo
        ```

-   Installation for Python users

    Analytics Zoo can be installed either with pip or without pip. When you install Analytics Zoo with pip, PySpark and BigDL are installed. This may cause a software conflict because PySpark has already been installed on the EMR cluster. To avoid such conflicts, install Analytics Zoo without pip.

    -   Installation without pip

        First, you need to run the following command:

        ```
        bash make-dist.sh
        ```

        Change to the pyzoo directory and install Analytics Zoo:

        ```
        python setup.py install
        ```

-   Setting environment variables

    After building Analytics Zoo, copy the dist directory to the directory of the EMR software stack and set the environment variable. Add the following lines to the /etc/profile.d/analytics\_zoo.sh file.

    ```
    export ANALYTICS_ZOO_HOME=/usr/lib/analytics_zoo
    export PATH=$ANALYTICS_ZOO_HOME/bin:$PATH
    ```

    You do not need to set SPARK\_HOME because it has already been set on EMR.


## Using Analytics Zoo {#section_wlv_nqp_kfb .section}

-   Use Spark to train and test deep learning models.
    -   Use Analytics Zoo to do text classification. You can find the code and description on [GitHub](https://github.com/intel-analytics/analytics-zoo/tree/master/zoo/src/main/scala/com/intel/analytics/zoo/examples/textclassification). Download the required data as required. Submit the following commands:

```
spark-submit --master yarn \
--deploy-mode cluster --driver-memory 8g \
--executor-memory 20g --class com.intel.analytics.zoo.examples.textclassification.TextClassification \
/usr/lib/analytics_zoo/lib/analytics-zoo-bigdl_0.6.0-spark_2.1.0-0.2.0-jar-with-dependencies.jar --baseDir /news
```

    -   You can log on to the instance of the Spark cluster through [ssh proxy](../DNemapreduce1876943/EN-US_TP_17923.dita#concept_sns_sww_y2b) to view the status of the jobs.

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/23346/154210009413534_en-US.png)

        You can also view the accuracy of each epoch through logs.

        ```
        INFO optim.DistriOptimizer$: [Epoch 2 9600/15107][Iteration 194][Wall Clock 193.266637037s] Trained 128 records in 0.958591653 seconds. Throughput is 133.52922 records/second. Loss is 0.74216986.
        INFO optim.DistriOptimizer$: [Epoch 2 9728/15107][Iteration 195][Wall Clock 194.224064816s] Trained 128 records in 0.957427779 seconds. Throughput is 133.69154 records/second. Loss is 0.51025534.
        INFO optim.DistriOptimizer$: [Epoch 2 9856/15107][Iteration 196][Wall Clock 195.189488678s] Trained 128 records in 0.965423862 seconds. Throughput is 132.58424 records/second. Loss is 0.553785.
        INFO optim.DistriOptimizer$: [Epoch 2 9984/15107][Iteration 197][Wall Clock 196.164318688s] Trained 128 records in 0.97483001 seconds. Throughput is 131.30495 records/second. Loss is 0.5517549.
        ```

-   Use PySpark and Jupyter to train deep learning models on Analytics Zoo.
    -   Install Jupyter.

        ```
        pip install jupyter
        ```

    -   Run the following command to start Jupyter.

        ```
        jupyter-with-zoo.sh
        ```

    -   We recommend that you use the pre-defined Wide And Deep Learning models provided by Analytics Zoo.
        1.  Import data.

            ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/23346/154210009413535_en-US.png)

        2.  Build a model and create an optimizer.

            ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/23346/154210009413536_en-US.png)

        3.  Start the training process.

            ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/23346/154210009513537_en-US.png)

        4.  View training results.

            ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/23346/154210009513538_en-US.png)

            ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/23346/154210009513539_en-US.png)


