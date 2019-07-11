# Hive 元数据管理 {#concept_ukp_lxk_z2b .concept}

从 E-MapReduce-2.4.0（以下简称 EMR） 版本开始，E-MapReduce 支持了统一元数据管理，在 E-MapReduce-2.4.0 版本之前，用户所有集群均采用的是集群本地的 mysql 数据库作为 Hive 元数据库，在 E-MapReduce-2.4.0 版本以及之后的版本中， E-MapReduce 会支持统一的高可靠的 Hive 元数据库。

## 介绍 {#section_fkz_hyk_z2b .section}

![hive元数据库](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17932/156282841011067_zh-CN.png)

有了统一的元数据管理之后，就可以实现：

-   提供持久化的元数据存储

    之前元数据都是在集群内部的 mysql 数据库，元数据会随着集群的释放而丢失，特别是 EMR 提供了灵活按量模式，集群可以按需创建用完就释放。如果用户需要保留现有的元数据信息，必须登录上集群手动将元数据信息导出。支持统一的元数据管理之后，不再存在该问题。

-   能更方便地实现计算存储分离

    EMR 上可以支持将数据存放在阿里云 OSS 中，在大数据量的情况下将数据存储在 OSS 上会大大降低使用的成本，EMR 集群主要用来作为计算资源，在计算完成之后机器可以随时释放，数据在 OSS 上，同时也不用再考虑元数据迁移的问题。

-   更方便地实现数据共享

    使用统一的元数据库，如果用户的所有数据都存放在 OSS 之上，则不需要做任何元数据的迁移和重建所有集群都是可以直接访问数据，这样每个 EMR 集群可以做不同的业务，但是可以很方便地实现数据的共享。


**说明：** 

在支持统一元数据之前，元数据是存储在每个集群本地环境的 Mysql 数据库中，所以元数据会随着集群的释放消亡。在支持统一元数据之后，释放集群不会清理元数据信息。所以，在任何时候删除 OSS 上的数据或者集群 HDFS 上的数据（包括释放集群操作）的时候，需要先确认该数据对应的元数据已经删除（即要 drop 掉数据对应的表和数据库）。否则元数据库中可能出现一些脏的元数据信息。

## 创建使用统一元数据的集群 {#section_rpg_fgj_wgb .section}

-   页面方式

    创建集群时，在基础配置页面打开**统一 meta 数据库**开关。

-   API 方式

    参考 API 文档中的 [CreateClusterV2](../../../../intl.zh-CN/API 参考/集群/CreateClusterV2.md#) 定义，请指定此选项：useLocalMetaDb=false。


## 元数据基本操作 {#section_t2k_hhj_wgb .section}

-   前提条件

    EMR 控制台统一元数据管理界面，当前只对使用统一元数据的集群生效，且必须有活跃中的统一元数据集群。

-   数据库操作

    可根据库名查找库，单击库名可查看当前库的所有表。

    **说明：** 删除数据库之前必须删除数据库下所有表。

-   表操作

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17932/156282841011078_zh-CN.png)

    表创建：当前仅支持外部表创建和分区表创建。分隔符不仅支持普通的逗号、空格等字符，还支持四种特殊字符：TAB、^A、^B 和 ^C，或者自定义分隔符。

-   表详情

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17932/156282841111079_zh-CN.png)

    分区查看：对于分区表，可以在**表详情** \> **分区信息**页面查看分区列表，最多显示 10000 个分区。

    **说明：** 

    -   如果没有任何 EMR 集群，不支持进行对数据库和表的创建和删除操作。
    -   由于 HDFS 是每个集群内部文件系统，在没有进行特殊的网络环境设置的情况下，不同集群之间 的HDFS 无法相互访问的，所以 EMR 表管理功能对数据库和表的创建只支持基于 OSS 文件系统。
    -   数据库和表的 location 都不能选择整个 OSS bucket，需要选择到 OSS bucket 下面的目录。
-   元数据库信息

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17932/156282841111080_zh-CN.png)

    可以在元数据库信息页查看当前 RDS 的用量和使用限制，如需修改，请提交工单联系我们。


## 迁移 {#section_b2g_nkj_wgb .section}

