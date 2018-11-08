# Druid common problems {#concept_bpm_5wd_z2b .concept}

This section describes Druid common problems you may meet

## Analyze the indexing failure {#section_gtn_wwd_z2b .section}

When indexing fails, the following troubleshooting steps are typically followed:

-   **For batch data index**
    1.  If curl returns an error directly, or no value returns, check the input file format. Or add a -v parameter to curl to observe the value returned from the REST API.
    2.  Observe the execution of the jobs on the Overlord page. If it fails, view the logs on the page.
    3.  In many cases, logs are not generated. If it is a Hadoop job, enter the Yarn page to see if there is an indexing job generated, and view the job execution log.
    4.  If no errors are found, you need to log on to the Druid cluster, and view the execution logs of Overlord \(at /mnt/disk1/log/druid/overlord—emr-header-1.cluster-xxxx.log\). If it is an HA cluster, check the Overlord that you submitted the job to.
    5.  If the job has been submitted to Middlemanager, but a failure is returned, you need to view the worker that the job is submitted to in Overlord, and log on to the worker node to view the Middlemanager logs in /mnt/disk1/log/druid/middleManager-emr-header-1.cluster-xxxx.log.
-   For real-time Tranquility index

    Check the Tranquility logs to see if the message is received or dropped.

    The remaining troubleshooting steps are the same as 2~5 of batch data index.

    Most of the errors are cluster configuration issues and job problems. Cluster configuration errors are about memory parameters, cross-cluster connection, access to clusters in high-security mode, and principals. Job errors are about the format of the job description files, input data parsing, and other job-related configuration issues \(such as ioConfig\).


## Obtain the FAQ list { .section}

-   Service startup fails.

    Most of these problems are due to the configuration problems with the component JVM running parameters. For example, the machine may not have a large memory, but it is configured with a larger JVM memory or a larger number of threads.

    Solution: View the component logs and adjust the relevant parameters to resolve these issues. JVM memory involves heap memory and direct memory. For more information, see [http://druid.io/docs/latest/operations/performance-faq.html](http://druid.io/docs/latest/operations/performance-faq.html).

-   The Yarn task fails during the indexing process, and shows Jar package conflict error like this: `Error: class com.fasterxml.jackson.datatype.guava.deser.HostAndPortDeserializer overrides final method deserialize.( Lcom/fasterxml/jackson/core/JsonParser;Lcom/fasterxml/jackson/databind/DeserializationContext;)Ljava/lang/Object;`.

    Solution: Add the following content to the job configuration file of indexing:

    ```
    "tuningConfig" : {
         ...
         "jobProperties" : {
             "mapreduce.job.classloader": "true"
             or
             "mapreduce.job.user.classpath.first": "true"
         }
         ...
     }
    ```

    The parameter mapreduce.job.classloader allows MapReduce job to use a standalone classloader, and the parameter mapreduce.job.user.classpath.first gives MapReduce the priority to use your jar packages. You can select one of these two configuration items. For details about this, see [http://druid.io/docs/0.9.2-rc1/operations/other-hadoop.html](http://druid.io/docs/0.9.2-rc1/operations/other-hadoop.html).

-   The logs of the index task report that the reduce task cannot create the segments directory.

    Solution:

    -   Check the settings for deep storage, including type and directory. When the type is local, pay attention to the permission settings of directory. When the type is HDFS, directory should be written as the full HDFS path, such as hdfs://:9000/. For hdfs\_master, IP is preferred. If you want to use a domain name, then use the full domain name, such as emr-header-1.cluster-xxxxxxxx rather than emr-header-1.
    -   When you use Hadroop for batch index, you must set the deep storage of segments as “hdfs”. The “local” type may cause the MR job to be in an unidentified state, because the remote Yarn cluster cannot create the segments directory in the reduce task.\( This is for standalone Druid clusters.\)
-   Failed to create directory within 10000 attempts…

    This issue occurs typically because the path set by java.io.tmp in the JVM configuration file doesn’t exist. Set the path and make sure that the Druid account has the permission to access it.

-   com.twitter.finagle.NoBrokersAvailableException: No hosts are available for disco! firehose:druid:overlord

    This issue is typically due to ZooKeeper connection issues. Make sure that Druid and Tranquility have the same connection string for ZooKeeper. Because the default ZooKeeper path for Druid is /druid, make sure that zookeeper.connect in Tranquility settings includes /druid. \( Two ZooKeeper settings exist in the Tranquility Kafka settings. One is zookeeper.connect used to connect the ZooKeeper of the Druid cluster, and the other is kafka.zookeeper.connect used to connect the ZooKeeper of the Kafka cluster. These two ZooKeepers may not belong to the same ZooKeeper cluster\).

-   The MiddleManager reports that the com.hadoop.compression.lzo.LzoCodec class cannot be found during the indexing process.

    This is because the Hadoop cluster of EMR is configured with lzo compression.

    Solution: Copy the jar package and the native file under the directory of EMR HADOOP\_HOME/lib to druid.extensions.hadoopDependenciesDir \(DRUID\_HOME/hadoop-dependencies by default\) of Druid.

-   The following error is reported during the indexing process:

    ```
    2018-02-01T09:00:32,647 ERROR [task-runner-0-priority-0] com.hadoop.compression.lzo.GPLNativeCodeLoader - could not unpack the binaries
      java.io.IOException: No such file or directory
              at java.io.UnixFileSystem.createFileExclusively(Native Method) ~[?:1.8.0_151]
              at java.io.File.createTempFile(File.java:2024) ~[?:1.8.0_151]
              at java.io.File.createTempFile(File.java:2070) ~[?:1.8.0_151]
              at com.hadoop.compression.lzo.GPLNativeCodeLoader.unpackBinaries(GPLNativeCodeLoader.java:115) [hadoop-lzo-0.4.21-SNAPSHOT.jar:?]
    ```

    This issue occurs because the java.io.tmp path doesn’t exist. Set the path and make sure that the Druid account has the permission to access it.


