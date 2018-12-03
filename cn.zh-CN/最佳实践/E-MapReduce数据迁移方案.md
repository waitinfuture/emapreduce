# E-MapReduce数据迁移方案 {#concept_bhj_rdy_wfb .concept}

在开发过程中我们通常会碰到需要迁移数据的场景，本篇将介绍如何将自建集群数据迁移到E-MapReduce集群中。

适用范围：

-   线下Hadoop到EMR迁移
-   线上ECS自建Hadoop到EMR迁移

迁移场景：

-   HDFS增量上游数据源包括RDS增量数据、flume

## 新旧集群网络打通 {#section_k22_sfy_wfb .section}

-   线下IDC自建Hadoop

    现在自建Hadoop迁移到EMR可以通过OSS进行过度，或者使用阿里云高速通道产品建立线下IDC和线上EMR所在VPC网络的连通。

-   利用ECS自建Hadoop

    由于VPC实现用户专有网络之间的逻辑隔离，EMR建议使用VPC网络。

    -   经典网络与VPC网络打通

        如果ECS自建Hadoop，需要通过ECS的[classiclink](../../../../intl.zh-CN/用户指南/ClassicLink/ClassicLink概述.md)的方式将经典网络和VPC网络打通。参见[建立ClassicLink连接](../../../../intl.zh-CN/用户指南/ClassicLink/建立ClassicLink连接.md#)。

    -   VPC网络之间连通

        数据迁移一般需要较高的网络带宽连通，建议新旧集群尽量处在同一个区域的同一个可用区内。


## HDFS数据迁移 {#section_ppp_mgy_wfb .section}

-   Distcp工具同步数据

    请参考[Hadoop DistCp工具官方说明文档](http://hadoop.apache.org/docs/r2.7.5/hadoop-distcp/DistCp.html)。

    HDFS数据迁移可以通过Hadoop社区标准的distcp工具迁移，可以实现全量和增量的数据迁移。为减轻现有集群资源压力，建议在新旧集群网络连通后在新集群执行distcp命令。

    -   全量数据同步

        ```
        hadoop distcp -pbugpcax -m 1000 -bandwidth 30 hdfs://oldclusterip:8020 /user/hive/warehouse /user/hive/warehouse
        ```

    -   增量数据同步

        ```
        hadoop distcp -pbugpcax -m 1000 -bandwidth 30  -update –delete hdfs://oldclusterip:8020 /user/hive/warehouse /user/hive/warehouse
        ```

    参数说明：

    -   hdfs://oldclusterip:8020 填写旧集群nameode ip，多个namenode情况填写当前状态为active的。
    -   默认副本数为3，如想保留原有副本数，-p后加r如-prbugpcax。如果不同步权限和acl，-p后去掉p和a。
    -   -m指定map数，和集群规模，数据量有关。比如集群有2000核cpu，就可以指定2000个map。
    -   -bandwidth指定单个map的同步速度，是靠控制副本复制速度实现的，是大概值。
    -   -update，校验源和目标文件的checksum和文件大小，如果不一致源文件会更新掉目标集群数据，新旧集群同步期间还有数据写入，可以通过-update做增量数据同步。
    -   -delete，如果源集群数据不存在，新集群的数据也会被删掉。
    **说明：** 

    -   迁移整体速度受集群间带宽，集群规模影响。同时文件越多，checksum需要的时间越长。如果迁移数据量大，可以先试着同步几个目录评估一下整体时间。如果只能在指定时间段内同步，可以将目录切为几个小目录，依次同步。
    -   一般全量数据同步，需要有个短暂的业务停写，以启用双写双算或直接将业务切换到新集群上。
-   HDFS权限配置

    HDFS有权限设置，确定旧集群是否有acl规则，是否要同步，检查dfs.permissions.enabled 和dfs.namenode.acls.enabled的配置新旧集群是否一致，按照实际需要修改。

    如果有acl规则要同步，distcp参数要加-p同步权限参数。如果distcp操作提示xx集群不支持acl，说明对应集群没配置acl规则。新集群没配置acl规则可以修改配置并重启NM。旧集群不支持，说明旧集群根本就没有acl方面的设置，也不需要同步。


## Hive元数据同步 {#section_kgw_v3y_wfb .section}

-   概述

    Hive元数据，一般存在MySQL里，与一般MySQL同步数据相比，要注意两点：

    -   Location变化
    -   Hive版本对齐
    EMR支持Hive Meta DB：

    -   统一元数据库，EMR管控RDS，每个用户一个Schema
    -   用户自建RDS
    -   用户ECS自建MySQL
    为了保证迁移之后新旧数据完全一致，最好是在迁移的时候将老的metastore服务停掉，等迁移过去之后，再把旧集群上的metastore服务打开，然后新集群开始提交业务作业。

-   操作步骤：
    1.  将新集群的元数据库删除，直接输出命令`drop database xxx`；
    2.  将旧集群的元数据库的表结构和数据通过`mysqldump`命令全部导出；
    3.  替换location，hive元数据中的表，分区等信息均带有location信息的，带dfs nameservices前缀，如hdfs://mycluster:8020/，而EMR集群的nameservices前缀是统一的emr-cluster，所以需要订正。

        订正的最佳方式是先导出数据

        ```
        mysqldump --databases hivemeta --single-transaction -u root –p > hive_databases.sql
        ```

        用sed替换hdfs://oldcluster:8020/为hdfs://emr-cluster/ ,再导入新db中。

        ```
        mysql hivemeta -p < hive_databases.sql
        ```

    4.  在新集群的界面上，停止掉hivemetastore服务；
    5.  登陆新的元数据库，create database创建数据库；
    6.  在新的元数据库中，导入替换location字段之后的老元数据库导出来的所有数据；
    7.  版本对齐，EMR的hive版本一般是当前社区最新的稳定版，自建集群hive版本可能会更老，所以导入的旧版本数据可能不能直接使用。需要执行hive的升级脚本（期间会有表、字段已存在的问题可以忽略），可以参见[hive升级脚本](https://github.com/apache/hive/tree/master/metastore/scripts/upgrade/mysql)。例如hive从1.2升级到2.3.0，需要依次执行upgrade-1.2.0-to-2.0.0.mysql.sql，upgrade-2.0.0-to-2.1.0.mysql.sql，upgrade-2.1.0-to-2.2.0.mysql.sql，upgrade-2.2.0-to-2.3.0.mysql.sql。脚本主要是建表，加字段，改内容，如有表已存在，字段已存在的异常可以忽略。
    8.  meta数据全部订正后，就可以重启metaserver了。命令行输入hive，查询库和表，查询数据，验证正确性。

## Flume数据迁移 {#section_dfr_fly_wfb .section}

-   Flume双写配置

    在新集群上也开启flume服务，并且将数据按照和老集群完全一致的规则写入到新集群中。

-   Flume分区表写入

    Flume数据双写，双写时需控制开始的时机，要保证flume在开始一个新的时间分区的时候来进行新集群的同步。 如flume每小时整点会同步所有的表，那就要整点之前，开启flume同步服务，这样flume在一个新的小时内写入的数据，在旧集群和新集群上是完全一致的。而不完整的旧数据在distcp的时候，全量的同步会覆盖它。而开启双写时间点后的新数据，在数据同步的时候不进行同步。 这个新的写入的数据，我们在划分数据阶段，记得不要放到数据同步的目录里。


## 作业同步 {#section_zrh_jly_wfb .section}

Hadoop，Hive，Spark，MR等如果有较大的版本升级，可能涉及作业改造，要视具体情况而定。

常见问题：

-   Gateway OOM

    修改/etc/ecm/hive-conf/hive-env.sh

    export HADOOP\_HEAPSIZE=512 改成1024

-   作业执行内存不足

    mapreduce.map.java.opts调整的是启动 JVM 虚拟机时，传递给虚拟机的启动参数，而默认值 -Xmx200m 表示这个 Java 程序可以使用的最大堆内存数，一旦超过这个大小，JVM 就会抛出 Out of Memory 异常，并终止进程

    `set mapreduce.map.java.opts=-Xmx3072m`

    mapreduce.map.memory.mb设置的是 Container 的内存上限，这个参数由 NodeManager 读取并进行控制，当 Container 的内存大小超过了这个参数值，NodeManager 会负责 kill 掉 Container。

    `set mapreduce.map.memory.mb=3840`


## 数据校验 {#section_alv_sly_wfb .section}

由客户自行抽检报表完成

## Presto集群迁移 {#section_csb_5ly_wfb .section}

如果有单独的Presto集群仅仅用来做数据查询，需要修改Hive中配置文件，请参见[Presto文档](https://prestodb.io/docs/current/connector/hive.html)。

需要修改hive.properties：

-   connector.name=hive-hadoop2
-   hive.metastore.uri=thrift://emr-header-1.cluster-500148414:9083
-   hive.config.resources=/etc/ecm/hadoop-conf/core-site.xml, /etc/ecm/hadoop-conf/hdfs-site.xml
-   hive.allow-drop-table=true
-   hive.allow-rename-table=true
-   hive.recursive-directories=true

## 附录 {#section_ubq_1my_wfb .section}

Hive 1.2升级到2.3的版本对齐示例：

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
  CQ_START bigint,
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

