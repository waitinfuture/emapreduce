# Configure spark-submit parameters {#concept_ixr_3gh_hfb .concept}

This topic describes how to configure spark-submit parameters in E-MapReduce.

## Cluster configuration {#section_jy1_kgh_hfb .section}

-   Software configuration

    E-MapReduce V1.1.0

    -   Hadoop V2.6.0
    -   Spark V1.6.0
-   Hardware configuration
    -   Master node

-   8-core, 16 GB memory, and 500 GB storage space \(ultra disk\)

-   1 set

    -   Worker node
        -   8-core, 16 GB memory, and 500 GB storage space \(ultra disk\)
        -   10 sets
    -   Total: 8-core 16 GB \(Worker\) × 10 + 8-core 16 GB \(Master\)

        **Note:** Only CPU and memory resources are calculated when a job is submitted. Therefore, the disk size is not included in total resource calculation.

    -   Total resources available for YARN: 12-core 12.8 GB \(worker\) × 10

        **Note:** By default, cores available for YARN = number of cores × 1.5, and memory available for YARN = node memory × 0.8.


## Submit a job {#section_oxp_vgh_hfb .section}

After you create a cluster, you can submit jobs. First, you need to create a job in E-MapReduce. The following figure shows the job parameters.

![Job configuration](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17980/156740518913107_en-US.png)

The job in the preceding figure uses the official Spark example package. Therefore, you do not need to upload your own JAR package.

The parameters are listed as follows:

``` {#codeblock_2j9_pc1_p1s}
--class org.apache.spark.examples.SparkPi --master yarn --deploy-mode client --driver-memory 4g --num-executors 2 --executor-memory 2g --executor-cores 2 /opt/apps/spark-1.6.0-bin-hadoop2.6/lib/spark-examples*.jar 10
```

The parameters are described as follows:

|Parameter|Example|Description|
|---------|-------|-----------|
|class|org.apache.spark.examples.SparkPi|The main class of the job.|
|master|yarn|E-MapReduce uses the YARN mode. Set the value to yarn.|
|yarn-client|Equivalent to setting the master parameter to yarn and the deploy-mode parameter to client. If you have set this parameter, then you do not need to set the deploy-mode parameter.|
|yarn-cluster|Equivalent to setting the master parameter to yarn and the deploy-mode parameter to cluster. If you have set this parameter, then you do not need to set the deploy-mode parameter.|
|deploy-mode|client|The client mode indicates that the ApplicationMaster \(AM\) of the job runs on the master node. If you set this parameter, you must also set the master parameter to yarn.|
|cluster|The cluster mode indicates that the AM runs randomly on one of the worker nodes. If you set this parameter, you must also set the master parameter to yarn.|
|driver-memory|4g|The memory to be allocated to the driver. The allocated memory must not be greater than total memory size per node.|
|num-executors|2|The number of executors to be created.|
|executor-memory|2g|The maximum amount of memory to be allocated to each executor. The allocated memory cannot be greater than the maximum available memory per node.|
|executor-cores|2|The number of threads used by each executor, which equals the maximum number of tasks that can be executed concurrently by each executor.|

## Resource calculation {#section_x4w_ghh_hfb .section}

The resources consumed by jobs running in different modes and settings are shown in the following table:

-   Resource calculation for the yarn-client mode

    |Node|Resource type|Resource amount \(the result is calculated based on the preceding examples\)|
    |----|-------------|----------------------------------------------------------------------------|
    |Master|Core|1 core|
    |Memory|driver-memory = 4 GB|
    |Worker|Core|num-executors × executor-cores = 4 cores|
    |Memory|num-executors × executor-memory = 4 GB|

    -   The main program of the job \(the driver program\) runs on the master node. As specified by the --driver-memory parameter, 4 GB memory is allocated to the main program based on the job settings. The main program may not use all of the allocated memory.
    -   As specified by the --num-executors parameter, two executors are initiated on work nodes. Each executor is allocated with 2 GB memory \(specified by the --executor-memory parameter\) and supports a maximum of 2 concurrent tasks \(specified by the --executor-cores parameter\).
