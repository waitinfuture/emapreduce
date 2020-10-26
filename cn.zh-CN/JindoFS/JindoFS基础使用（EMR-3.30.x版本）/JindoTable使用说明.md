# JindoTable使用说明

JindoTable提供表或分区级别的热度统计、存储分层和表文件优化的功能。本文为您介绍JindoTable的使用方法。

-   本地安装了Java JDK 8。
-   已创建EMR-3.30.0或后续版本的集群，详情请参见[创建集群](/cn.zh-CN/集群管理/集群配置/创建集群.md)。

## 使用JindoTable

常见命令如下：

-   [-accessStat](#section_8d6_eqt_law)
-   [-cache](#section_wkj_6g2_1jn)
-   [-archive](#section_gib_fjk_71m)
-   [-unarchive](#section_08s_0ja_jpy)
-   [-uncache](#section_z79_vix_eme)
-   [-status](#section_wk2_a1s_bqa)
-   [-optimize](#section_o50_ol1_75w)
-   [-showTable](#section_2v1_fit_fql)
-   [-showPartition](#section_4to_c3g_s9n)
-   [-listTables](#section_p6w_t9n_xis)
-   [-dumpmc](#section_orx_5fi_0pl)

**说明：** 指定表时使用`database.table`的格式，指定分区时使用`partitionCol1=1,partitionCol2=2,...`的格式。

## -accessStat

-   语法

    jindo table -accessStat \{-d\}<days\>\{-n\} <topNums\>

-   功能

    查询在指定时间范围内访问最多表或分区的条数。

    <days\>和<topNums\>应为正整数。天数为1时，表示查询从本地时间当天0:00开始到现在的所有访问记录。

-   示例：查询近七天访问最多的表和分区的20条访问记录。

    ```
    jindo table -accessStat -d 7 -n 20
    ```


## -cache

-   语法

    jindo table -cache \{-t\} <dbName.tableName\> \[-p\] <partitionSpec\> \[-pin\]

-   功能

    表示缓存指定表或分区的数据至集群本地磁盘上。

    表或分区的路径需要位于OSS或JindoFS。指定表时使用`database.table`的格式，指定分区时使用`partitionCol1=1,partitionCol2=2,...`的格式。指定`-pin`时，在缓存空间不足时尽量不删除相关数据。

-   示例：缓存2020-03-16日db1.t1表的数据至本地磁盘上。

    ```
    jindo table -cache -t db1.t1 -p date=2020-03-16
    ```


## -uncache

-   语法

    jindo table -uncache \{-t\} <dbName.tableName\> \[-p\] <partitionSpec\>

-   功能

    表示删除集群本地磁盘上指定表或分区的缓存数据。

    对应的路径需要位于OSS或JindoFS。指定表时使用`database.table`的格式，指定分区时使用`partitionCol1=1,partitionCol2=2,...`的格式。

-   示例：
    -   删除集群本地磁盘上表db1.t2的缓存数据。

        ```
        jindo table -cache -t db1.t2
        ```

    -   删除集群本地磁盘上表db1.t1的缓存数据。

        ```
        jindo table -uncache -t db1.t1 -p date=2020-03-16,category=1
        ```


## -archive

-   语法

    jindo table -archive \{-a\|i \} \{-t\} <dbName.tableName\> \[-p\] <partitionSpec\>

-   功能

    表示降低表或者分区的存储策略级别，默认改为归档存储。

    加上-i使用低频存储。指定表时使用database.table的格式，指定分区时使用\`partitionCol1=1,partitionCol2=2,...\`的格式。

-   示例：指定表db1.t1缓存至本地磁盘上。

    ```
    jindo table -archive -t db1.t1 -p date=2020-10-12
    ```


## -unarchive

-   语法

    jindo table -archive \[-o\|-i\] \{-t\} <dbName.tableName\> \[-p\] <partitionSpec\>

-   功能

    表示将归档数据转为标准存储。

    `-o`将归档数据临时解冻，`-i`将归档数据转为低频。


## -status

-   语法

    jindo table -status \{-t\} <dbName.tableName\> \[-p\] <partitionSpec\>

-   功能

    表示查看指定表或者分区的存储状态。

-   示例：
    -   查看表db1.t2的状态。

        ```
        jindo table -status -t db1.t2
        ```

    -   查看表db1.t1在2020-03-16日的状态。

        ```
        jindo table -status -t db1.t1 -p date=2020-03-16
        ```


## -optimize

-   语法

    jindo table -optimize \{-t\} <dbName.tableName\>

-   功能

    优化表在存储层的数据组织。

-   示例：优化表db1.t1在存储层的数据组织。

    ```
    jindo table -optimize -t db1.t1
    ```


## -showTable

-   语法

    jindo table -showTable \{-t\} <dbName.tableName\>

-   功能

    如果是分区表，则展示所有分区；如果是非分区表，则返回表的存储情况。

-   示例：展示db1.t1分区表的所有分区。

    ```
    jindo table -showTable -t db1.t1
    ```


## -showPartition

-   语法

    jindo table -showPartition \{-t\} <dbName.tableName\> \[-p\] <partitionSpec\>

-   功能

    表示返回分区的存储情况。

-   示例：返回分区表db1.t1在2020-10-12日的存储情况。

    ```
    jindo table -showPartition -t db1.t1 -p date=2020-10-12
    ```


## -listTables

-   语法

    jindo table -listTables \[-db\] <dbName.tableName\>

-   功能

    展示指定数据库中的所有表。不指定`[-db]`时默认展示default库中的表。

-   示例：
    -   展示default库中的表。

        ```
        jindo table -listTables
        ```

    -   列出数据库db1中的表。

        ```
        jindo table -listTables -db db1
        ```


## -dumpmc

-   语法

    jindo table -dumpmc\{-i\} <accessId\> \{-k\} <accessKey\> \{-m\} <numMaps\> \{-t\} <tunnelUrl\> \{-project\} <projectName\> \{-table\} <tablename\> \{-p\} <partitionSpec\> \{-f\} <csv\|tfrecord\> \{-o\} <outputPath\>

    |参数|描述|是否必选|
    |--|--|----|
    |-i|阿里云的AccessKey ID。|是|
    |-k|阿里云的AccessKey Secret。|是|
    |-m|map任务数。|是|
    |-t| |是|
    |-project|Maxcompute的项目空间名。|是|
    |-table|Maxcompute的表名。|是|
    |-p|分区信息 。例如`pt=xxx`，多个分区表时用英文逗号（,）分开`pt=xxx,dt=xxx`。|是|
    |-f|文件格式。包括：    -   tfrecord
    -   csv
|是|
    |-o|目的路径。|是|

-   功能

    表示Dumpmc Maxcompute表至EMR集群或OSS存储。支持CSV格式和TFRECORD格式。

-   示例：
    -   Dumpmc Maxcompute表（TFRECORD格式）至EMR集群。

        ```
        jindo table -dumpmc -m 10 -project mctest_project -table t1 -t http://dt.xxx.maxcompute.aliyun-inc.com -k xxxxxxxxx -i XXXXXX -o /tmp/outputtf1 -f tfrecord
        ```

    -   Dumpmc Maxcompute表（CSV格式）至OSS存储。

        ```
        jindo table -dumpmc -m 10 -project mctest_project -table t1 -t http://dt.xxx.maxcompute.aliyun-inc.com -k xxxxxxxxx -i XXXXXX -o oss://bucket1/tmp/outputcsv -f csv
        ```


