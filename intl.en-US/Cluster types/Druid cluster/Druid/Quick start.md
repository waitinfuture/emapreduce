# Quick start

E-MapReduce V3.11.0 and later versions support E-MapReduce Druid as a cluster type.

The use of E-MapReduce Druid as a separate cluster type \(instead of adding Druid service to the Hadoop cluster\) is mainly based on the following reasons:

-   E-MapReduce Druid can be used independently of Hadoop.
-   E-MapReduce Druid has high memory requirements when there is a large amount of data, especially for Broker and Historical nodes. E-MapReduce Druid is not controlled by Yarn and will compete for resources during multi-service operation.
-   As the infrastructure, the node number of a Hadoop cluster can be relatively large, whereas an E-MapReduce Druid cluster can be relatively small. The work is more flexible if they work together.

## Create an Druid cluster

Select Druid as the cluster type when you create a cluster. You can select Superset and Yarn when creating an E-MapReduce Druid cluster. The HDFS and Yarn in the Druid cluster are for testing only, as described at the beginning of this guide. We recommend that you use a dedicated Hadoop cluster as the production environment.

## Configure a cluster

-   Configure the cluster to use HDFS as the deep storage of E-MapReduce Druid

    For a standalone E-MapReduce Druid cluster, you may need to store your index data in the HDFS of another Hadoop cluster. Therefore, you need to complete related settings for the connectivity between the two clusters \(for details, see [Interaction with Hadoop clusters](#hadoop)\). Then you need to configure the following items on the configuration page of E-MapReduce Druid and restart the service. The configuration items are in common.runtime of the configuration page.

    -   druid.storage.type: hdfs
    -   druid.storage.storageDirectory: \(the hdfs directory must be a full one, such as hdfs://emr-header-1.cluster-xxxxxxxx:9000/druid/segments.\)
    **Note:** If the Hadoop cluster is an HA cluster, you must change emr-header-1.cluster-xxxxx:9000 to emr-cluster, or change port 9000 to port 8020.

-   Use OSS as the deep storage of E-MapReduce Druid

    E-MapReduce Druid supports the use of OSS as deep storage. Due to the AccessKey-free capability of E-MapReduce, E-MapReduce Druid can automatically get access to OSS without the need to configure the AccessKey. Because the OSS function of HDFS enables E-MapReduce Druid to have access to OSS, druid.storage.type still needs to be configured as HDFS during the configuration process.

    -   druid.storage.type: hdfs
    -   druid.storage.storageDirectory: \(for example, oss://emr-druid-cn-hangzhou/segments）
    Because the OSS function of HDFS enables E-MapReduce Druid to have access to OSS, you need to select one of the following two scenarios:

    -   Choose to install HDFS when you create a cluster. Then the system is automatically configured. \(After HDFS is installed, you can choose not to use it, disable it, or use it for testing purposes only.\)
    -   Create hdfs-site.xml in the configuration directory of E-MapReduce Druid /etc/ecm/druid-conf/druid/\_common/, the content is as follows, and then copy the file to the same directory of all nodes:

        ```
        <? xml version="1.0" ? >
          <configuration>
            <property> 
              <name>fs.oss.impl</name>
              <value>com.aliyun.fs.oss.nat.NativeOssFileSystem</value> 
            </property>
            <property>
              <name>fs.oss.buffer.dirs</name> 
              <value>file:///mnt/disk1/data,...</value> 
            </property> 
            <property>
              <name>fs.oss.impl.disable.cache</name>
              <value>true</value> 
            </property>
          </configuration>
        ```

        The fs.oss.buffer.dirs can be set to multiple paths.

-   Use RDS to save E-MapReduce Druid metadata

    Use the MySQL database on header-1 node to save E-MapReduce Druid metadata. You can also use the Alibaba Cloud RDS to save the metadata.

    The following uses RDS MySQL as an example to demonstrate the configuration. Before you configure it, make sure that:

    -   The RDS MySQL instance has been created.
    -   A separate account has been created for E-MapReduce Druid to access RDS MySQL \(root is not recommended \). This example uses account name druid and password druidpw.
    -   Create a separate MySQL database for Druid metadata. Suppose the database is called druiddb.
    -   Make sure that account druid has permission to access druiddb.
    In the E-MapReduce console, click Manage behind the E-MapReduce Druid cluster you want to configure. Click the Druid service, and then select the **Configuration** tab to find the common.runtime configuration file. Click **Custom Configuration** to add the following three configuration items:

    -   druid.metadata.storage.connector.connectURI, where the value is: jdbc:mysql://rm-xxxxx.mysql.rds.aliyuncs.com:3306/druiddb
    -   druid.metadata.storage.connector.user, where the value is druid.
    -   druid.metadata.storage.connector.password, where the value is druidpw.
    Choose **Save**,**Deploy Client Configuration**,**Restart All Components** to make the configuration take effect.

    Log on to the RDS console to view the tables created by druiddb. You will find tables automatically created by druid.

-   Service memory configuration

    The memory of the Druid service consists of the heap memory \(configured through jvm.config\), and direct memory \(configured through jvm.config and runtime.properteis\). E-MapReduce will automatically generate a set of configurations when you create a cluster. However, in some cases, you may still need to configure the memory.

    To adjust the service memory configuration, you can access the cluster services through the E-MapReduce console, and perform related operations on the page.

    **Note:** For direct memory, make sure that:

    ```
    -XX:MaxDirectMemorySize is greater than or equal to druid.processing.buffer.sizeBytes * (druid.processing.numMergeBuffers + druid.processing.numThreads + 1).
    ```


## Visit the E-MapReduce Druid web page

E-MapReduce Druid comes with two Web pages:

-   Overlord: http://emr-header-1.cluster-1234:18090 is used to view the running status of tasks.
-   Coordinator: http://emr-header-1.cluster-1234:18081 is used to view the storage status of segments.

EMR provides three methods to access E-MapReduce Druid Web pages:

-   On the cluster management page, click **Access Link and port**, locate the Druid overlord or Druid coordinator link, and click the link to enter \(recommended method, EMR-3.20.0, and later versions \).
-   Use [SSH tunneling](/intl.en-US/Cluster Management/Configure clusters/Log on to a cluster by using SSH.md) to create an SSH tunnel and enable proxy browser access.
-   Access through public IP + Port, as shown in figureHttp: // 123.123.123.123: 18090\(Not recommended. use security group settings to properly control cluster access through the public network \).

## Batch index

-   Interaction with Hadoop clusters

    If you select HDFS and Yarn \(with their own Hadoop clusters\) when creating E-MapReduce Druid clusters, the system will automatically configure the interaction between HDFS and Yarn. The following example shows how to configure the interaction between a standalone E-MapReduce Druid cluster and a standalone Hadoop cluster. It is assumed that the E-MapReduce Druid cluster ID is 1234, and the Hadoop cluster ID is 5678. In addition, read through and follow the instructions strictly. The clusters may not work as expected because of slightly improper operation.

    For the interaction with standard-mode Hadoop clusters, perform the following operations:

    1.  Ensure the communication between the two clusters. \(Each cluster is associated with a different security group,and access rules are configured for the two security groups.\)
    2.  Put core-site.xml, hdfs-site.xml, yarn-site.xml, mapred-site.xml of /etc/ecm/hadoop-conf of the Hadoop cluster in the /etc/ecm/duird-conf/druid/\_common directory on each node of the E-MapReduce Druid cluster. \(If you select the built-in Hadoop when you create the cluster, several soft links in this directory will map to the configuration of the Hadoop service of E-MapReduce. Remove these soft links first.\)
    3.  Write the hosts of the Hadoop cluster to the hosts list on the E-MapReduce Druid cluster. Note that the hostname of the Hadoop cluster should be in the form of a long name, such as emr-header-1.cluster-xxxxxxxx. You are advised to put the hosts of Hadoop behind the hosts of the E-MapReduce Druid cluster, such as:

        ```
        ...
        10.157.201.36   emr-as.cn-hangzhou.aliyuncs.com
        10.157.64.5     eas.cn-hangzhou.emr.aliyuncs.com
        192.168.142.255 emr-worker-1.cluster-1234 emr-worker-1 emr-header-2.cluster-1234 emr-header-2 iZbp1h9g7boqo9x23qbifiZ
        192.168.143.0   emr-worker-2.cluster-1234 emr-worker-2 emr-header-3.cluster-1234 emr-header-3 iZbp1eaa5819tkjx55yr9xZ
        192.168.142.254 emr-header-1.cluster-1234 emr-header-1 iZbp1e3zwuvnmakmsjer2uZ
        For Hadoop clusters in high-security mode, perform the following operations:
        192.168.143.6   emr-worker-1.cluster-5678 emr-worker-1 emr-header-2.cluster-5678 emr-header-2 iZbp195rj7zvx8qar4f6b0Z
        192.168.143.7 emr-worker-2.cluster-5678 emr-worker-2 emr-header-3.cluster-5678 emr-header-3 iZbp15vy2rsxoegki4qhdpZ
        192.168.143.5 emr-header-1.cluster-5678 emr-header-1 iZbp10tx4egw3wfnh5oii1Z
        ```

    For Hadoop clusters in high-security mode, perform the following operations:

    1.  Ensure the communication between the two clusters. \(Each cluster is associated with a different security group,and access rules are configured for the two security groups.\)
    2.  Put core-site.xml, hdfs-site.xml, yarn-site.xml, mapred-site.xml of /etc/ecm/hadoop-conf of the Hadoop cluster in the /etc/ecm/duird-conf/druid/\_common directory on each node of the E-MapReduce Druid cluster. \(If you select the built-in Hadoop when creating a cluster, several soft links in this directory will point to the configuration with Hadoop. Remove these soft links first.\) Modify hadoop.security.authentication.use.has in core-site.xml to false. \(This configuration is completed on the client to enable AccessKey authentication for users. If Kerberos authentication is used, disable AccessKey authentication.\)
    3.  Write the hosts of the Hadoop cluster to the hosts list of each node on the E-MapReduce Druid cluster. Note that the hostname of the Hadoop cluster should be in the form of a long name, such as emr-header-1.cluster-xxxxxxxx. You are advised to put the hosts of Hadoop behind the hosts of the E-MapReduce Druid cluster.
    4.  Set Kerberos cross-domain mutual trust between the two clusters. For more information, see [Cross-region access](/intl.en-US/Cluster types/Hadoop cluster/Kerberos/Cross-region access.md).
    5.  Create a local druid account \(useradd-m-g hadoop\) on all nodes of the Hadoop cluster, or set druid.auth.authenticator.kerberos.authtomate to create a mapping rule for the Kerberos account to the local account. \(For specific pre-release rules, see [Druid-Kerberos](http://druid.io/docs/0.11.0/development/extensions-core/druid-kerberos.html).\) This method is recommended because it is easy to operate without errors.

        **Note:** In Hadoop cluster of the high-security mode , all Hadoop commands must be run from a local account. By default, this local account needs to have the same name as the principal. Yarn also supports mapping a principal to a local account.

    6.  Restart the E-MapReduce DruidDruid service.
-   Use Hadoop to index batch data

    Druid comes with an example named wikiticker, which is located in the $\{DRUID\_HOME\}/quickstart/tutorial path. $\{DRUID\_HOME\}is set to /usr/lib/druid-current by default. Each line of the wikiticke document \(wikiticker-2015-09-12-sampled.json.gz\) is a record. Each record is a json object. The format is as follows:

    ```
    ```json
    {
        "time": "2015-09-12T00:46:58.771Z",
        "channel": "#en.wikipedia",
        "cityName": null,
        "comment": "added project",
        "countryIsoCode": null,
        "countryName": null,
        "isAnonymous": false,
        "isMinor": false,
        "isNew": false,
        "isRobot": false,
        "isUnpatrolled": false,
        "metroCode": null,
        "namespace": "Talk",
        "page": "Talk:Oswald Tilghman",
        "regionIsoCode": null,
        "regionName": null,
        "user": "GELongstreet",
        "delta": 36,
        "added": 36,
        "deleted": 0
    }
    ```
    ```

    To use Hadoop to create index for batch data, perform the following steps:

    1.  Decompress the compressed file and place it in a directory of HDFS \(such as: hdfs://emr-header-1.cluster-5678:9000/druid\). Run the following command on the Hadoop Cluster.

        ```
        ### If you are operating on a standalone Hadoop cluster, copy a druid.keytab to Hadoop cluster after the mutual trust is established between the two clusters, and run the kinit command.
         kinit -kt /etc/ecm/druid-conf/druid.keytab druid
         ###
         hdfs dfs -mkdir hdfs://emr-header-1.cluster-5678:9000/druid
         hdfs dfs -put ${DRUID_HOME}/quickstart/wikiticker-2015-09-16-sampled.json hdfs://emr-header-1.cluster-5678:9000/druid
        ```

        **Note:**

        -   Modify hadoop.security.authentication.use.has in /etc/ecm/hadoop-conf/core-site.xml to false before running HDFS command for a high-security mode cluster.
        -   Make sure that you have created a Linux account named druid on each node of the Hadoop cluster.
    2.  Use the following configurations to prepare a file for data indexing. The file path is set to $\{DRUID\_HOME\}/quickstart/tutorial/wikiticker-index.json.

        ```
        {
             "type" : "index_hadoop",
             "spec" : {
                 "ioConfig" : {
                     "type" : "hadoop",
                     "inputSpec" : {
                         "type" : "static",
                         "paths" : "hdfs://emr-header-1.cluster-5678:9000/druid/wikiticker-2015-09-16-sampled.json"
                     }
                 },
                 "dataSchema" : {
                     "dataSource" : "wikiticker",
                     "granularitySpec" : {
                         "type" : "uniform",
                         "segmentGranularity" : "day",
                         "queryGranularity" : "none",
                         "intervals" : ["2015-09-12/2015-09-13"]
                     },
                     "parser" : {
                         "type" : "hadoopyString",
                         "parseSpec" : {
                             "format" : "json",
                             "dimensionsSpec" : {
                                 "dimensions" : [
                                     "channel",
                                     "cityName",
                                     "comment",
                                     "countryIsoCode",
                                     "countryName",
                                     "isAnonymous",
                                     "isMinor",
                                     "isNew",
                                     "isRobot",
                                     "isUnpatrolled",
                                     "metroCode",
                                     "namespace",
                                     "page",
                                     "regionIsoCode",
                                     "regionName",
                                     "user"
                                 ]
                             },
                             "timestampSpec" : {
                                 "format" : "auto",
                                 "column" : "time"
                             }
                         }
                     },
                     "metricsSpec" : [
                         {
                             "name" : "count",
                             "type" : "count"
                         },
                         {
                             "name" : "added",
                             "type" : "longSum",
                             "fieldName" : "added"
                         },
                         {
                             "name" : "deleted",
                             "type" : "longSum",
                             "fieldName" : "deleted"
                         },
                         {
                             "name" : "delta",
                             "type" : "longSum",
                             "fieldName" : "delta"
                         },
                         {
                             "name" : "user_unique",
                             "type" : "hyperUnique",
                             "fieldName" : "user"
                         }
                     ]
                 },
                 "tuningConfig" : {
                     "type" : "hadoop",
                     "partitionsSpec" : {
                         "type" : "hashed",
                         "targetPartitionSize" : 5000000
                     },
                     "jobProperties" : {
                         "mapreduce.job.classloader": "true"
                     }
                 }
             },
             "hadoopDependencyCoordinates": ["org.apache.hadoop:hadoop-client:2.7.2"]
         }
        ```

        **Note:**

        -   spec.ioConfig.type is set to hadoop.
        -   spec.ioConfig.inputSpec.paths is the path of the input file.
        -   tuningConfig.type is set to hadoop.
        -   tuningConfig.jobProperties sets the classloader of the mapreduce job.
        -   hadoopDependencyCoordinates develops the version of Hadoop client.
    3.  Run the batch index command on the E-MapReduce Druid cluster.

        ```
        cd ${DRUID_HOME}
         curl --negotiate -u:druid -b ~/cookies -c ~/cookies -XPOST -H 'Content-Type:application/json' -d @quickstart/wikiticker-index.json http://emr-header-1.cluster-1234:18090/druid/indexer/v1/task
        ```

        The `- -negotiate`, `-u`, `-b`, and `-c` options are for secure E-MapReduce Druid clusters. The Overlord port number is 18090 by default.

    4.  View the running state of the jobs.

        Enter http://emr-header-1.cluster-1234:18090/console.html in your browser's address bar to view the running status of jobs.

    5.  Query the data based on E-MapReduce Druid syntax.

        E-MapReduce Druid has its own query syntax. You need to prepare a json-formatted query file that describes how you want to query. A topN query to the wikiticker data is as follows $\{DRUID\_HOME\}/quickstart/wikiticker-top-pages.json\):

        ```
        {
             "queryType" : "topN",
             "dataSource" : "wikiticker",
             "intervals" : ["2015-09-12/2015-09-13"],
             "granularity" : "all",
             "dimension" : "page",
             "metric" : "edits",
             "threshold" : 25,
             "aggregations" : [
                 {
                     "type" : "longSum",
                     "name" : "edits",
                     "fieldName" : "count"
                 }
             ]
         }
        ```

        You can check the results of the query by running the following command:

        ```
        cd ${DRUID_HOME}
         curl --negotiate -u:druid -b ~/cookies -c ~/cookies -XPOST -H 'Content-Type:application/json' -d @quickstart/wikiticker-top-pages.json 'http://emr-header-1.cluster-1234:18082/druid/v2/?pretty'
        ```

        Note that the items such as`- -negotiate`、`-u`、`-b`、`-c` are for E-MapReduce Druid clusters in the high-security mode. You can check the results of a specific query in normal cases.


## Real-time index

For Indexing data from a Kafka cluster to an E-MapReduce Druid cluster in real time, we recommend that you use the Kafka Indexing Service extension to ensure high reliability and support exactly-once semantics. See the Use Druid Kafka Indexing Service to consume Kafka data in real time section in [Kafka Indexing Service](/intl.en-US/Cluster types/Druid cluster/Druid/Kafka Indexing Service.md).

If your data is real-time accessed by Alibaba Cloud log Service \(SLS\) and you want to use E-MapReduce Druid to index the data in real time, we provide the SLS Indexing Service extension. Using SLS Indexing Service avoids the overhead of creating and maintaining a Kafka cluster. SLS Indexing Service provides high-reliability and exactly-once semantics like Kafka Indexing Service. Here, you can use SLS as a Kafka.

Kafka Indexing Service and SLS Indexing Service are similar. They pull data from the data source to the E-MapReduce Druid cluster in pull mode, and provide high reliability and exactly-once semantics.

## Analyze the indexing failure

When indexing fails, the following troubleshooting steps are typically followed:

-   For batch data index
    1.  If curl returns an error directly, or no value returns, check the input file format. Or add a -v parameter to curl to observe the value returned from the REST API.
    2.  Observe the execution of the jobs on the Overlord page. If it fails, view the logs on the page.
    3.  In many cases, logs are not generated. In the case of a Hadoop job, open the Yarn page to check whether there is an index job generated, and view the job execution log.
    4.  If no errors are found, you need to log on to the E-MapReduce Druid cluster, and view the execution logs of Overlord \(at /mnt/disk1/log/druid/overlord—emr-header-1.cluster-xxxx.log\). If it is an HA cluster, check the Overlord that you submitted the job to.
    5.  If the job has been submitted to Middlemanager, but a failure is returned, you need to view the worker that the job is submitted to in Overlord, and log on to the worker node to view the Middlemanager logs in /mnt/disk1/log/druid/middleManager-emr-header-1.cluster-xxxx.log.
-   For Kafka Indexing Service and SLS Indexing Service
    1.  First, view the Overlord Web page Http://emr-header-1:18090, check the running status of the Supervisor, and check whether payload is valid.
    2.  View the log of the failed task.
    3.  If you cannot identify the cause of failure from the task log, you need to start with the Overlord log to troubleshoot the problem.

