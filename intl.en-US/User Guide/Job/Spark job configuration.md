# Spark job configuration {#concept_g1f_4hp_y2b .concept}

Spark job configuration

## Procedure {#section_p4m_rhp_y2b .section}

1.  Log on to the [Alibaba Cloud E-MapReduce Console](https://emr.console.aliyun.com/?spm=5176.8250060.103.1.48466f55SEaqMe#/cn-hangzhou) to enter the Cluster List page with primary account.
2.  Click the **Data Platform** tab on the top to enter the **Project List** page.
3.  Click **Design Workflow** of the specified project in the **Operation** column.
4.  On the left side of the Job Editing page, right-click on the folder you want to operate and select **New Job**.
5.  In the **New Job** dialog box, enter the job name, job description.

    Once the job type is selected, it cannot be modified.

6.  Click **OK**.

    **Note:** You can also create subfolder, rename folder, and delete folder by right-clicking on the folder.

7.  Select the Spark job type to create a Spark job. This type of job is submitted in the background by using the following process:

    ```
    spark-submit [options] --class [MainClass] xxx.jar args
    ```

8.  Enter the parameters in the **Content**box with command line parameters required to submit this Spark job. Only the parameters after spark-submit can be entered. The following example shows how to enter the parameters for creating Spark jobs and Pyspark jobs.
    -   Create a Spark job

        Create a Spark WordCount job:

        -   Job name: Wordcount
        -   Type: Select Spark
        -   Parameters:

            -   The command is as follows:

                ```
                spark-submit --master yarn-client --driver-memory 7G --executor-memory 5G --executor-cores 1 --num-executors 32 --class com.aliyun.emr.checklist.benchmark.SparkWordCount emr-checklist_2.10-0.1.0.jar oss://emr/checklist/data/wc oss://emr/checklist/data/wc-counts 32
                ```

            -   In the E-MapReduce job parameter box enter the following:

                ```
                --master yarn-client --driver-memory 7G --executor-memory 5G --executor-cores 1 --num-executors 32 --class com.aliyun.emr.checklist.benchmark.SparkWordCount ossref://emr/checklist/jars/emr-checklist_2.10-0.1.0.jar oss://emr/checklist/data/wc oss://emr/checklist/data/wc-counts 32
                ```

            **Note:** Job jar packages are saved in OSS. The way to reference this Jar package is: ossref://emr/checklist/jars/emr-checklist\_2.10-0.1.0.jar. Click **Select OSS path** to view and select from OSS, the system will automatically complete the absolute path of Spark script on OSS. Switch the default oss protocol to ossref protocol.

    -   Create a pyspark job

        Additionally to Scala and Java job types, E-MapReduce also supports Spark jobs of python type. Create a Spark Kmeans job for python script:

        -   Job name: Python-Kmeans
        -   Type: Spark
        -   Parameters:

            ```
            --master yarn-client --driver-memory 7g --num-executors 10 --executor-memory 5g --executor-cores 1 --jars ossref://emr/checklist/jars/emr-core-0.1.0.jar ossref://emr/checklist/python/wordcount.py oss://emr/checklist/data/kddb 5 32
            ```

        -   References of Python script resource are supported, and ossref protocol is used.
        -   For Pyspark, online Python installation kit is not supported.
9.  Click **Save** to complete the Spark job configuration.

