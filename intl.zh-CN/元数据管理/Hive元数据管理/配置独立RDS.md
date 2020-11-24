# 配置独立RDS

本文介绍如何配置独立的阿里云RDS，作为E-MapReduce（简称EMR）集群的元数据。

已购买RDS，详情请参见[创建RDS MySQL实例](/intl.zh-CN/RDS MySQL 数据库/快速入门/创建RDS MySQL实例.md)。

**说明：** 建议**类型**选择**MySQL**的5.7；**系列**选择**高可用版**。

## 元数据库准备

1.  创建hivemeta的数据库。

    详情请参见[创建数据库和账号](/intl.zh-CN/RDS MySQL 数据库/快速入门/创建数据库和账号.md)中的创建数据库。

    ![create_database](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/8211549951/p93349.png)

2.  创建用户并授权读写权限。

    详情请参见[创建数据库和账号](/intl.zh-CN/RDS MySQL 数据库/快速入门/创建数据库和账号.md)中的创建普通账号。

    ![create_user](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/8211549951/p93347.png)

    请记录创建账号的用户名和密码，[创建集群](#section_13x_gol_ybr)会用到。

3.  获取数据库内网地址。

    1.  设置白名单，详情请参见[设置白名单](/intl.zh-CN/RDS MySQL 数据库/快速入门/设置白名单.md)。

    2.  在实例详细页面，单击左侧导航栏中的**数据库连接**。

    3.  在**数据库连接**页面，单击内网地址进行复制。

        ![net_inter](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/8211549951/p93358.png)

        请记录内网地址，[创建集群](#section_13x_gol_ybr)时会用到。


## 创建集群

在创建集群的**基础配置**页面，配置以下参数，其他参数的配置请参见[创建集群](/intl.zh-CN/集群管理/集群配置/创建集群.md)。

![create_cluster](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/8211549951/p93366.png)

|参数|描述|
|--|--|
|**集群名称**|集群的名字，长度限制为1~64个字符，仅可使用中文、字母、数字、中划线（-）和下划线（\_）。|
|**元数据选择**|选择**独立RDS MySQL**。|
|**数据库链接**|**数据库连接**填写格式为jdbc:mysql://rm-xxxxxx.mysql.rds.aliyuncs.com/<数据库名称\>?createDatabaseIfNotExist=true&characterEncoding=UTF-8。 -   rm-xxxxxx.mysql.rds.aliyuncs.com为[元数据库准备](#section_94v_jfe_16n)中获取的数据库内网地址。
-   <数据库名称\>为[元数据库准备](#section_94v_jfe_16n)中设置的数据库名称。 |
|**数据库用户名**|填写[元数据库准备](#section_94v_jfe_16n)中账号的用户名。|
|**数据库密码**|填写[元数据库准备](#section_94v_jfe_16n)中账号的密码。|

## Metastore初始化

1.  登录[阿里云E-MapReduce控制台](https://emr.console.aliyun.com/)。

2.  在顶部菜单栏处，根据实际情况选择地域（Region）和资源组。

3.  单击上方的**集群管理**页签。

4.  在**集群管理**页面，单击相应集群所在行的**详情**。

5.  在**集群基础信息**页面的**软件信息**区域，检查Hive的版本，并进行初始化。

    -   如果Hive是2.3.5版本，请执行以下命令进行初始化。

        使用SSH方式登录集群的Master节点，执行以下命令。

        1.  进入如下目录。

            ```
            cd /usr/lib/hive-current/scripts/metastore/upgrade/mysql/
            ```

        2.  登录MySQL数据库。

            ```
            mysql -h {RDS数据库内网或外网地址} -u{RDS用户名} -p{RDS密码}
            ```

        3.  在MySQL命令行中执行以下命令。

            ```
            use {RDS数据库名称};
            source /usr/lib/hive-current/scripts/metastore/upgrade/mysql/hive-schema-2.3.0.mysql.sql;
            ```

        上述命令中参数描述如下：

        -   `{RDS用户名}`为[元数据库准备](#section_94v_jfe_16n)中账号的用户名。
        -   `{RDS密码}`为[元数据库准备](#section_94v_jfe_16n)中账号的密码。
        -   `{RDS数据库名称}`为[元数据库准备](#section_94v_jfe_16n)中设置的数据库名称。
    -   如果Hive是其他版本时，推荐执行以下命令进行初始化。

        使用ssh方式登录集群的Master节点，执行以下命令。

        ```
        su hadoop
        schematool -initSchema -dbType mysql
        ```

    待初始化成功后，就可以使用自建的RDS作为Hive的元数据库。

    **说明：** 在初始化之前，Hive的Hive MetaStore、HiveServer2和Spark的ThriftServer可能会出现异常，待初始化之后会恢复正常。

    如果初始化时提示异常信息，请检查RDS MySQL安全组配置，确认RDS对EMR集群开通安全组白名单。

    ![initialize_fail](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/8211549951/p93353.png)

    如果Hive元数据信息中包含中文信息，例如列注释和分区名等，可以在对应RDS数据库中逐条依次执行如下命令修改相应字段为UTF-8格式。

    ```
    mysql> alter table <your_table> modify column <COMMENT> varchar(256) character set utf8;
    ```

    ```
    mysql> alter table TABLE_PARAMS modify column PARAM_VALUE varchar(4000) character set utf8;
    ```

    ```
    mysql> alter table PARTITION_PARAMS modify column PARAM_VALUE varchar(4000) character set utf8;
    ```

    ```
    mysql> alter table PARTITION_KEYS modify column PKEY_COMMENT varchar(4000) character set utf8;
    ```

    ```
    mysql> alter table INDEX_PARAMS modify column PARAM_VALUE varchar(4000) character set utf8;
    ```


