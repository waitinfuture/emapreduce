# Common Druid problems {#concept_bpm_5wd_z2b .concept}

This section describes some of the common problems you may encounter with Druid.

## Analyze the indexing failure {#section_gtn_wwd_z2b .section}

If indexing fails, complete the following steps to troubleshoot the failure:

-   **For batch data indexing**
    1.  If the curl command output displays an errort or does not display any information, check the file format. Alternatively, add the -v parameter to the curl command to check the value returned from the RESTful API.
    2.  Observe the execution of jobs on the Overlord page. If the execution fails, view the logs on that page.
    3.  In many cases, logs are not generated. In the case of a Hadoop job, open the YARN page to check whether an index job has been generated.
    4.  If no errors are found, you need to log on to the Druid cluster and view the execution logs of Overlord at /mnt/disk1/log/druid/overlord—emr-header-1.cluster-xxxx.log. In the case of an HA cluster, check the Overlord that you submitted the job to.
    5.  If the job has been submitted to Middlemanager but a failure is returned from Middlemanager, you need to view the worker that the job is submitted to in Overlord, and log on to the worker node to view the Middlemanager logs \(at /mnt/disk1/log/druid/middleManager-emr-header-1.cluster-xxxx.log\).
-   **For real-time Tranquility indexing**

    Check the Tranquility log to see if the message was received or dropped.

    The remaining troubleshooting steps are the same as steps 2 to 5 of batch indexing.

    Most errors are about cluster configurations and jobs. Cluster configuration errors are about memory parameters, cross-cluster connection, access to clusters in high-security mode, and principals. Job errors are about the format of the job description files, input data parsing, and other job-related configuration issues \(such as ioConfig\).


## Obtain the FAQ list { .section}

-   Service startup fails.

    Most of these problems are due to configuration problems with the running parameters of the JVM component. For example, the machine may not have a large memory, but it is configured with a larger JVM memory or a larger number of threads.

    To resolve this issue, view the component logs and adjust the relevant parameters. JVM memory involves heap memory and direct memory. For more information, go to [Druid Performance FAQ](http://druid.io/docs/latest/operations/performance-faq.html).

-   The YARN task fails during indexing and shows a JAR package conflict error like this: `Error: class com.fasterxml.jackson.datatype.guava.deser.HostAndPortDeserializer overrides final method deserialize.( Lcom/fasterxml/jackson/core/JsonParser;Lcom/fasterxml/jackson/databind/DeserializationContext;)Ljava/lang/Object;`.

    To resolve this issue, add the following content to the indexing job configuration file:

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

    The parameter mapreduce.job.classloader allows MapReduce jobs to use standalone classloaders, and the parameter mapreduce.job.user.classpath.first gives MapReduce the priority to use your JAR packages. You can select one of these two configuration items. For more information, go to [Druid documents](http://druid.io/docs/0.9.2-rc1/operations/other-hadoop.html).

-   The logs of the index task report that the reduce task cannot create the segments directory.

    To resolve this issue, complete the following:

    -   Check the settings for deep storage, including type and directory. If the type is local, pay attention to the permission settings of the directory. If the type is HDFS, the directory should be written as the full HDFS path, such as hdfs://:9000/. For hdfs\_master, IP is recommended. If you want to use a domain name, use the full domain name, such as emr-header-1.cluster-xxxxxxxx rather than emr-header-1.
    -   If you are using Hadoop for batch indexing, you must set the deep storage of segments as "hdfs". The local type may cause the MapReduce job to be in an unidentified state, because the remote YARN cluster cannot create the segments directory in the reduce task. \(This is only applicable to standalone Druid clusters.\)
-   Failed to create directory within 10,000 attempts.

    This issue occurs typically because the path set by java.io.tmp in the JVM configuration file does not exist. Set the path and make sure that the Druid account has permission to access it.

-   com.twitter.finagle.NoBrokersAvailableException: No hosts are available for disco!firehose:druid:overlord

    This issue is typically due to ZooKeeper connection issues. Make sure that Druid and Tranquility have the same connection string for ZooKeeper. Because the default ZooKeeper path for Druid is /druid, make sure that zookeeper.connect in the Tranquility settings includes /druid. \(Two ZooKeeper settings exist in Tranquility Kafka. One is zookeeper.connect used to connect the ZooKeeper of the Druid cluster, and the other is kafka.zookeeper.connect used to connect the ZooKeeper of the Kafka cluster. These two ZooKeepers may not belong to the same ZooKeeper cluster.\)

-   The MiddleManager reports that the com.hadoop.compression.lzo.LzoCodec class cannot be found during indexing.

    This is because the Hadoop cluster of E-MapReduce is configured with lzo compression.

    To resolve this issues, copy the JAR package and the native file under the EMR HADOOP\_HOME/lib directory to Druid's druid.extensions.hadoopDependenciesDir \(by default, DRUID\_HOME/hadoop-dependencies\).

-   The following error is reported during indexing:

    ```
    2018-02-01T09:00:32,647 ERROR [task-runner-0-priority-0] com.hadoop.compression.lzo.GPLNativeCodeLoader - could not unpack the binaries
      java.io.IOException: No such file or directory
              at java.io.UnixFileSystem.createFileExclusively(Native Method) ~[?:1.8.0_151]
              at java.io.File.createTempFile(File.java:2024) ~[?:1.8.0_151]
              at java.io.File.createTempFile(File.java:2070) ~[?:1.8.0_151]
              at com.hadoop.compression.lzo.GPLNativeCodeLoader.unpackBinaries(GPLNativeCodeLoader.java:115) [hadoop-lzo-0.4.21-SNAPSHOT.jar:?]
    ```

    This issue occurs because the java.io.tmp path does not exist. Set the path and make sure that the Druid account has permission to access it.


