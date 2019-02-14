# E-MapReduce 基本问题 {#concept_bw2_5s1_pfb .concept}

## Q：作业和执行计划的区别 {#section_ybr_zs1_pfb .section}

A：在阿里云 E-MapReduce 中，要运行作业，需要有分成两个步骤，分别是：

-   创建作业

    在 E-MapReduce 产品中，说创建一个作业，实际上是创建一个作业运行配置，它并不能被直接运行。既如果在 E-MapReduce 中创建了一个作业，实际上只是创建了一个作业如何运行的配置，这份配置中包括该作业要运行的 jar 包，数据的输入输出地址，以及一些运行参数。这样的一份配置创建好后，给它命一个名，既定义了一个作业。当你需要调试运行作业的时候就需要执行计划了。

-   创建执行计划

    执行计划，是将作业与集群关联起来的一个纽带。通过它，我们可以把多个作业组合成一个作业序列，通过它我们可以为作业准备一个运行集群（或者自动创建出一个临时集群或者关联一个已存在的集群），通过它我们可以为这个作业序列设置周期执行计划，并在完成任务后自动释放集群。我们也可以在他的执行记录列表上查看每一次执行的执行成功情况与日志。


## Q：如何查看作业日志 {#section_cgv_jt1_pfb .section}

A：在 E-MapReduce 系统里，系统已经将作业运行日志按照 JobID 的规划上传到 OSS 中（路径由用户在创建集群时设置），用户可以直接在网页上点击查看作业日志。如果用户是登录到 Master 机器进行作业提交和脚本运行等，则日志根据用户自己的脚本而定，用户可以自行规划。

## Q：如何登录 Core 节点 {#section_bf4_nt1_pfb .section}

A：按照如下步骤：

1.  首先在 Master 节点上切换到 Hadoop 账号：

    ```
    su hadoop
    ```

2.  然后即可免密码 SSH 登录到对应的 Core 节点：

    ```
    ssh emr-worker-1
    ```

3.  通过 sudo可以获得 root 权限：

    ```
    sudo vi /etc/hosts
    ```


## Q：直接在OSS上查看日志 {#section_nvk_xt1_pfb .section}

A：用户也可以直接从 OSS 上查找所有的日志文件，并下载。但是因为 OSS 不能直接查看日志，使用起来会比较麻烦一些。如果用户创建集群时打开了运行日志功能，并且指定了一个 OSS 的日志位置，那么作业的日志要如何找到呢？例如对下面这个保存位置OSS://mybucket/emr/spark：

1.  首先来到执行计划的页面，找到对应的执行计划，单击**运行记录**进入运行记录页面。
2.  在运行记录页面找到具体的哪一条执行记录，比如最后的一条执行记录。然后单击它对应的**执行集群**查看这个执行集群的 ID。
3.  然后再OSS://mybucket/emr/spark目录下寻找OSS://mybucket/emr/spark/集群ID 这个目录
4.  在OSS://mybucket/emr/spark/clusterID/jobs 目录下会按照作业的执行 ID 存放多个目录，每一个目录下存放了这个作业的运行日志文件。

## Q：集群、执行计划以及运行作业的计时策略 {#section_olm_s51_pfb .section}

A：三种计时策略如下：

-   集群的计时策略

    在集群列表里可以看到每个集群的运行时间，该运行时间的计算策略为 运行时间 = 集群释放时刻 - 集群开始构建时刻。即集群一旦开始构建就开始计时，直到集群的生命周期结束。

-   执行计划的计时策略

    在执行计划的运行记录列表，可以看到每次执行记录运行的时间，该时间的计时策略总结为两种情况：

    -   如果执行计划是按需执行的，每次执行记录的运行过程涉及到创建集群、提交作业运行、释放集群。所以按需执行计划的运行时间计算策略为，运行时间 = 构建集群的时间 + 执行计划包含所有作业全部运行结束的总耗时 + 集群释放的时间。
    -   如果执行计划是关联已有集群运行的，整个运行周期不涉及到创建集群和释放集群，所以其运行时间 = 执行计划包含所有作业全部运行结束的总耗时。
-   作业的计时策略

    这里的作业指的是被挂载到执行计划里面的作业。在每条执行计划运行记录右侧的**查看作业列表**点击进去可以查看到该执行作计划下作业列表。这里每个作业的运行时间的计算策略为，运行时间 = 作业运行结束的实际时间 - 作业开始运行的实际时间。作业运行开始（结束\)的实际时间指的是作业被 Spark 或 Hadoop 集群实际开始调度运行或运行结束的时间点。


