# Quick start {#concept_tfd_fld_z2b .concept}

E-MapReduce 3.11.0 and later support Druid as a cluster type.

Druid is used as a separate cluster type \(instead of being added to a Hadoop cluster\) for the following reasons:

-   Druid can be used independently of Hadoop.
-   Druid has high memory requirements when there is a large amount of data, especially for Broker and Historical nodes. Druid is not controlled by YARN, and will compete for resources during multi-service operation.
-   As an infrastructure, the number of nodes in a Hadoop cluster can be relatively large, whereas a Druid cluster can be relatively small. Using them together results in greater flexibility.

## Create a Druid cluster {#section_a2v_3ld_z2b .section}

Select the Druid cluster type when you create a cluster. You can select HDFS and YARN when creating a Druid cluster for testing only. We strongly recommend that you use a dedicated Hadoop cluster as the production environment.

## Configure a cluster {#section_ngn_jld_z2b .section}

-   Configure the cluster to use HDFS as deep storage for Druid

    For a standalone Druid cluster, you may need to store your index data in the HDFS of another Hadoop cluster. Therefore, you need to complete the settings for the connectivity between the two clusters. For details, see Interaction with [Hadoop clusters](#hadoop). You then need to configure the following items on the Druid configuration page and restart the service. The configuration items are in common.runtime on the configuration page.

    -   druid.storage.type: HDFS
    -   druid.storage.storageDirectory: The HDFS directory must be complete, such as hdfs://emr-header-1.cluster-xxxxxxxx:9000/druid/segments.
    **Note:** If the Hadoop cluster is an HA cluster, you must change emr-header-1.cluster-xxxxx:9000 to emr-cluster, or change port 9000 to port 8020.

-   Use OSS as deep storage for Druid

    E-MapReduce Druid supports the use of OSS as deep storage. Due to the AccessKey-free capability of E-MapReduce, Druid can automatically get access to OSS without having to configure the AccessKey. Because the OSS function of HDFS enables Druid to have access to OSS, druid.storage.type still needs to be configured as HDFS: during the configuration process.

    -   druid.storage.type: HDFS
    -   druid.storage.storageDirectory: For example, oss://emr-druid-cn-hangzhou/segments.
    Because the OSS function of HDFS enables Druid to have access to OSS, you need to select one of the following two scenarios:

    -   Install HDFS when you create a cluster. The system is then configured automatically. \(After HDFS is installed, you can choose not to use it, disable it, or use it for testing purposes only.\)
    -   Create hdfs-site.xml in the Druid configuration directory /etc/ecm/druid-conf/druid/\_common/. The content is as follows. Copy the file to the same directory of all nodes:

        ```
        &lt;? xml version="1.0"? >
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

-   Use RDS to save Druid metadata

    Use the MySQL database on the header-1 node to save Druid metadata. You can also use Alibaba Cloud RDS to save the metadata.

    The following uses RDS MySQL as an example to demonstrate the configuration. Before you configure it, make sure that:

    -   The RDS MySQL instance has been created.
    -   A separate account has been created for Druid to access RDS MySQL \(the root account is not recommended \). This example uses account name druid and password druidpw.
    -   Create a separate MySQL database for Druid metadata. Suppose the database is called druiddb.
    -   Make sure that account druid has permission to access druiddb.
    In the E-MapReduce console, click Manage next to the Druid cluster you want to configure. Click the Druid service, and then select the **Configuration** tab to find the common.runtime configuration file. Click **Custom Configuration** to add the following three configuration items:

    -   druid.metadata.storage.connector.connectURI, where the value is: jdbc:mysql://rm-xxxxx.mysql.rds.aliyuncs.com:3306/druiddb.
    -   druid.metadata.storage.connector.user, where the value is druid.
    -   druid.metadata.storage.connector.password, where the value is druidpw.
    Click the **Save**, **Configuration to Host**, and then **Restart Related Services** in the upper-right corner to make the configuration take effect.

    Log on to the RDS console to view the tables created by druiddb. You will find tables automatically created by druid.

-   Service memory configuration

    The memory of the Druid service consists of heap memory \(configured through jvm. config\) and direct memory \(configured through jvm. config and runtime. properteis\). E-MapReduce automatically generates a set of configurations when you create a cluster. However, in some cases, you may still need to configure the memory.

    To adjust the service memory configuration, you can access the cluster services through the E-MapReduce console and perform related operations on the page.

    **Note:** For direct memory, make sure that:

    ```
    -XX:MaxDirectMemorySize is greater than or equal to druid.processing.buffer.sizeBytes * (druid.processing.numMergeBuffers + druid.processing.numThreads + 1).
    ```


## Batch index {#section_wdw_2md_z2b .section}

-   Interaction with Hadoop clusters

    If you select HDFS and YARN \(with their own Hadoop clusters\) when creating your Druid clusters, the system automatically configures the interaction between HDFS and YARN. The following example shows how to configure the interaction between a standalone Druid cluster and a standalone Hadoop cluster. It is assumed that the Druid cluster ID is 1234 and the Hadoop cluster ID is 5678. If your clusters do not work as expected, this may be because of a slightly inaccurate operation.

    For the interaction with standard-mode Hadoop clusters, complete the following operations:

    1.  Ensure the communication between the two clusters. \(Each cluster is associated with a different security group, and access rules are configured for these security groups.\)
    2.  Put core-site.xml, hdfs-site.xml, yarn-site.xml, mapred-site.xml of /etc/ecm/hadoop-conf of the Hadoop cluster in the /etc/ecm/duird-conf/druid/\_common directory on each node of the Druid cluster. \(If you select built-in Hadoop when creating the cluster, several soft links in this directory will map to the configuration of the Hadoop service of E-MapReduce. Remove these soft links first.\)
    3.  Write the hosts of the Hadoop cluster to the hosts list on the Druid cluster. Note that the hostname of the Hadoop cluster should be a long name, such as emr-header-1.cluster-xxxxxxxx. We recommend that you put the Hadoop hosts after the hosts of the Druid cluster, such as:

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

    For Hadoop clusters in high-security mode, complete the following operations:

    1.  Ensure the communication between the two clusters. \(Each cluster is associated with a different security group, and access rules are configured for these security groups.\)
    2.  Put core-site.xml, hdfs-site.xml, yarn-site.xml, mapred-site.xml of /etc/ecm/hadoop-conf of the Hadoop cluster in the /etc/ecm/duird-conf/druid/\_common directory on each node of the Druid cluster. \(If you select built-in Hadoop when creating a cluster, several soft links in this directory will point to the configuration with Hadoop. Remove these soft links first.\) Modify hadoop.security.authentication.use.has in core-site.xml to false. \(This configuration is completed on the client to enable AccessKey authentication for users. If Kerberos authentication is used, disable AccessKey authentication.\)
    3.  Write the hosts of the Hadoop cluster to the hosts list of each node on the Druid cluster. Note that the hostname of the Hadoop cluster should be a long name, such as emr-header-1.cluster-xxxxxxxx. We recommend that you put the Hadoop hosts after the hosts of the Druid cluster.
    4.  Set Kerberos cross-domain mutual trust between the two clusters. For more details, see [Cross-region access](intl.en-US/User Guide/Kerberos authentication/Cross-region access.md#).
    5.  Create a local Druid account \(useradd-m-g hadoop\) on all nodes of the Hadoop cluster, or set druid.auth.authenticator.kerberos.authtomate to create a mapping rule for the Kerberos account to the local account. For specific pre-release rules, see [here](http://druid.io/docs/0.11.0/development/extensions-core/druid-kerberos.html). This method is recommended because it is easy to operate without errors.

        **Note:** In high-security Hadoop clusters, all Hadoop commands must be run from a local account. By default, this local account needs to have the same name as the principal. YARN also supports mapping a principal to a local account.

    6.  Restart the Druid service.
-   Use Hadoop to index batch data

    Druid has an example named wikiticker located in $\{DRUID\_HOME\}/quickstart. By default, $\{DRUID\_HOME\} is /usr/lib/ druid-current. Each line of the wikiticker document \(wikiticker-2015-09-12-sampled.json.gz\) is a record. Each record is a json object. The format is as follows:

    ```
    ```json
    {
        "Time": "2015-09-12T00: 46: 58.771Z ",
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

    To use Hadoop to index batch data, complete the following steps:

    1.  Decompress the compressed file and place it in a directory of HDFS \(such as: hdfs://emr-header-1.cluster-5678:9000/druid\). Run the following command on the Hadoop cluster:

        ```
        ### If you are operating on a standalone Hadoop cluster, copy a druid.keytab to Hadoop cluster after the mutual trust is established between the two clusters, and run the kinit command.
         kinit -kt /etc/ecm/druid-conf/druid.keytab druid
         ###
         hdfs dfs -mkdir hdfs://emr-header-1.cluster-5678:9000/druid
         hdfs dfs -put ${DRUID_HOME}/quickstart/wikiticker-2015-09-16-sampled.json hdfs://emr-header-1.cluster-5678:9000/druid
        ```

        **Note:** 

        -   Modify hadoop.security.authentication.use.has in /etc/ecm/hadoop-conf/core-site.xml to false before running the HDFS command for a high-security cluster.
        -   Make sure that you have created a Linux account named Druid on each node of the Hadoop cluster.
    2.  Modify Druid cluster $\{DRUID\_HOME\}/quickstart/wikiticker-index.json, as shown below:

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
        -   tuningConfig.type is hadoop.
        -   tuningConfig.jobProperties sets the classloader of the MapReduce job.
        -   hadoopDependencyCoordinates develops the version of Hadoop client.
    3.  Run the batch index command on the Druid cluster.

        ```
        cd ${DRUID_HOME}
         curl --negotiate -u:druid -b ~/cookies -c ~/cookies -XPOST -H 'Content-Type:application/json' -d @quickstart/wikiticker-index.json http://emr-header-1.cluster-1234:18090/druid/indexer/v1/task
        ```

        Note that items such as `- -negotiate`, `-u`, `-b`, and `-c` are for high-security mode Druid clusters. The Overlord port number is 18090 by default.

    4.  View the running state of the jobs.

        Enter http://emr-header-1.cluster-1234:18090/console.html into your browser to view how the jobs run. To access the page properly, you need to open an SSH tunnel in advance \(see [Connect to clusters using SSH](intl.en-US/User Guide/Connect to clusters using SSH.md#)\), and start a Chrome agent. If the high-security mode is enabled for the Druid cluster, you have to configure your browser to support the Kerberos authentication process. For more information, see [here](http://druid.io/docs/0.11.0/development/extensions-core/druid-kerberos.html).

    5.  Query the data based on Druid syntax.

        Druid has its own query syntax. You need to prepare a json-formatted query file that describes how you want to query. A topN query to the wikiticker data is as follows $\{DRUID\_HOME\}/quickstart/wikiticker-top-pages.json:

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

        Note that items such as`- -negotiate`, `-u`, `-b`, and`-c` are for Druid clusters in high-security mode. You can check the results of a specific query.

-   Real-time index

    We recommend that you use [Tranquility client](https://github.com/druid-io/tranquility) to send real-time data to Druid. Tranquility supports sending data to Druid in a variety of ways, such as Kafka, Flink, Storm, and Spark Streaming. For more information about the Kafka method, see [Tranquility](intl.en-US/User Guide/Open source components/Druid/Tranquility.md#). Druid also follows the Kafka section in Tranquility. For more information about how to use Tranquility and SDKs, see [Tranquility Help Document](https://github.com/druid-io/tranquility/tree/master/docs).

    For Kafka, you can also use the kafka-indexing-service extension. For details, see [Kafka Indexing Service](intl.en-US/User Guide/Open source components/Druid/Kafka Indexing Service.md#).

-   Troubleshoot index failures

    When the index fails, complete the following steps to troubleshoot the failure:

    -   **For the index of batch data**
        1.  If the curl command output displays an error or does not display any information, check the file format. Alternatively, add the -v parameter to the curl command to check the value returned from the RESTful API.
        2.  Observe the execution of jobs on the Overlord page. If the execution fails, view the logs on that page.
        3.  In many cases, logs are not generated. In the case of a Hadoop job, open the YARN page to check whether an index job has been generated.
        4.  If no errors are found, you need to log on to the Druid cluster, and view the execution logs of Overlord at /mnt/disk1/log/druid/overlord—emr-header-1.cluster-xxxx.log. In the case of an HA cluster, check the Overlord that you submitted the job to.
        5.  If the job has been submitted to Middlemanager, but a failure is returned from Middlemanager, you need to view the worker that the job is submitted to in Overlord, and log on to the worker to view the Middlemanager logs \(at /mnt/disk1/log/druid/middleManager-emr-header-1.cluster-xxxx.log\).
    -   For real-time index of Tranquility

        View the Tranquility log to check whether the message was received or dropped.

        The remaining troubleshooting steps are the same as steps 2 to 5 for batch indexing.

        Most errors are about cluster configurations and jobs. Cluster configuration errors are about memory parameters, cross-cluster connection, access to clusters in high-security mode, and principals. Job errors are about the format of the job description files, input data parsing, and other job-related configuration issues \(such as ioConfig\).