-   Resource calculation for the yarn-cluster mode

    |Node|Resource type|Resource amount \(the result is calculated based on the preceding examples\)|
    |----|-------------|----------------------------------------------------------------------------|
    |Master|N/A|A tiny client program, which is responsible for synchronizing job information and consumes only a small amount of resources.|
    |Worker|Core|num-executors × executor-cores + spark.driver.cores = 5 cores|
    |Memory|num-executors × executor-memory + driver-memory = 8 GB|

    **Note:** The default value of spark.driver.cores is 1. You can set it to a value greater than 1.


## Resource usage optimization {#section_ytg_4hh_hfb .section}

-   yarn-client mode

    If you have a large job running in the yarn-client mode and want to use more resources of the cluster, see the following configurations:

    ``` {#codeblock_rbc_wug_sdx}
    --master yarn-client --driver-memory 5g --num-executors 20 --executor-memory 4g --executor-cores 4
    ```

    **Note:** 

    -   Spark will allocate 375 MB or 7% \(whichever is higher\) memory in addition to the memory value that you have set.
    -   When allocating memory to containers, YARN rounds up to the nearest integer gigabyte. The memory value here must be a multiple of 1 GB.
    Based on the preceding resource formula:

    -   The resource amount for the master is as follows:

        -   Cores: 1
        -   Memory: 6 GB \(5 GB + 375 MB, which is rounded up to 6 GB\)
    -   The resource amount for the workers is as follows:

        -   Core: 20 × 4 = 80 cores
        -   Memory: 20 × 5 GB \(4 GB + 375 MB, which is rounded up to 5 GB\) = 100 GB
    According to the resource calculation results, the amount of resources allocated to the job has not exceeded the total amount of the resources of the cluster. By following this rule, you can use other resource allocation configurations, such as:

    ``` {#codeblock_za8_mxi_sqt}
    --master yarn-client --driver-memory 5g --num-executors 40 --executor-memory 1g --executor-cores 2
    ```

    ``` {#codeblock_kgy_h37_nki}
    --master yarn-client --driver-memory 5g --num-executors 15 --executor-memory 4g --executor-cores 4
    ```

    ``` {#codeblock_nf9_w2t_g9y}
    --master yarn-client --driver-memory 5g --num-executors 10 --executor-memory 9g --executor-cores 6
    ```

    Theoretically, you only need to make sure that the total amount of resources calculated by using the preceding formula does not exceed the total amount of the resources of the cluster. However, in production scenarios, the operating system, HDFS file systems, and E-MapReduce services may also need to use core and memory resources. If no core and memory resources are available for them, then the job performance declines or the job fails.

    Typically, the executor-cores parameter is set to the same value as the number of cluster cores. If the value is too large, the CPU switches frequently without benefiting the performance as expected.

-   yarn-cluster mode

    In the yarn-cluster mode, the driver program runs on worker nodes. Resources in the resource pool of the worker nodes are used. If you want to use more resources of the cluster, use the following configuration:

    ``` {#codeblock_tje_cf9_hcq}
    --master yarn-cluster --driver-memory 5g --num-executors 15 --executor-memory 4g --executor-cores 4
    ```


## Recommended configuration {#section_c42_xhh_hfb .section}

-   If you set the memory to a very large value, you should pay close attention to the overhead caused by garbage collection. Typically, we recommend that you assign memory less than or equal to 64 GB to an executor.

-   If you are executing an HDFS read/write job, we recommend that you set the number of concurrent jobs for each executor to a value smaller than or equal to 5 for reading and writing data.

-   If you are executing an OSS read/write job, we recommend that you distribute executors to different ECS instances so that the bandwidth of every ECS instance can be used. For example, if you have 10 ECS instances, you can set num-executors to 10, and set the appropriate memory and number of concurrent jobs.

-   If the code that you use in the job is not thread-safe, you need to monitor whether the concurrency causes job errors when you set the executor-cores parameter. If yes, we recommend that you set executor-cores to 1.


