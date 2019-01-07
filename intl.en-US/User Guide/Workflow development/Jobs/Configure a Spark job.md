# Configure a Spark job {#concept_g1f_4hp_y2b .concept}

In this tutorial, you will learn how to configure a Spark job.

## Procedure {#section_p4m_rhp_y2b .section}

1.  Log on to the [Alibaba Cloud E-MapReduce console](https://emr.console.aliyun.com/?spm=5176.8250060.103.1.48466f55SEaqMe#/cn-hangzhou).
2.  At the top of the navigation bar, click **Data Platform**.
3.  In the **Actions** column, click **Design Workflow** next to the specified project.
4.  On the left of the Job Editing page, right-click the folder you want to operate and select **New Job**.
5.  In the **New Job** dialog box, enter the job name and description.
6.  Click **OK**.

    **Note:** You can also create subfolders, rename folders, and delete folders by right-clicking on them.

7.  Select the Spark job type to create a Spark job. This type of job is submitted in the background using the following method.

    ```
    spark-submit [options] --class [MainClass] xxx.jar args
    ```

8.  Enter the parameters in the **Content** field that are required to submit this job. Only the parameters after spark-submit can be entered. The following example shows how to enter the parameters for creating a Spark job and a PySpark job.
    -   Create a Spark job

        Create a Spark WordCount job:

        -   Job name: WordCount
        -   Type: Select Spark
        -   Parameters:

            -   Enter the following command:

                ```
                spark-submit --master yarn-client --driver-memory 7G --executor-memory 5G --executor-cores 1 --num-executors 32 --class com.aliyun.emr.checklist.benchmark.SparkWordCount emr-checklist_2.10-0.1.0.jar oss://emr/checklist/data/wc oss://emr/checklist/data/wc-counts 32
                ```

            -   Enter the following in the E-MapReduce job **Content** field:

                ```
                --master yarn-client --driver-memory 7G --executor-memory 5G --executor-cores 1 --num-executors 32 --class com.aliyun.emr.checklist.benchmark.SparkWordCount ossref://emr/checklist/jars/emr-checklist_2.10-0.1.0.jar oss://emr/checklist/data/wc oss://emr/checklist/data/wc-counts 32
                ```

            **Note:** Job jar packages are saved in OSS. In the example above, the way to reference the Jar package is ossref://emr/checklist/jars/emr-checklist\_2.10-0.1.0.jar. Click **Select OSS path** to view and select one from OSS. The system will automatically complete the absolute path of the Spark script on OSS. Switch the default OSS protocol to the ossref protocol.

    -   Create a PySpark job

        In addition to Scala and Java job types, E-MapReduce also supports Python job types in Spark. Create a Spark K-means job for the Python script:

        -   Job name: Python-Kmeans
        -   Type: Spark
        -   Parameters:

            ```
            --master yarn-client --driver-memory 7g --num-executors 10 --executor-memory 5g --executor-cores 1 --jars ossref://emr/checklist/jars/emr-core-0.1.0.jar ossref://emr/checklist/python/wordcount.py oss://emr/checklist/data/kddb 5 32
            ```

        -   References of Python script resources are supported, and the ossref protocol is used.
        -   For PySpark, the online Python installation kit is not supported.
9.  Click **Save** to complete the Spark job configuration.

