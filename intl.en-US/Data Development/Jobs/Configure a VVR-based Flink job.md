# Configure a VVR-based Flink job

E-MapReduce \(EMR\) V3.27.X and earlier versions use the open source version of Flink. Versions later than EMR V3.27.X use Ververica Runtime \(VVR\), an enterprise-grade computing engine. VVR is fully compatible with Flink. This topic describes how to configure a VVR-based Flink job.

-   A project is created. For more information, see [Manage projects](/intl.en-US/Data Development/Manage a workflow project.md).
-   Resources that are required for jobs and data files to be processed are obtained, such as JAR packages, data file names, and storage paths of the packages and files.

Enterprise-edition Flink is officially released by the founding team of Apache Flink and has a globally uniform brand.

VVR provides an enterprise-edition state backend whose performance is three to five times that of open source Flink. You can use the VVR engine and EMR data development feature to submit jobs in an EMR Hadoop cluster. VVR supports open source Flink 1.10 and provides business GeminiStateBackend by default, which brings the following benefits:

-   Uses a new data structure to increase the random query speed and reduce frequent disk I/O operations.
-   Optimizes the cache policy. If memory is sufficient, hot data is not stored in disks and cache entries do not expire after compaction.
-   Uses Java to implement GeminiStateBackend, which eliminates Java Native Interface \(JNI\) overheads that are caused by RocksDB.
-   Uses off-heap memory and implements an efficient memory allocator based on GeminiDB to eliminate the impact of garbage collection for Java Virtual Machines \(JVMs\).
-   Supports asynchronous incremental checkpoints. This ensures that only memory indexes are copied during data synchronization. Compared with RocksDB, GeminiStateBackend avoids jitters that are caused by I/O operations.
-   Supports local recovery and storage of the timer.

**Note:** If you want to use GeminiStateBackend, do not specify the type of a state backend in the code. To use GeminiStateBackend to start the Flink component, TaskManager must have 1,728 MiB of memory or more.

The basic configurations of the checkpoint and state backend in Flink also apply to GeminiStateBackend. For more information, see [Configuration.](https://ci.apache.org/projects/flink/flink-docs-release-1.10/ops/config.html#checkpoints-and-state-backends)

You can configure parameters based on your requirements. The following table describes some special parameters.

|Parameter|Description|
|---------|-----------|
|**state.backend.gemini.memory.managed**|Specifies whether to calculate the memory size of each backend based on the values of the Managed Memory and Task Slot parameters. Default value: true. Valid values: -   true
-   false |
|**state.backend.gemini.offheap.size**|Specifies the memory of each backend when the **state.backend.gemini.memory.managed** parameter is set to false. Default value: 2. Unit: GiB.|
|**state.backend.gemini.local.dir**|Specifies the directory that stores local data files of GeminiDB.|
|**state.backend.gemini.timer-service.factory**|Specifies the storage location of the timer-service state. Default value: HEAP. Valid values: -   HEAP
-   GEMINI |

**Note:** For more information about parameter settings, see [Configure parameters](/intl.en-US/Cluster Management/Third-party software/Configure parameters.md).

## Procedure

1.  In the top navigation bar, select the region where your cluster residesand select a resource group based on your business requirements.

2.  In the Create Job dialog box, specify **Name** and **Description**, and select **Flink** from the **Job Type** drop-down list.

3.  Click **OK**.

4.  Specify the command line parameters required to submit the job in the **Content** field.

    -   You can configure a Flink Datastream, table, or SQL job that is specified as a JAR package. Example:

        ```
        run -m yarn-cluster -yjm 1024 -ytm 2048 ossref://path/to/oss/of/WordCount.jar --input oss://path/to/oss/to/data --output oss://path/to/oss/to/result
        ```

    -   In EMR V3.28.2 and later EMR V3.X versions, you can configure a PyFlink job. Example:

        ```
        run -m yarn-cluster -yjm 1024 -ytm 2048 -py ossref://path/to/oss/of/word_count.py
        ```

        For more information about the parameters related to the PyFlink job, see [Apache Flink official documentation](https://ci.apache.org/projects/flink/flink-docs-release-1.10/ops/cli.html#usage).

5.  Click **Save**.

    You can use one of the following methods to access the web UI of Flink:

    -   Use the EMR console. For more information, see [Access open-source components](/intl.en-US/Cluster Management/Configure clusters/Access open-source components.md).

        **Note:** You cannot use the EMR console to access the web UI of Flink 1.8 or later.

    -   Use an SSH tunnel. For more information, see [Create an SSH tunnel to access web UIs of open source components](/intl.en-US/Cluster Management/Configure clusters/Connect to a cluster/Create an SSH tunnel to access web UIs of open source components.md).

