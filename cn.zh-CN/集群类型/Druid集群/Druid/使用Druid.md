# 使用Druid

EMR-3.11.0及其后续版本，E-MapReduce支持将Druid作为单独的一种集群类型。

## 背景信息

E-MapReduce将Druid作为单独的集群类型，主要基于以下几方面的考虑：

-   E-MapReduce Druid可以完全脱离Hadoop来使用。
-   大数据量情况下，E-MapReduce Druid对内存要求比较高，尤其是Broker和Historical节点。E-MapReduce Druid本身资源不受YARN管控 ，在多服务运行时容易发生资源抢夺。
-   Hadoop作为基础设施，其规模通常较大，而E-MapReduce Druid集群较小，部署在同一集群上，由于规模不一致可能造成资源浪费，所以单独部署会更加灵活。

## 创建Druid集群

创建集群时选择Druid集群类型即可，详情请参见[创建集群](/cn.zh-CN/集群管理/集群配置/创建集群.md)。

**说明：** E-MapReduce Druid集群自带的HDFS和YARN仅供测试使用。对于生产环境，建议您使用专门的Hadoop集群。

## 配置集群

-   配置HDFS作为E-MapReduce Druid的Deep Storage。

    对于独立的E-MapReduce Druid集群，如果您需要存放索引数据至一个Hadoop集群的HDFS，请设置两个集群的连通性（详情请参见[与Hadoop集群交互](#hadoop)）。

    在E-MapReduce Druid**配置**页面的**common.runtime**页签，配置如下参数。

    |参数|描述|
    |--|--|
    |druid.storage.type|设置为hdfs。|
    |druid.storage.storageDirectory|HDFS目录，建议填写完整目录。例如hdfs://emr-header-1.cluster-xxxxxxxx:9000/druid/segments。|

    **说明：** 如果Hadoop集群为HA集群，emr-header-1.cluster-xxxxx:9000需要改成emr-cluster，或者把端口9000改成8020。

-   配置OSS作为E-MapReduce Druid的Deep Storage。

    在E-MapReduce Druid**配置**页面的**common.runtime**页签，配置如下参数。

    |参数|描述|
    |--|--|
    |druid.storage.type|设置为hdfs。|
    |druid.storage.storageDirectory|OSS目录。 例如oss://emr-druid-cn-hangzhou/segments。|

-   配置RDS作为E-MapReduce Druid的元数据存储。

    默认情况下E-MapReduce Druid利用emr-header-1节点上的本地MySQL数据库作为元数据存储。您也可以配置使用阿里云RDS作为元数据存储。

    1.  本示例以RDS MySQL版为例介绍。在具体配置之前，请先确保：
        -   已创建RDS MySQL实例。
        -   为E-MapReduce Druid访问RDS MySQL创建了单独的账户（不推荐使用root）。例如账户名为druid，密码为druidpw。
        -   为E-MapReduce Druid元数据创建单独的MySQL数据库。例如数据库名为druiddb。
        -   确保账户druid有权限访问druiddb。
    2.  在E-MapReduce Druid**配置**页面的**common.runtime**页签，单击**自定义配置**，添加如下三个配置项。

        |参数|描述|
        |--|--|
        |druid.metadata.storage.connector.connectURI|设置为jdbc:mysql://rm-xxxxx.mysql.rds.aliyuncs.com:3306/druiddb。|
        |druid.metadata.storage.connector.user|设置为druid。|
        |druid.metadata.storage.connector.password|设置为druidpw。|

    3.  依次单击右上角的**保存**、**部署客户端配置**和**重启All Components**，配置即可生效。
    4.  登录[RDS管理控制台](https://rds.console.aliyun.com/)，查看druiddb创建表的情况。
-   配置组件内存。

    E-MapReduce Druid组件内存设置主要包括两方面：

    -   堆内存。
    -   direct内存。

## 访问Druid web页面

E-MapReduce Druid自带三个Web页面：

-   Overlord：http://emr-header-1.cluster-1234:18090，用于查看Task运行情况。
-   Coordinator：http://emr-header-1.cluster-1234:18081，用于查看Segments存储情况，并设置Rule加载和丢弃Segments。
-   Router（EMR-3.23.0及后续版本）：http://emr-header-1.cluster-1234:18888，也称之为Console，是新版Druid的统一入口。

访问E-MapReduce Druid的Web页面：

-   在集群管理页面，单击**访问链接与端口**，找到Druid Router UI链接，单击链接进入。

    **说明：** 您可以使用Knox账号访问Druid Web页面，Knox账号创建请参见[管理用户](/cn.zh-CN/集群管理/集群规划/管理用户.md)，Knox使用请参见[Knox使用说明](/cn.zh-CN/集群类型/Hadoop集群/Knox.md)。

-   通过建立SSH隧道。详情请参见[通过SSH隧道方式访问开源组件Web UI](/cn.zh-CN/集群管理/集群配置/连接集群/通过SSH隧道方式访问开源组件Web UI.md)。

## 批量索引

-   与Hadoop集群交互

    您在创建E-MapReduce Druid集群时如果勾选了YARN，则系统会自动为您配置好HDFS和YARN的交互，您无需额外操作。下面的介绍是E-MapReduce 配置独立Druid集群与独立Hadoop集群之间交互。例如，E-MapReduce Druid集群cluster id为1234，Hadoop集群cluster id为5678。

    **说明：** 请严格按照指导进行操作，如果操作不当，集群可能就不会按照预期工作。

    非安全独立Hadoop集群，请按照如下操作进行：

    1.  确保集群间能够通信（两个集群在一个安全组下，或两个集群在不同安全组，但两个安全组之间配置了访问规则）。
    2.  在E-MapReduce Druid集群的每个节点的指定路径下，放置一份Hadoop集群中/etc/ecm/hadoop-conf路径下的core-site.xml、hdfs-site.xml、yarn-site.xml、 mapred-site.xml文件。 这些文件在E-MapReduce Druid集群节点上放置的路径与E-MapReduce集群的版本有关，详情说明如下：

        -   EMR-3.23.0及后续版本：/etc/ecm/druid-conf/druid/cluster/\_common
        -   EMR-3.23.0之前版本：/etc/ecm/druid-conf/druid/\_common
        **说明：** 如果创建集群时选了自带Hadoop，则在上述目录下会有几个软链接指向自带Hadoop的配置，请先移除这些软链接。

    3.  将Hadoop集群的hosts写入到E-MapReduce Druid集群的hosts列表中，注意Hadoop集群的hostname应采用长名形式，如emr-header-1.cluster-xxxxxxxx，且最好将Hadoop的hosts放在本集群hosts之后，例如：

        ```
        ...
        10.157.*.*    emr-as.cn-hangzhou.aliyuncs.com
        10.157.*.*    eas.cn-hangzhou.emr.aliyuncs.com
        192.168.*.*   emr-worker-1.cluster-1234 emr-worker-1 emr-header-2.cluster-1234 emr-header-2 iZbp1h9g7boqo9x23qb****
        192.168.*.*   emr-worker-2.cluster-1234 emr-worker-2 emr-header-3.cluster-1234 emr-header-3 iZbp1eaa5819tkjx55y****
        192.168.*.*   emr-header-1.cluster-1234 emr-header-1 iZbp1e3zwuvnmakmsje****
        --以下为hadoop集群的hosts信息
        192.168.*.*   emr-worker-1.cluster-5678 emr-header-2.cluster-5678 iZbp195rj7zvx8qar4f****
        192.168.*.*   emr-worker-2.cluster-5678 emr-header-3.cluster-5678 iZbp15vy2rsxoegki4q****
        192.168.*.*   emr-header-1.cluster-5678 iZbp10tx4egw3wfnh5o****
        ```

    安全Hadoop集群，请按如下操作进行：

    1.  确保集群间能够通信（两个集群在一个安全组下，或两个集群在不同安全组，但两个安全组之间配置了访问规则）。
    2.  在E-MapReduce Druid集群的每个节点的指定路径下，放置一份Hadoop集群/etc/ecm/hadoop-conf路径下的core-site.xml、hdfs-site.xml、yarn-site.xml、 mapred-site.xml文件，并修改core-site.xml中hadoop.security.authentication.use.has为false。

        其中，core-site.xml、hdfs-site.xml、yarn-site.xml、 mapred-site.xml文件在E-MapReduce Druid集群节点上放置的路径与E-MapReduce集群的版本有关，详情说明如下：

        -   EMR-3.23.0 及以上版本：/etc/ecm/druid-conf/druid/cluster/\_common
        -   EMR-3.23.0 以下版本：/etc/ecm/druid-conf/druid/\_common
        **说明：** 如果创建集群时选了自带Hadoop，则在上述目录下会有几个软链接指向自带Hadoop的配置，请先移除这些软链接。

        其中，hadoop.security.authentication.use.has是客户端配置，目的是让用户能够使用AccessKey进行认证。如果使用Kerberos认证方式，则需要disable该配置。

    3.  将Hadoop集群的hosts写入到E-MapReduce Druid集群每个节点的hosts列表中。

        Hadoop集群的hostname应采用长名形式，例如emr-header-1.cluster-xxxxxxxx，且最好将Hadoop的hosts放在本集群hosts之后。

    4.  设置两个集群间的Kerberos跨域互信，详情请参见[跨域互信](/cn.zh-CN/集群类型/Hadoop集群/Kerberos/跨域互信.md)。
    5.  在Hadoop集群的所有节点下都创建一个本地druid账户（`useradd -m -g hadoop druid`），或者设置druid.auth.authenticator.kerberos.authToLocal，创建Kerberos账户到本地账户的映射规则。

        预发规则请参见[Druid-Kerberos](http://druid.io/docs/0.11.0/development/extensions-core/druid-kerberos.html)。

        **说明：** 默认在安全Hadoop集群中，所有Hadoop命令必须运行在一个本地的账户中，该本地账户需要与Principal的name部分同名。YARN支持将一个Principal映射至本地一个账户。

    6.  重启Druid服务。
-   使用Hadoop对批量数据创建索引

    E-MapReduce Druid自带了一个名为wikiticker的例子，在$\{DRUID\_HOME\}/quickstart/tutorial目录下（$\{DRUID\_HOME\}默认为/usr/lib/druid-current）。wikiticker文件（wikiticker-2015-09-12-sampled.json.gz）的每一行是一条记录，每条记录是个JSON对象。其格式如下所示。

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

    使用Hadoop对批量数据创建索引，请按照如下步骤进行操作：

    1.  解压该压缩文件，并放置于HDFS的目录下（例如hdfs://emr-header-1.cluster-5678:9000/druid）。 在Hadoop集群上执行如下命令。

        ```
        ### 如果是在独立Hadoop集群上操作，设置两个集群互信之后需要拷贝一个druid.keytab到Hadoop集群再kinit。
         kinit -kt /etc/ecm/druid-conf/druid.keytab druid
         ###
         hdfs dfs -mkdir hdfs://emr-header-1.cluster-5678:9000/druid
         hdfs dfs -put ${DRUID_HOME}/quickstart/tutorial/wikiticker-2015-09-12-sampled.json hdfs://emr-header-1.cluster-5678:9000/druid
        ```

        **说明：**

        -   对于安全集群执行HDFS命令前先修改/etc/ecm/hadoop-conf/core-site.xml中的hadoop.security.authentication.use.has为false。
        -   请确保已经在Hadoop集群每个节点上创建名为druid的Linux账户。
    2.  准备数据索引任务文件$\{DRUID\_HOME\}/quickstart/tutorial/wikiticker-index.json，示例如下。

        ```
        {
             "type" : "index_hadoop",
             "spec" : {
                 "ioConfig" : {
                     "type" : "hadoop",
                     "inputSpec" : {
                         "type" : "static",
                         "paths" : "hdfs://emr-header-1.cluster-5678:9000/druid/wikiticker-2015-09-12-sampled.json"
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
             "hadoopDependencyCoordinates": ["org.apache.hadoop:hadoop-client:2.8.5"]
         }
        ```

        |参数|描述|
        |--|--|
        |spec.ioConfig.type|设置为hadoop。|
        |spec.ioConfig.inputSpec.paths|文件路径。|
        |tuningConfig.type|设置为hadoop。|
        |tuningConfig.jobProperties|设置MapReduce Job的Classloader。|
        |hadoopDependencyCoordinates|制定了Hadoop Client的版本。|

    3.  在E-MapReduce Druid集群上运行批量索引命令。

        ```
        cd ${DRUID_HOME}
         curl --negotiate -u:druid -b ~/cookies -c ~/cookies -XPOST -H 'Content-Type:application/json' -d @quickstart/tutorial/wikiticker-index.json http://emr-header-1.cluster-1234:18090/druid/indexer/v1/task
        ```

        其中`- -negotiate`、`-u`、`-b`、`-c`等选项是针对安全E-MapReduce Druid集群的。Overlord的端口默认为18090。

    4.  查看作业运行情况。

        在浏览器访问http://emr-header-1.cluster-1234:18090/console.html，查看作业运行情况。

    5.  根据Druid语法查询数据。

        Druid有自己的查询语法。请准备一个JSON格式的查询文件。如下所示为wikiticker数据的top N查询文件（$\{DRUID\_HOME\}/quickstart/tutorial/wikiticker-top-pages.json）。

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

        在命令行界面运行下面的命令即可看到查询结果。

        ```
        cd ${DRUID_HOME}
         curl --negotiate -u:druid -b ~/cookies -c ~/cookies -XPOST -H 'Content-Type:application/json' -d @quickstart/tutorial/wikiticker-top-pages.json 'http://emr-header-1.cluster-1234:18082/druid/v2/?pretty'
        ```

        其中`- -negotiate`、`-u`、`-b`、`-c`等选项是针对安全E-MapReduce Druid集群的。


## 实时索引

对于数据从Kafka集群实时到E-MapReduce Druid集群进行索引，推荐您使用Kafka Indexing Service扩展，提供高可靠保证和Exactly-Once语义。详情请参见[Kafka Indexing Service](/cn.zh-CN/集群类型/Druid集群/Druid/Kafka Indexing Service.md)。

如果您的数据实时到了阿里云日志服务（SLS），并想用E-MapReduce Druid实时索引这部分数据，您可以使用SLS Indexing Service扩展，提供高可靠保证和Exactly-Once语义。使用SLS Indexing Service避免了您额外建立并维护Kafka集群。详情请参见 [SLS-Indexing-Service](/cn.zh-CN/集群类型/Druid集群/Druid/SLS Indexing Service.md)。

