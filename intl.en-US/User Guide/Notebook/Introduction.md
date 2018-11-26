# Introduction {#concept_eb1_qlj_z2b .concept}

Notebook allows you to compile and run Spark, Spark SQL, and Hive SQL tasks directly on the E-MapReduce console. You can view the running results directly on Notebook. Notebook is ideal for processing debugging tasks that require shorter runtime and whose data results need to be viewed directly. For tasks with longer runtime and requiring regular execution, the job and execution plan function must be used. This section describes how to create and run a demo task. For other examples and operation instructions, refer to the subsequent chapters.

## Create a demo task {#section_snn_2nj_z2b .section}

1.  Log on to the [Alibaba Cloud E-MapReduce console](https://emr.console.aliyun.com/).
2.  At the top of the navigation bar, click **Old EMR Scheduling**.
3.  In the left-side navigation bar, click **Notebook**.
4.  Click **New notebook demo**.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17927/154226454510996_en-US.jpg)

5.  A confirmation box is displayed, indicating the required cluster environment. Click **OK** to create demo tasks. Three examples of interactive tasks will be created.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17927/154226454510998_en-US.jpg)


## Run a Spark demo task {#section_qv4_qrj_z2b .section}

1.  Click **EMR-Spark-Demo** to display the example of a Spark notebook. Before running the notebook, you need to associate the task to a created cluster. Select a created cluster in the list of available clusters. Note that the associated cluster must be EMR-2.3 or later and has no less than three nodes, each with at least 4 cores and 8 GB of memory.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17927/154226454511000_en-US.jpg)

2.  After a cluster is associated, click **Run**. When the associated cluster executes the Spark or Spark SQL notebook for the first time, it takes about one minute to build the Spark context and running environment. It does not need to be built in subsequent executions. The running result is displayed below the Run button.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17927/154226454511001_en-US.jpg)


## Run a SparkSQL demo task {#section_us2_ssj_z2b .section}

1.  Click **EMR-Spark-Demo** to display the SparkSQL notebook example. Before running the notebook, you need to associate the notebook to a created cluster. Click the upper right corner and select a created cluster in the list of available clusters.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17927/154226454511003_en-US.jpg)

2.  The SparkSQL demo contains several demo sections which can be run individually or all together by clicking **Run All**. After the running, you can see all returned data results of each section.

    **Note:** If the section for creating a table is run multiple times, an error will be reported indicating that the table already exists.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17927/154226454611004_en-US.jpg)


## Run a Hive demo task {#section_bqy_dtj_z2b .section}

1.  Click **EMR-Hive-Demo** to display the Hive notebook example. Before running the notebook, you need to associate the notebook to a created cluster. Click the upper right corner and select a created cluster in the list of available clusters.
2.  The Hive demo task contains several demo sections, which can be run individually, or all together by clicking **Run All**. After running, you can see all returned data results of each section.

    **Note:** 

    -   When the associated cluster executes the Hive notebook for the first time, it takes a few seconds to build the Hive client running environment. It will no longer need to be built in subsequent execution.
    -   If the section for creating a table is run multiple times, an error will be reported indicating that the table already exists.
    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17927/154226454611005_en-US.jpg)


## Cancel the association with clusters {#section_mdy_c5j_z2b .section}

After a notebook is run in a cluster, the cluster creates a process for catching some context running environments in order to ensure quick response upon re-execution. If you do not need to execute other notebooks, and you want to release the cluster resources occupied by caching, you can disassociate all interactive tasks that have been run from the associated clusters. In this way, you can release the memory resources occupied on the original associated clusters.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17927/154226454611006_en-US.jpg)