## Q：第一次使用执行计划时没有安全组可选 {#section_lzn_cv1_pfb .section}

A：因为一些安全的原因，EMR 目前的安全组并不能直接选择用户的已有安全组来使用，所以如果你还没有在 EMR 中创建过安全组的话，在执行计划上将无法选择到可用的安全组。我们推荐您先手动创建一个按需集群来进行作业的测试，手动创建集群的时候可以创建一个新的 EMR 安全组，等到测试都通过了以后，再设置您的执行计划来周期调度。这个时候之前创建的安全组也会出现在这里可供选择。

## Q：读写 MaxCompute时，抛出java.lang.RuntimeException.Parse responsed failed: ‘<!DOCTYPE html\>…’ {#section_kdm_dv1_pfb .section}

A：检查 ODPS tunnel endpoint是否正确，如果写错会出现这个错误。

## Q：多个 ConsumerID 消费同一个 Topic 时出现 TPS 不一致问题 {#section_az5_2v1_pfb .section}

A：有可能这个 Topic 在公测或其他环境创建过，导致某些 Consumer 组消费数据不一致。请在工单系统中将对应的 Topic 和 ConsumerID 提交到 ONS 处理。

## Q：E-MapReduce 中能否查看作业的 Worker 上日志 {#section_lq2_kv1_pfb .section}

A：可以。前置条件：是创建集群时打开**运行日志**选项。查看日志位置：**执行计划列表** \> **更多** \> **运行记录** \> **运行记录** \> **查看作业列表** \> **作业列表** \> **作业实例**。

## Q：Hive 创建外部表，没有数据 {#section_ydh_cw1_pfb .section}

A：例如：

```
CREATE EXTERNAL TABLE storage_log(content STRING) PARTITIONED BY (ds STRING)
    ROW FORMAT DELIMITED
    FIELDS TERMINATED BY '\t'
    STORED AS TEXTFILE
    LOCATION 'oss://log-124531712/biz-logs/airtake/pro/storage'; 
    hive> select * from storage_log;
    OK
    Time taken: 0.3 seconds
    创建完外部表后没有数据
```

实际上 Hive 并不会自动关联指定目录的 partitions 目录，您需要手动操作，例如：

```
alter table storage_log add partition(ds=123);                                                                                                                                             OK
    Time taken: 0.137 seconds
    hive> select * from storage_log;
    OK
    abcd    123
    efgh    123
```

## Q：Spark Streaming 作业运行一段时间后无故结束 {#section_rqz_2w1_pfb .section}

A：首先检查 Spark 版本是否是 1.6 之前版本。Spark 1.6 修复了一个内存泄漏的 BUG，这个 BUG 会导致 container 内存超用然后被kill掉（当然，这只是可能的原因之一，不能说明Spark 1.6 不存在任何问题）。此外，检查自己的代码在使用内存上有没有做好优化。

## Q：Spark Streaming 作业已经结束，但是 E-MapReduce 控制台显示作业还处于“运行中”状态 {#section_zj5_fw1_pfb .section}

A：检查 Spark Streaming 作业的运行模式是否是 yarn-client，若是建议改成 yarn-cluster 模式。E-MapReduce 对 yarn-client 模式的 Spark Streaming 作业的状态监控存在问题，会尽快修复。

## **Q："Error: Could not find or load main class"** {#section_wql_kw1_pfb .section}

A：检查作业配置中作业jar包的路径协议头是否是 `ossref`，若不是请改为 `ossref`。

## Q：集群机器分工使用说明 {#section_x13_nw1_pfb .section}

A：E-MapReduce 中包含一个 Master 节点和多个 Slave（或者 Worker）节点。其中 Master 节点不参与数据存储和计算任务，Slave 节点用来存储数据和计算任务。例如 3 台 4 核 8 G 机型的集群，其中一台机器用来作为 Master 节点，另外两台用来作为 Slave 节点，也就是集群的可用计算资源为 2 台 4 核 8 G 机器。

## Q：如何在 MR 作业中使用本地共享库 {#section_pcy_nw1_pfb .section}

A：方法有很多，这里给出一种方式。修改 mapred-site.xml 文件，例如：

```
<property>  
    <name>mapred.child.java.opts</name>  
    <value>-Xmx1024m -Djava.library.path=/usr/local/share/</value>  
  </property>  
  <property>  
    <name>mapreduce.admin.user.env</name>  
    <value>LD_LIBRARY_PATH=$HADOOP_COMMON_HOME/lib/native:/usr/local/lib</value>  
  </property>
```

