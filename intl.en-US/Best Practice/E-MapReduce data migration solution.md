# E-MapReduce data migration solution {#concept_bhj_rdy_wfb .concept}

This topic describes how to migrate data from a self-built cluster to an E-MapReduce \(EMR\) cluster.

Applicable migration scenarios include:

-   Migrating data from an offline Hadoop cluster to E-MapReduce.
-   Migrating data from a self-built Hadoop cluster on ECS to E-MapReduce.

Supported data sources include:

-   HDFS incremental upstream data sources such as RDS incremental data and Flume.

## Network interconnection between new and old clusters {#section_k22_sfy_wfb .section}

-   Self-built Hadoop cluster on an offline IDC

    A self-built Hadoop cluster can be migrated to E-MapReduce through OSS, or by using Alibaba Cloud Express Connect, to establish a connection between your offline IDC and the VPC in which your E-MapReduce cluster is located.

-   Self-built Hadoop cluster on ECS instances

    Because VPC networks are logically isoloated, we recommend that you use the VPC-Connected E-MapReduce service to establish an interconnection. Depending on the specific network types involved for interconnection, you need to perform the following actions:

    -   Interconnection between classic networks and VPC networks

        For a Hadoop cluster built on ECS instances, you need to interconnect the classic network and VPC network using the ECS [ClassicLink](../../../../intl.en-US/User Guide/ClassicLink/ClassicLink overview.md) method. For more information, see [Build a ClassicLink connection](../../../../intl.en-US/User Guide/ClassicLink/Build a ClassicLink connection.md#).

    -   Interconnection between VPC networks

        To ensure optimal connectivity between VPC networks, we recommned that you create the new cluster in the same region and zone as the old cluster.


## HDFS data migration {#section_ppp_mgy_wfb .section}

-   Synchronize data with DistCp

    For more information, see [Hadoop DistCp](http://hadoop.apache.org/docs/r2.7.5/hadoop-distcp/DistCp.html).

    You can migrate full and incremental HDFS data using the DistCp tool. To alleviate pressures on your current cluster resources, we recommend that you execute the distcp command after the new and old cluster networks are interconnected.

    -   Full data synchronization

        ```
        hadoop distcp -pbugpcax -m 1000 -bandwidth 30 hdfs://oldclusterip:8020 /user/hive/warehouse /user/hive/warehouse
        ```

    -   Incremental data synchronization

        ```
        hadoop distcp -pbugpcax -m 1000 -bandwidth 30  -update –delete hdfs://oldclusterip:8020 /user/hive/warehouse /user/hive/warehouse
        ```

    Parameter descriptions:

    -   hdfs://oldclusterip:8020 indicates the namenode IP of the old cluster. If there are multiple namenodes, input the namenode that is in the active status.
    -   By default, the number of replicas is 3. If you want to keep the original number of replicas, add r after -p, such as -prbugpcax. If the permissions and ACL do not need to be synchronized, remove p and a after -p.
    -   -m indicates the amount of maps and the size of the cluster, which corresponds to the data volume. For example, if a cluster has a 2000-core cpu, you can specify 2000 maps.
    -   -bandwidth indicates an estimated value of the synchronized speed of a single map, which is implemented by controlling the copy speed of replicas.
    -   -update, verifies the checksum and file size of the source and target files. If the file sizes compared are different, the source file updates the target cluster data. If there are data writes during the synchronization of the old and new clusters, -update can be used for incremental data synchronization.
    -   -delete, if data in the old cluster no longer exists, the data in the new cluster will be deleted.
    **Note:** 

    -   The overall speed of migration is affected by cluster badwidth and size. The larger the number of files, the longer the checksum takes to process. If you have a large amount of data to migrate, try to synchronize several directories to evaluate the overall time. If synchronization is performed within the specified time period, you can split the directory into several small directories and synchronize them one at a time.
    -   A short service stop is required for the full data synchronization to enable double write and double counting, and to directly switch the service to the new cluster.
-   HDFS permission configuration

    HDFS provides permission settings. Before migrating HDFS data, you need to ensure whether the old cluster has an ACL rule and if the rule is to be synchronized, and check if dfs.permissions.enabled and dfs.namenode.acls.enabled were configured the same in the old and new clusters. The configurations will take effect immediately.

    If there is an ACL rule to be synchronized, the distcp parameter must be added to -p to synchronize the permission parameter. If the distcp operation displays that the cluster does not support the ACL, this means that you did not set the ACL rule for the corresponding cluster. If the new cluster is not configured with the ACL rule, you can add it and restart NM. If the old cluster does not support an ACL rule, you do not need to set or synchronize an ACL rule.


## Hive metadata synchronization {#section_kgw_v3y_wfb .section}

-   Overview

    Hive metadata is generally stored in MySQL. When compared with MySQL data synchronization, note that:

    -   The location must be changed
    -   Hive version alignment is required
    E-MapReduce supports Hive metabases, including

    -   Unified metabase, whereby EMR manages RDS and each user has a schema
    -   Self-built RDS
    -   Self-built MySQL on ECS instances
    To ensure data is consistent after migration between the old and new clusters, we recommend that you stop the metastore service during the migration, open the metastore service on the old cluster after the migration, and then submit jobs on the new cluster.

-   Procedure:
    1.  Delete the metabase of the new cluster and input `drop database xxx`.
    2.  Run the `mysqldump` command to export the table structure and data of the old cluster's metabase.
    3.  Replace the location. Tables and partitions in the Hive metadata all have location information witinh the dfs nameservices prefix, such as hdfs://mycluster:8020/. However, the nameservices prefix of an E-MapReduce cluster is emr-cluster, which means you need to replace the location information.

        To replace the location information, export the data as follows.

        ```
        mysqldump --databases hivemeta --single-transaction -u root –p > hive_databases.sql
        ```

        Use sed to replace hdfs://oldcluster:8020/ with hdfs://emr-cluster/ and then import data into the new database.

        ```
        mysql hivemeta -p < hive_databases.sql
        ```

    4.  n the interface of the new cluster, stop the hivemetastore service.
    5.  Log on to the new metabase and create a database.
    6.  In the new metabase, import all data exported from the old metabase after the location field is replaced.
    7.  Currently, E-MapReduce Hive version is the latest stable version. However, if the Hive version of your self-built cluster is earlier, any imported data may not be directly usable. To resolve this issue, you need to execute the upgraded Hive script \(ignore table and field problems\). For more information, see [Hive upgrade scripts](https://github.com/apache/hive/tree/master/metastore/scripts/upgrade/mysql). For example, to upgrade Hive 1.2 to 2.3.0, execute upgrade-1.2.0-to-2.0.0.mysql.sql, upgrade-2.0.0-to-2.1.0.mysql.sql, upgrade-2.1.0-to-2.2.0.mysql.sql, and upgrade-2.2.0-to-2.3.0.mysql.sql in sequence. This script is mainly used to build the table, add the field, and change the content.
    8.  Exceptions that the table and the field already exist can be ignored. After all metadata are revised, restart MetaServer, input the hive command in the command line, query the database and table, and verify the information is correct.

## Flume data migration {#section_dfr_fly_wfb .section}

-   Flume double-write configuration

    Enable the Flume service in the new cluster and write the data to the new cluster in accordance with the rules that are identical to the old cluster.

-   Write the Flume partition table

    When executing the Flume data double-write, you must control the start timing to make sure that the new cluster is synchronized when Flume starts a new time partition. If Flume synchronizes all the tables every hour, you need to enable the Flume synchronization service before the next synchronization. This ensures that the data written by Flume in the new hour is properly duplicated. Incomplete old data is then synchronized when full data synchronization with DistCp is performed. The new data generated after the double-write time is enabled is not synchronized. When you partition data, do NOT put the new written data into the data synchronization directory.


## Job synchronization {#section_zrh_jly_wfb .section}

If the verion upgrades of Hadoop, Hive, Spark, and MapReduce are large, you may rebuild your jobs on demand.

Common issues and corresponding solutions are as follows:

-   Gateway OOM

    Change /etc/ecm/hive-conf/hive-env.sh.

    export HADOOP\_HEAPSIZE=512 is changed to 1024.

-   Insufficient job execution memory

    mapreduce.map.java.opts adjusts the startup parameters passed to the virtual machine when the JVM virtual macine is started. The default value -Xmx200m indicates the maximum amount of heap memory used by this Java program. When the amount is exceeded, the JVM displays the Out of Memory exception

    `and terminates the set mapreduce.map.java.opts=-Xmx3072m process.`

    mapreduce.map.memory.mb sets the memory limit of the Container, which is read and controlled by NodeManager. When the memory size of the Container exceeds this parameter value, NodeManager will kill the Container.

    `set mapreduce.map.memory.mb=3840`


## Data verification {#section_alv_sly_wfb .section}

Data is verified through a customer's self-generated reports.

## Presto cluster migration {#section_csb_5ly_wfb .section}

If a Presto cluster is used for data queries, the Hive configuration files need to be modified. For more information, see [Presto documentation](https://prestodb.io/docs/current/connector/hive.html).

The Hive properties that need to be modified are as follows:

-   connector.name=hive-hadoop2
-   hive.metastore.uri=thrift://emr-header-1.cluster-500148414:9083
-   hive.config.resources=/etc/ecm/hadoop-conf/core-site.xml, /etc/ecm/hadoop-conf/hdfs-site.xml
-   hive.allow-drop-table=true
-   hive.allow-rename-table=true
-   hive.recursive-directories=true

## Appendix {#section_ubq_1my_wfb .section}

Alignment example of upgrading Hive version 1.2 to 2.3:

```
source /usr/lib/hive-current/scripts/metastore/upgrade/mysql/upgrade-1.2.0-to-2.0.0.mysql.sql
CREATE TABLE COMPACTION_QUEUE (
  CQ_ID bigint PRIMARY KEY,
  CQ_DATABASE varchar(128) NOT NULL,
  CQ_TABLE varchar(128) NOT NULL,
  CQ_PARTITION varchar(767),
  CQ_STATE char(1) NOT NULL,
  CQ_TYPE char(1) NOT NULL,
  CQ_WORKER_ID varchar(128),
  Cq_start bigint,
  CQ_RUN_AS varchar(128),
  CQ_HIGHEST_TXN_ID bigint,
  CQ_META_INFO varbinary(2048),
  CQ_HADOOP_JOB_ID varchar(32)
) ENGINE=InnoDB DEFAULT CHARSET=latin1;
CREATE TABLE TXNS (
  TXN_ID bigint PRIMARY KEY,
  TXN_STATE char(1) NOT NULL,
  TXN_STARTED bigint NOT NULL,
  TXN_LAST_HEARTBEAT bigint NOT NULL,
  TXN_USER varchar(128) NOT NULL,
  TXN_HOST varchar(128) NOT NULL,
  TXN_AGENT_INFO varchar(128),
  TXN_META_INFO varchar(128),
  TXN_HEARTBEAT_COUNT int
) ENGINE=InnoDB DEFAULT CHARSET=latin1;
CREATE TABLE HIVE_LOCKS (
  HL_LOCK_EXT_ID bigint NOT NULL,
  HL_LOCK_INT_ID bigint NOT NULL,
  HL_TXNID bigint,
  HL_DB varchar(128) NOT NULL,
  HL_TABLE varchar(128),
  HL_PARTITION varchar(767),
  HL_LOCK_STATE char(1) not null,
  HL_LOCK_TYPE char(1) not null,
  HL_LAST_HEARTBEAT bigint NOT NULL,
  HL_ACQUIRED_AT bigint,
  HL_USER varchar(128) NOT NULL,
  HL_HOST varchar(128) NOT NULL,
  HL_HEARTBEAT_COUNT int,
  HL_AGENT_INFO varchar(128),
  HL_BLOCKEDBY_EXT_ID bigint,
  HL_BLOCKEDBY_INT_ID bigint,
  PRIMARY KEY(HL_LOCK_EXT_ID, HL_LOCK_INT_ID),
  KEY HIVE_LOCK_TXNID_INDEX (HL_TXNID)
) ENGINE=InnoDB DEFAULT CHARSET=latin1;
CREATE INDEX HL_TXNID_IDX ON HIVE_LOCKS (HL_TXNID);
source /usr/lib/hive-current/scripts/metastore/upgrade/mysql/upgrade-1.2.0-to-2.0.0.mysql.sql
source /usr/lib/hive-current/scripts/metastore/upgrade/mysql/upgrade-2.0.0-to-2.1.0.mysql.sql

CREATE TABLE TXN_COMPONENTS (
  TC_TXNID bigint,
  TC_DATABASE varchar(128) NOT NULL,
  TC_TABLE varchar(128),
  TC_PARTITION varchar(767),
  FOREIGN KEY (TC_TXNID) REFERENCES TXNS (TXN_ID)
) ENGINE=InnoDB DEFAULT CHARSET=latin1;
source /usr/lib/hive-current/scripts/metastore/upgrade/mysql/upgrade-2.0.0-to-2.1.0.mysql.sql
source /usr/lib/hive-current/scripts/metastore/upgrade/mysql/upgrade-2.1.0-to-2.2.0.mysql.sql
CREATE TABLE IF NOT EXISTS `NOTIFICATION_LOG`
(
    `NL_ID` BIGINT(20) NOT NULL,
    `EVENT_ID` BIGINT(20) NOT NULL,
    `EVENT_TIME` INT(11) NOT NULL,
    `EVENT_TYPE` varchar(32) NOT NULL,
    `DB_NAME` varchar(128),
    `TBL_NAME` varchar(128),
    `MESSAGE` mediumtext,
    PRIMARY KEY (`NL_ID`)
) ENGINE=InnoDB DEFAULT CHARSET=latin1;
CREATE TABLE IF NOT EXISTS `PARTITION_EVENTS` (
  `PART_NAME_ID` bigint(20) NOT NULL,
  `DB_NAME` varchar(128) CHARACTER SET latin1 COLLATE latin1_bin DEFAULT NULL,
  `EVENT_TIME` bigint(20) NOT NULL,
  `EVENT_TYPE` int(11) NOT NULL,
  `PARTITION_NAME` varchar(767) CHARACTER SET latin1 COLLATE latin1_bin DEFAULT NULL,
  `TBL_NAME` varchar(128) CHARACTER SET latin1 COLLATE latin1_bin DEFAULT NULL,
  PRIMARY KEY (`PART_NAME_ID`),
  KEY `PARTITIONEVENTINDEX` (`PARTITION_NAME`)
) ENGINE=InnoDB DEFAULT CHARSET=latin1;

CREATE TABLE COMPLETED_TXN_COMPONENTS (
  CTC_TXNID bigint NOT NULL,
  CTC_DATABASE varchar(128) NOT NULL,
  CTC_TABLE varchar(128),
  CTC_PARTITION varchar(767)
) ENGINE=InnoDB DEFAULT CHARSET=latin1;
 source /usr/lib/hive-current/scripts/metastore/upgrade/mysql/upgrade-2.1.0-to-2.2.0.mysql.sql
 source /usr/lib/hive-current/scripts/metastore/upgrade/mysql/upgrade-2.2.0-to-2.3.0.mysql.sql
  CREATE TABLE NEXT_TXN_ID (
  NTXN_NEXT bigint NOT NULL
) ENGINE=InnoDB DEFAULT CHARSET=latin1;
INSERT INTO NEXT_TXN_ID VALUES(1);
CREATE TABLE NEXT_LOCK_ID (
  NL_NEXT bigint NOT NULL
) ENGINE=InnoDB DEFAULT CHARSET=latin1;
INSERT INTO NEXT_LOCK_ID VALUES(1);
```

