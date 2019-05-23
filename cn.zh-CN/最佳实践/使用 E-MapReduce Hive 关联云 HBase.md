# 使用 E-MapReduce Hive 关联云 HBase {#concept_ygm_vfw_mgb .concept}

本文简单介绍如何使用 E-MapReduce Hive 关联云 HBase 的表。云HBase 需要借助外部 Hive 对多表进行关联分析。

**说明：** 后续云 HBase 将集成 Spark，更加建议使用Spark 分析 HBase 数据。

## 准备工作 {#section_cd2_tgw_mgb .section}

-   购买按量计费的 EMR 集群，配置依据实际场景确定，注意云 HBase 要和 EMR 处在同一 VPC 下，建议不开启高可用。
-   将 EMR 所有节点的 IP 加入到云 HBase 白名单。
    -   获取云 HBase 的 Zookeeper 访问地址，可在云 HBase 控制台查看。

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/114385/155860064038367_zh-CN.png)

-   由于云 HBase 的 HDFS 端口默认是不开的，需要联系工作人员开通。

## 实施步骤 {#section_pf2_l3w_mgb .section}

1.  使用 SSH 方式登录到集群，可参考 [SSH 登录集群](../../../../cn.zh-CN/集群规划与配置/集群配置/SSH 登录集群.md#)。
2.  修改 Hive 配置
    -   进入 Hive 配置目录 /etc/ecm/hive-conf/
    -   修改 hbase-site.xml，将 hbase.zookeeper.quorum 的值修改为云 HBase 的 Zookeeper 访问连接:

        ```
        <property>
                   <name>hbase.zookeeper.quorum</name>
                   <value>hb-bp183x4tu8x7qk840-001.hbase.rds.aliyuncs.com,hb-bp1mhyea7754bpigt-002.hbase.rds.aliyuncs.com,hb-bp1mhyea7754bpigt-003.hbase.rds.aliyuncs.com</value>
              </property>
        ```

3.  Hive 中创建云 HBase 表

    如果 HBase 表不存在，可在 Hive 中直接创建云 HBase 关联表

    1.  输入 hive 命令进入 Hive cli 命令行
    2.  创建 HBase 表：

        ```
        CREATE TABLE hive_hbase_table(key int, value string) 
        STORED BY 'org.apache.hadoop.hive.hbase.HBaseStorageHandler'
        WITH SERDEPROPERTIES ("hbase.columns.mapping" = ":key,cf1:val")
        TBLPROPERTIES ("hbase.table.name" = "hive_hbase_table", "hbase.mapred.output.outputtable" = "hive_hbase_table");
        ```

    3.  Hive 中向 HBase 插入数据

        ```
        insert into hive_hbase_table values(212,'bab');
        ```

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/114385/155860064037681_zh-CN.png)

    4.  查看云 HBase 表，HBase 表已创建，数据也已经写入

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/114385/155860064037682_zh-CN.png)

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/114385/155860064037683_zh-CN.png)

    5.  在 HBase 中写入数据，并在 Hive 中查看

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/114385/155860064037684_zh-CN.png)

        在 Hive 中查看：

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/114385/155860064037685_zh-CN.png)

    6.  Hive 删除表，HBase 表也一并删除

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/114385/155860064037686_zh-CN.png)

    查看 HBase 表，报错不存在表：

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/114385/155860064037687_zh-CN.png)

    **说明：** 如果 HBase 表已存在，可在 Hive 中 HBase 外表进行关联，外部表在删除时不影响 HBase 已创建表

    1.  云 HBase 中创建 HBase 表，并 put 测试数据：

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/114385/155860064037688_zh-CN.png)

    2.  Hive 中创建 HBase 外部关联表，并查看数据

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/114385/155860064137689_zh-CN.png)

    3.  删除 Hive 表不影响 HBase 已存在的表：

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/114385/155860064137690_zh-CN.png)

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/114385/155860064137691_zh-CN.png)


## 总结 {#section_f2n_3qw_mgb .section}

Hive 更多操作 HBase 步骤，可参考 [HBaseIntegration](https://cwiki.apache.org/confluence/display/Hive/HBaseIntegration)。如果使用 ECS 自建 MR 集群的Hive 时，操作步骤跟 EMR 操作类似，需要注意的是自建 Hive 的 hbase-site.xml 部分配置项可能与云 HBase 不一致，简单来说网络和端口开放后，只保留 hbase.zookeeper.quorum 即可与云 HBase 进行关联。

## 参考信息 {#section_mlh_jnl_ngb .section}

云栖社区 ： [利用 EMR Hive 关联云 HBase](https://yq.aliyun.com/articles/652464?spm=a2c4e.11163080.searchblog.9.26032ec10hUrSR&do=login&accounttraceid=43f5cf8e-7606-472e-a12f-b5d694893b58&do=login).

