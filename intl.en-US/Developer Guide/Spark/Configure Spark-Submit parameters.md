# Configure Spark-Submit parameters {#concept_ixr_3gh_hfb .concept}

This topic describes how to configure spark-submit parameters in E-MapReduce.

## Cluster configuration {#section_jy1_kgh_hfb .section}

-   Software configuration

    E-MapReduce version 1.1.0

    -   Hadoop 2.6.0
    -   Spark 1.6.0
-   Hardware configuration
    -   Master node

-   8-core, 16 GB RAM, 500 GB Ultra Disk

-   1 unit

    -   Worker node \(x 10\)
        -   8-core, 16 GB RAM, 500 GB Ultra Disk
        -   10 units
    -   Total: 8-core 16GB \(Worker\) x 10 + 8-core 16GB \(Master\)

        **Note:** Note: Only CPU and memory resources are calculated when a job is submitted. Therefore, the disk size is not included in the total resources.

    -   Yarn total available resources: 12-core, 12.8 GB \(worker\) x 10

        **Note:** By default, cores available for Yarn = number of cores x 1.5, memory available for Yarn = machine memory x 0.8.


## Submit a job {#section_oxp_vgh_hfb .section}

After a cluster has been created, you can submit a job. First, you need to create a job in E-MapReduce with the following configuration:

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17980/154752320113107_en-US.png)

The job in the preceding figure directly uses the official Spark example package, so you do not need to upload your own JAR package.

The parameters are listed as follows:

```
--class org.apache.spark.examples.SparkPi --master yarn --deploy-mode client --driver-memory 4g --num-executors 2 --executor-memory 2g --executor-cores 2 /opt/apps/spark-1.6.0-bin-hadoop2.6/lib/spark-examples*.jar 10
```

The details of the parameters are described as follows:

|Parameter|Reference value|Description|
|---------|---------------|-----------|
|class|org.apache.spark.examples.SparkPi|The main class of the job.|
|master|yarn|E-MapReduce uses the Yarn mode. Therefore, the mode must be in the Yarn mode.|
| |yarn-client|Equivalent to setting the master parameter to yarn and the deploy-mode parameter to client. You do not need to set the deploy-mode parameter separately here.|
| |yarn-cluster|Equivalent to setting the master parameter to yarn and the deploy-mode parameter to cluster. You do not need to set the deploy-mode parameter separately here.|
|deploy-mode|client|The client mode indicates that the AM runs on the master node. You must set the preceding master parameter to yarn at the same time when setting this parameter.|
| |cluster|The cluster mode indicates that the AM runs randomly on one of the worker nodes. However, if you set this parameter, you must also set the preceding master parameter to yarn.|
|driver-memory|4g|The memory used by the driver cannot exceed the total number of cores of a single machine.|
|num-executors|2|The number of executors to be created.|
|executor-memory|2g|The maximum memory used by each executor cannot exceed the maximum available memory of a single machine.|
|executor-cores|2|The number of threads used by each executor, which is also the maximum number of tasks that can be executed concurrently by each executor.|

## Resource calculations {#section_x4w_ghh_hfb .section}

The resources used by jobs running in different modes and settings are shown in the following table:

-   **Resource calculation for the yarn-client mode**

    |Node|Resource type|Resource amount \(the result is calculated based on the preceding examples\)|
    |----|-------------|----------------------------------------------------------------------------|
    |master|core|1|
    | |mem|driver-memory = 4 GB|
    |worker|core|num-executors \* executor-cores = 4|
    | |mem|num-executors \* executor-memory = 4 GB|

    -   The primary program of the job \(the driver program\) runs on the master node. 4 GB memory \(specified by --driver-memory\) is allocated to the primary program based on the job settings \(the memory may not be fully used\).
    -   Two executors \(specified by --num-executors\) are initiated on the worker node, with each executor allocated with 2 GB \(specified by --executor-memory\) memory, and each executor supports a maximum of 2 \(specified by - -executor-cores\) concurrent tasks.
-   Resource calculation for the yarn-cluster mode

    |Node|Resource type|Resource amount \(the result is calculated using the preceding examples\)|
    |----|-------------|-------------------------------------------------------------------------|
    |master| |A tiny client program, which is responsible for synchronizing job information and consumes significantly less space.|
    |worker|core|num-executors \* executor-cores + spark.driver.cores = 5|
    | |mem|num-executors \* executor-memory + driver-memory = 8g|

    **Note:** By default, the value of spark.driver.cores is 1. You can set it to a value greater than 1.


## Resource optimization {#section_ytg_4hh_hfb .section}

-   yarn-client mode

    If you have a large job in the yarn-client mode and want to use more resources of the cluster, see the following configurations:

    ```
    --master yarn-client --driver-memory 5g –-num-executors 20 --executor-memory 4g --executor-cores 4
    ```

    **Note:** 

    -   When allocating memory, Spark allocates 375 MB or 7% \(whichever is higher\) extra memory in addition to the memory value you have set.
    -   When allocating memory to containers, Yarn rounds up to the nearest integer gigabyte. Here the memory must be the integral multiple of 1 GB.
    Based on the preceding resource formula:

    -   The resource amount for the master is:

        -   cores: 1
        -   mem: 6 GB \(5 GB + 375 MB, rounded up to 6 GB\)
    -   The resource amount for the workers is:

        -   core: 20\*4 = 80
        -   mem: 20\*5 GB \(4 GB + 375 MB, rounded up to 5 GB\) = 100 GB
    We can see that the total resources allocated to the job have not exceeded the total resources for the cluster. Following this rule, you have multiple available configurations, such as:

    ```
    --master yarn-client --driver-memory 5g --num-executors 40 --executor-memory 1g --executor-cores 2
    ```

    ```
    --master yarn-client --driver-memory 5g --num-executors 15 --executor-memory 4g --executor-cores 4
    ```

    ```
    --master yarn-client --driver-memory 5g --num-executors 10 --executor-memory 9g --executor-cores 6
    ```

    In principle, you only have to make sure that the total resources calculated using the preceding formula do not exceed the total resources of the cluster. However, in production scenarios, the services of the system, HDFS, and E-MapReduce occupy certain core resources and memory resources. Therefore, if you use up the core resources and memory resources, the job performance declines or the job fails to run.

    Usually, the number of executor-cores is set to the same value as the core number of the cluster. If you set the value too large, the CPU switches frequently without benefiting the performance as expected.

-   yarn-cluster mode

    In the yarn-cluster mode, the driver program runs on worker nodes. Resources in the resource pool of worker nodes are used. If you want to use more resources of this cluster, use the following configurations:

    ```
    --master yarn-cluster --driver-memory 5g --num-executors 15 --executor-memory 4g --executor-cores 4
    ```


## Recommendations on configuration {#section_c42_xhh_hfb .section}

-   If you set the memory to a very large value, you should pay attention to the overhead caused by garbage collection. In general, we recommend that you set an executor with memory less than or equal to 64GB.

-   If you are executing an HDFS read/write job, we recommend that you set the number of concurrent jobs for each executor to be less than or equal to 5 for reading and writing.

-   If you are executing an OSS read/write job, we recommend that you distribute executors to different ECS instances so that the bandwidth of every ECS instance can be used. For example, if you have 10 ECS instances, you can set num-executors to 10, and set the appropriate memory and number of concurrent jobs.

-   If the code you use in the job is not thread-safe, you need to monitor whether the concurrency causes job errors when setting the executor-cores. If yes, we recommend that you set executor-cores to 1.


