# 使用Raft-RocksDB-Tablestore作为存储后端

JindoFS在EMR-3.27.0及之后版本中支持使用Raft-RocksDB-OTS作为Jindo元数据服务（Namespace Service）的存储。1个EMR JindoFS集群创建3个Master节点组成1个Raft实例，实例的每个Peer节点使用本地RocksDB存储元数据信息。

-   创建Tablestore实例，推荐使用高性能实例，详情请参见[创建实例](/cn.zh-CN/快速入门/创建实例.md)。

    **说明：** 需要开启事务功能。

-   创建3 Master的EMR集群，详情请参见[创建集群](/cn.zh-CN/集群管理/集群配置/创建集群.md)。

    ![3 Master](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/6257459951/p102535.png)

    **说明：** 如果没有部署方式，请[提交工单](https://selfservice.console.aliyun.com/ticket/createIndex?spm=5176.2020520129.103.2.9Z8xg7)处理。


RocksDB通过Raft协议实现3个节点之间的复制。集群可以绑定1个Tablestore（OTS）实例，作为Jindo的元数据服务的额外存储介质，本地的元数据信息会实时异步地同步到用户的Tablestore实例上。

元数据服务-多机Raft-RocksDB-Tablestore+HA如下图所示。

![Raft + RocksDB + Tablestore](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/6257459951/p102531.png)

## 配置本地raft后端

1.  新建EMR集群后，暂停SmartData所有服务。

    1.  登录[阿里云E-MapReduce控制台](https://emr.console.aliyun.com/)。

    2.  在顶部菜单栏处，根据实际情况选择地域（Region）和资源组。

    3.  单击上方的**集群管理**页签。

    4.  在**集群管理**页面，单击相应集群所在行的**详情**。

    5.  在左侧导航栏，单击**集群服务** \> **SmartData**。

    6.  单击右上角的**操作** \> **停止 All Components**。

2.  根据使用需求，添加需要的namespace。

3.  进入SmartData服务的**namespace**页签。

    1.  在左侧导航栏，单击**集群服务** \> **SmartData**。

    2.  单击**配置**页签。

    3.  在**服务配置**区域，单击**namespace**页签。

4.  在SmartData服务的**namespace**页签，设置如下参数。

    |参数|描述|示例|
    |--|--|--|
    |**namespace.backend.type**|设置namespace后端存储类型，支持：     -   rocksdb
    -   ots
    -   raft
默认为rocksdb。

|raft|
    |**namespace.backend.raft.initial-conf**|部署raft实例的3个Master地址（固定值）。|emr-header-1:8103:0,emr-header-2:8103:0,emr-header-3:8103:0|
    |**jfs.namespace.server.rpc-address**|Client端访问raft实例的3个Master地址（固定值）|emr-header-1:8101,emr-header-2:8101,emr-header-3:8101|

    **说明：** 如果不需要使用OTS远端存储，直接执行[步骤6](#step_6)和[步骤7](#step_7)；如果需要使用OTS远端存储，请执行[步骤5](#step_5)~[步骤7](#step_7)。

5.  配置远端OTS异步存储。

    在SmartData服务的**namespace**页签，设置如下参数。

    |参数|参数说明|示例|
    |--|----|--|
    |**namespace.ots.instance**|Tablestore实例名称。|emr-jfs|
    |**namespace.ots.accessKey**|Tablestore实例的AccessKey ID。|kkkkkk|
    |**namespace.ots.accessSecret**|Tablestore实例的AccessKey Secret。|XXXXXX|
    |**namespace.ots.endpoint**|Tablestore实例的endpoint地址，通常EMR集群，推荐使用VPC地址。|http://emr-jfs.cn-hangzhou.vpc.tablestore.aliyuncs.com|
    |**namespace.backend.raft.async.ots.enabled**|是否开启OTS异步上传，包括：     -   true
    -   false
当设置为true时，需要在SmartData服务完成初始化前，开启OTS异步上传功能。

**说明：** 如果SmartData服务已完成初始化，则不能再开启该功能。因为OTS的数据已经落后于本地RocksDB的数据。

|true|

6.  保存配置。

    1.  单击右上角的**保存**。

    2.  在**确认修改**对话框中，输入执行原因，开启**自动更新配置**。

    3.  单击**确定**。

7.  单击右上角的**操作** \> **启动 All Components**。


## 从Tablestore恢复元数据信息

如果您在原始集群开启了远端Tablestore异步存储，则Tablestore上会有1份完整的JindoFS元数据的副本。您可以在停止或释放原始集群后，在新创建的集群上恢复原先的元数据，从而继续访问之前保存的文件。

1.  准备工作。

    1.  统计原始集群的元数据信息（文件和文件夹数量）。

        ```
        [hadoop@emr-header-1 ~]$ hadoop fs -count jfs://test/
                1596      1482809                 25 jfs://test/
            (文件夹个数） （文件个数）
        ```

    2.  停止原始集群的作业，等待30~120秒左右，等待原始集群的数据已经完全同步到Tablestore。执行以下命令查看状态。如果LEADER节点显示`_synced=1`，则表示Tablestore为最新数据，同步完成。

        ```
        jindo jfs -metaStatus -detail
        ```

        ![查看状态](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/6257459951/p102586.png)

    3.  停止或释放原始集群，确保没有其它集群正在访问当前的Tablestore实例。

2.  创建新集群。

    新建与Tablestore实例相同Region的EMR集群，暂停SmartData所有服务。详情请参见[配置本地raft后端](#section_08i_5b4_zxl)。

3.  初始化配置。

    在SmartData服务的**namespace**页签，设置以下参数。

    |参数|描述|示例|
    |--|--|--|
    |**namespace.backend.raft.async.ots.enabled**|是否开启OTS异步上传，包括：     -   true
    -   false
|false|
    |**namespace.backend.raft.recovery.mode**|是否开启从OTS恢复元数据，包括：     -   true
    -   false
|true|

4.  保存配置。

    1.  单击右上角的**保存**。

    2.  在**确认修改**对话框中，输入执行原因，开启**自动更新配置**。

    3.  单击**确定**。

5.  单击右上角的**操作** \> **启动 All Components**。

6.  新集群的SmartData服务启动后，自动从OTS恢复元数据到本地Raft-RocksDB上，可以通过以下命令查看恢复进度。

    ```
    jindo jfs -metaStatus -detail
    ```

    如图所示，LEADER节点的state为FINISH表示恢复完成。

    ![查看状态](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/7257459951/p102577.png)

7.  执行以下操作，可以比较一下文件数量与原始集群是否一致。

    此时的集群为恢复模式，也是只读模式。

    ```
    # 对比文件数量一致
    [hadoop@emr-header-1 ~]$ hadoop fs -count jfs://test/
            1596      1482809                 25 jfs://test/
    
    # 文件可正常读取(cat、get命令)
    [hadoop@emr-header-1 ~]$ hadoop fs -cat jfs://test/testfile
    this is a test file
    # 查看目录
    [hadoop@emr-header-1 ~]$ hadoop fs -ls jfs://test/
    Found 3 items
    drwxrwxr-x   - root   root            0 2020-03-25 14:54 jfs://test/emr-header-1.cluster-50087
    -rw-r-----   1 hadoop hadoop          5 2020-03-25 14:50 jfs://test/haha-12096RANDOM.txt
    -rw-r-----   1 hadoop hadoop         20 2020-03-25 15:07 jfs://test/testfile
    
    # 只读状态，不可修改文件
    [hadoop@emr-header-1 ~]$ hadoop fs -rm jfs://test/testfile
    java.io.IOException: ErrorCode : 25021 , ErrorMsg: Namespace is under recovery mode, and is read-only.
    ```

8.  修改配置，将集群设置为正常模式，开启OTS异步上传功能。

    在SmartData服务的**namespace**页签，设置以下参数。

    |参数|描述|示例|
    |--|--|--|
    |**namespace.backend.raft.async.ots.enabled**|是否开启OTS异步上传，包括：     -   true
    -   false
|true|
    |**namespace.backend.raft.recovery.mode**|是否开启从OTS恢复元数据，包括：     -   true
    -   false
|false|

9.  重启集群。

    1.  单击上方的**集群管理**页签。

    2.  在**集群管理**页面，单击相应集群所在行的**更多** \> **重启**。