-   从 EMR 统一元数据库迁出到用户自建 RDS
    1.  购买 RDS 实例，并保证 RDS 可以和集群的 master 节点网络互通，建议跟E-MapReduce 的 ECS 在同一个安全组内，这样可以直接使用 RDS 的内网地址；

        **说明：** 出于安全考虑，需要对 RDS 的 IP 白名单进行设置，设置只允许对应的 EMR 机器可以访问;

    2.  在 RDS 中创建一个 Database，命名为 hivemeta，同时创建一个用户，把 hivemeta 的读写权限赋给这个用户;
    3.  导出统一元数据库的内容（只导出数据，不用导表结构），为了保证数据的一致性，在执行这一步操作之前，最好将 hive 的 metastore 服务停掉，保证导出期间不会有新的 meta 数据变化。
    4.  登录[阿里云 E-MapReduce 控制台](https://emr.console.aliyun.com/)，单击上方**集群管理**页签进入集群管理页面。
    5.  单击对应集群 ID 进入集群与服务管理页面，在服务列表中依次单击**Hive** \> **配置**进入 Hive 管理页面，找到以下几个配置项的值（如果是不支持配置管理的老版本集群，在 $HIVE\_CONF\_DIR/hive-site.xml 中查找）：

        ``` {#codeblock_i6l_rof_1ij}
        javax.jdo.option.ConnectionUserName //对应数据库用户名;
        javax.jdo.option.ConnectionPassword //对应数据库访问密码;
        javax.jdo.option.ConnectionURL //对应数据库访问地址和库名;
        ```

    6.  使用 [SSH](intl.zh-CN/集群规划与配置/集群配置/SSH 登录集群.md#) 方式登录到集群 master 节点，执行以下命令：

        ``` {#codeblock_kji_70e_5hx}
        mysqldump　-t　hivemeta -h <统一元数据库地址>　-u <统一元数据库用户名>　-p　>　/tmp/metastore.sql
        ```

        密码就是上面提到的配置中的对应数据库访问密码。

    7.  修改集群 master 节点（如果是 HA 集群则两个 master 都需要）上的/usr/local/emr/emr-agent/run/meta\_db\_info.json，将文件中的use\_local\_meta\_db 设置为 false，meta 数据库信的链接地址、用户名和密码设置成 RDS 的信息。
    8.  在 Hive 管理页面，把 meta 数据库信的链接地址、用户名和密码换成新 RDS 的信息；如果是不支持配置管理的老版本集群，修改 $HIVE\_CONF\_DIR/hive-site.xml 中的对应的配置为需要连接的数据库。
    9.  在一台 master 节点上，把hive-site.xml中的 meta 数据库链接地址、用户名和密码换成 RDS 的信息，然后执行 init schema:

        ``` {#codeblock_j2g_ght_76c}
        cd /usr/lib/hive-current/bin
        ./schematool -initSchema -dbType mysql
        ```

    10. 将之前导出来的 meta 数据导入 RDS。命令行登陆 mysql ：

        ``` {#codeblock_smf_0mj_iw9}
        mysql -h {rds的url} -u {rds的用户名} -p
        ```

        进入 mysql 的命令行之后，执行source /tmp/metastore.sql 正常情况可以完全导入，不会报错。

    11. 在集群与服务管理，重启 Hive 所有组件，单击 **RESTART ALL COMPONENTS**。 验证 Hive功能是否正常，可以在 master 节点上，执行hive cli，查看数据是否与之前一致。

## 常见问题 {#section_jkt_hdl_z2b .section}

-   Wrong FS：oss://yourbucket/xxx/xxx/xxx 或 Wrong FS： hdfs://yourhost:9000/xxx/xxx/xxx 

    出现这个问题，是由于删除 OSS 上的表数据之前，没有删除数据表对应的元数据。导致表的 schema 还在，但实际的数据已不存在或已移动到别的路径。可以先修改表的 location 为一个存在的路径，然后再删除表：

    `alter table test set location 'oss://your_bucket/your_folder'`

    该操作可以直接在EMR控制台的交互式控制台中完成：

    **说明：** oss://your\_bucket/your\_folder必须是一个存在的 OSS 路径。

-   删除 hive database 的时候出现：java.lang.IllegalArgumentException: java.net.UnknownHostException: xxxxxxx

    出现该问题的原因，是因为在之前的集群之上创建了 Hive 的数据库，并且数据库的位置是落在之前集群的 HDFS 之上，但是在集群释放的时候，没有清理掉对应的 hive database，导致新建集群之后，没法访问到之前已经释放集群的 HDFS 数据。所以如果是手动创建了在 HDFS 之上的数据库和表，在释放集群的时候请记得清理。

    解决方案：

    首先，通过命令行登录到集群 master 节点上，找到 hive meta db 的访问地址和用户名密码信息，在$HIVE\_CONF\_DIR/hive-site.xml中，找到对应属性。

    ``` {#codeblock_0tj_9x0_z3m}
    javax.jdo.option.ConnectionUserName //对应数据库用户名;
    javax.jdo.option.ConnectionPassword //对应数据库访问密码;
    javax.jdo.option.ConnectionURL //对应数据库访问地址和库名;
    ```

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17932/156282841111097_zh-CN.png)

    在集群 master 节点上登录 hive meta db:

    ``` {#codeblock_ncr_qtd_r6w}
    mysql -h ${DBConnectionURL} -u ${ConnectionUserName} -p [回车]
    [输入密码]${ConnectionPassword}
    ```

    登录上 hive meta db 之后，修改对应 hive database 的 location 为该 region 真实存在的 OSS 路径即可：

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17932/156282841111102_zh-CN.png)