只要加上你所需的库文件即可。

## Q：如何在 MR/Spark 作业中指定 OSS 数据源文件路径 {#section_zrk_qw1_pfb .section}

A： 如下：`OSS URL： oss://[accessKeyId:accessKeySecret@]bucket[.endpoint]/object/path`

用户在作业中指定输入输出数据源时使用这种 URI，可以类比`hdfs://`。 用户操作 OSS 数据时：

-   （建议）EMR 提供了 MetaService 服务，支持免 AK 访问 OSS 数据，直接写oss://bucket/object/path。
-   （不建议）可以将 AccessKeyId，AccessKeySecret 以及 endpoint 配置到Configuration（Spark 作业是 SparkConf，MR 类作业是 Configuration）中，也可以在 URI中直接指定 AccessKeyId，AccessKeySecret 以及 endpoint。具体请参考[开发准备](../../../../../intl.zh-CN/开发指南/准备/OSS 参考使用说明.md#)一节。

## Q：Spark SQL抛出“Exception in thread “main” java.sql.SQLException: No suitable driver found for jdbc:mysql:xxx”报错 {#section_sy1_ww1_pfb .section}

A：

-   低版本 mysql-connector-java 有可能出现类似问题，更新到最新版本。
-   作业参数中使用 —driver-class-path ossref://bucket/…/mysql-connector-java-\[version\].jar 来加载 mysql-connector-java 包，直接将 mysql-connector-java 打进作业 jar 包也会出现上述问题。

## Q：Spark SQL连RDS出现“Invalid authorization specification, message from server: ip not in whitelist” {#section_uw3_zw1_pfb .section}

A：检查 RDS 的白名单设置，将集群机器的内网地址加到 RDS 的白名单中。

## Q : 创建低配置机型集群注意事项 {#section_vgj_1x1_pfb .section}

A ：

-   若 Master 节点选择 2 核 4 G 机型，则 Master 节点内存非常吃紧，很容易造成物理内存不够用，建议调大Master内存。
-   若 Slave节点选择 2 核 4 G 机型，在运行 MR 作业或者 Hive 作业时，请调节参数。MR 作业添加参数-D yarn.app.mapreduce.am.resource.mb=1024；Hive 作业设置参数 set yarn.app.mapreduce.am.resource.mb=1024; 避免作业挂起。

## Q：Hive/Impala 作业读取 SparkSQL 导入的 Parquet 表报错\(表包含 Decimal 格式的列\)：Failed with exception java.io.IOException:org.apache.parquet.io.ParquetDecodingException: Can not read value at 0 in block -1 in file hdfs://…/…/part-00000-xxx.snappy.parquet {#section_m52_hx1_pfb .section}

A ： 由于 Hive 和 SparkSQL 在 Decimal 类型上使用了不同的转换方式写入 Parquet，导致 Hive 无法正确读取 SparkSQL 所导入的数据。对于已有的使用 SparkSQL 导入的数据，如果有被 Hive/Impala 使用的需求，建议加上 spark.sql.parquet.writeLegacyFormat=true，重新导入数据。

## Q:beeline 如何访问Kerberos 安全集群 {#section_y2c_2gp_wfb .section}

A：

-   HA 集群\(Discovery 模式\)

    ```
    
    !connect jdbc:hive2://emr-header-1:2181,emr-header-2:2181,emr-header-3:2181/;serviceDiscoveryMode=zooKeeper;zooKeeperNamespace=hiveserver2;principal=hive/_HOST@EMR.${clusterId).COM
    ```

-   HA 集群\(直连某台机器\)

    连 emr-header-1

    ```
    !connect jdbc:hive2://emr-header-1:10000/;principal=hive/emr-header-1@EMR.${clusterId}.COM
    ```

    连 emr-header-2

    ```
    !connect jdbc:hive2://emr-header-2:10000/;principal=hive/emr-header-2@EMR.${clusterId}.COM
    ```

-   非 HA 集群

    ```
    !connect jdbc:hive2://emr-header-1:10000/;principal=hive/emr-header-1@EMR.${clusterId}.COM
    ```


## Q：ThriftServer 进程正常，但链接出现异常，报错Connection refused telnet emr-header-1 10001 无法连接 {#section_o55_2yd_ggb .section}

A：

可以查看 /mnt/disk1/log/spark 日志

该问题是由于 thrift server oom，需要调大内存，调大 spark.driver.memory 值即可。

。

