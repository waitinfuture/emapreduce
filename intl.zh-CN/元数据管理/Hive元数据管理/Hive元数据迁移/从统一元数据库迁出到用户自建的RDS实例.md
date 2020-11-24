# 从统一元数据库迁出到用户自建的RDS实例

为了保证更稳定的大规模Hive元数据服务，您可以从原有的统一元数据库迁出到您自建的RDS实例。

已购买RDS，详情请参见[创建RDS MySQL实例](/intl.zh-CN/RDS MySQL 数据库/快速入门/创建RDS MySQL实例.md)。

**说明：**

-   建议**类型**选择**MySQL**的5.7；**系列**选择**高可用版**。
-   RDS MySQL实例须与E-MapReduce的实例处于同一个安全组，以便RDS与E-MapReduce可以通过内网地址互通。

## 操作步骤

1.  在RDS中创建一个**database**，名称为**hivemeta**，同时创建一个用户，把**hivemeta**的读写权限赋给这个用户，详情请参见[配置独立RDS](/intl.zh-CN/元数据管理/Hive元数据管理/配置独立RDS.md)。

2.  导出统一元数据库的内容（只导出数据，不用导表结构）。

    1.  为保证数据的一致性， 在Hive服务页面停止Hive的MetaStore服务，保证导出期间不会有新的元数据变化，详情请参见[停止Hive的MetaStore服务](#section_7ea_anx_wx9)。

    2.  在Hive服务页面，单击**配置**页签。

    3.  在配置页面，查找**javax.jdo.option.ConnectionUserName**、**javax.jdo.option.ConnectionPassword**和**javax.jdo.option.ConnectionURL**的值。

        **说明：**

        -   如果是老版本集群，请在$HIVE\_CONF\_DIR/hive-site.xml中查找。
        -   **javax.jdo.option.ConnectionUserName**表示对应数据库用户名；**javax.jdo.option.ConnectionPassword**表示对应数据库访问密码；**javax.jdo.option.ConnectionURL**对应数据库访问地址和库名。
    4.  进入集群Master节点，执行以下命令。

        ```
        mysqldump -t DATABASENAME -h HOST -P PORT -u USERNAME -p PASSWORD > /tmp/metastore.sql
        ```

        **说明：** 密码为上一步骤在配置页面获取的密码。

3.  修改集群Master节点上的/usr/local/emr/emr-agent/run/meta\_db\_info.json，把里面的**use\_local\_meta\_db**设置为**false**，meta数据库的链接地址、用户名和密码换成RDS的信息。

    **说明：**

    -   对于无此文件的集群，直接忽略此步骤。
    -   如果是HA集群，两个Master节点都需要进行操作。
4.  在Hive配置页面，把元数据库的链接地址、用户名和密码换成新RDS的信息。

    如果是老版本集群，修改$HIVE\_CONF\_DIR/hive-site.xml中对应的配置为需要连接的数据库。

5.  在一台Master节点上，把hive-site.xml里面的元数据库链接地址、用户名和密码换成RDS的信息，然后执行初始化schema。

    -   如果Hive是2.3.5版本时，请执行以下命令进行初始化。

        使用ssh方式登录集群Master节点，执行以下命令。

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

        **说明：** `{RDS数据库名称}`为[步骤1](#step_tsu_445_5fr)中创建的数据库，例如hivemeta。

    -   其他版本时，执行以下命令进行初始化。

        使用ssh方式登录集群Master节点，执行以下命令。

        ```
        cd /usr/lib/hive-current/bin
        ./schematool -initSchema -dbType mysql
        ```

6.  把之前导出来的meta数据导入RDS，您可以通过以下命令行登录MySQL。

    ```
    mysql -h {rds的url} -u {rds的用户名} -p
    ```

    进入MySQL的命令行之后，执行`source /tmp/metastore.sql`正常情况可以完全导入，不会报错。

7.  [重启Hive所有组件](#section_ncd_1g2_jsm)。

    验证Hive功能是否正常，可以在Master节点上，执行`hive cli`，确认数据是否和先前一样。


## 进入Hive服务页面

1.  登录[阿里云E-MapReduce控制台](https://emr.console.aliyun.com/)。

2.  单击上方的**集群管理**页签。

3.  在**集群管理**页面，单击相应集群所在行的**详情**。

4.  在左侧导航栏，单击**集群服务** \> **Hive**。


## 停止Hive的MetaStore服务

1.  [进入Hive服务页面](#section_vo6_t1q_3st)。

2.  在Hive服务页面的**组件列表**区域，单击**Hive MetaStore**所在行的**停止**。

    1.  在**执行集群操作**对话框中，选择**执行范围**、**是否滚动执行**和**失败处理策略**，输入**执行原因**。

    2.  单击**确定**。


## 重启Hive所有组件

1.  [进入Hive服务页面](#section_vo6_t1q_3st)。

2.  单击**配置**页签。

3.  单击右上角的**操作** \> **重启 All Components**。

    1.  在**执行集群操作**对话框中，选择**执行范围**、**是否滚动执行**和**失败处理策略**，输入**执行原因**。

    2.  单击**确定**。


