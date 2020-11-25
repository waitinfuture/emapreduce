# 通过Hive作业处理TableStore数据

本文介绍如何在E-MapReduce中通过Hive作业来处理TableStore中的数据。

确保将实例部署在E-MapReduce集群相同的VPC环境下。

## 步骤一：创建Hadoop集群

1.  登录[阿里云E-MapReduce控制台](https://emr.console.aliyun.com)。

2.  创建Hadoop集群，详情请参见[创建集群](/intl.zh-CN/快速入门/创建集群.md)。

    ![创建集群](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/3042598951/p52748.png)


## 步骤二：获取JAR包并上传到Hadoop集群

1.  获取环境依赖的JAR包。

    |JAR包|获取方法|
    |----|----|
    |emr-tablestore-X.X.X.jar|Maven库中下载：[emr-tablestore](https://mvnrepository.com/artifact/com.aliyun.emr/emr-tablestore)。|
    |hadoop-lzo-X.X.X-SNAPSHOT.jar|登录Hadoop集群的emr-header-1主机，在/opt/apps/ecm/service/hadoop/x.x.x-x.x.x/package/hadoop-x.x.x-x.x.x/lib/下获取JAR包。|
    |hive-exec-X.X.X.jar|登录Hadoop集群的emr-header-1主机，在/opt/apps/ecm/service/hive/x.x.x-x.x.x/package/apache-hive-x.x.x-x.x.x-bin/lib/下获取JAR包。|
    |joda-time-X.X.X.jar|登录Hadoop集群的emr-header-1主机，在/opt/apps/ecm/service/hive/x.x.x-x.x.x/package/apache-hive-x.x.x-x.x.x-bin/lib/下获取JAR包。|
    |tablestore-X.X.X-jar-with-dependencies.jar|下载EMR SDK相关的依赖包：[tablestore](https://mvnrepository.com/artifact/com.aliyun.openservices/tablestore)。|

    **说明：**

    -   X.X.X表示JAR包的具体版本号；x.x.x-x.x.x表示文件目录，具体需要根据实际集群中的版本来修改这个JAR包。例如目录是3.1.1-1.1.6的， 那么就是JAR包是hive-exec-3.1.1.jar。

        ![jar](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/3042598951/p60923.png)

    -   登录Hadoop集群的emr-header-1主机的步骤，可参见[步骤二的子步骤2](#step_yxi_yke_s9h) ~ [步骤二的子步骤4](#step_7dt_7ua_jkn)。
2.  在**集群管理**页面，单击已创建的Hadoop集群的集群ID。

3.  在左侧导航树中选择**主机列表**，然后在右侧查看Hadoop集群中emr-header-1主机的IP信息。

4.  在SSH客户端中新建一个命令窗口，登录Hadoop集群的emr-header-1主机。

5.  上传所有JAR包到emr-header-1节点的某个目录下。

6.  拷贝所有JAR包到emr-header-1节点的/opt/apps/extra-jars/目录。

7.  通过scp命令将所有JAR包拷贝到worker节点（所有worker节点均需要）的/tmp目录。

    ```
    su hadoop
    scp <file> emr-worker-1:/tmp
    ```

8.  登录worker节点，拷贝所有JAR包到worker节点的/opt/apps/extra-jars/目录。

    **说明：** 所有worker节均需要进行此操作。


## 步骤三：重启Hive服务

1.  返回[阿里云E-MapReduce控制台](https://emr.console.aliyun.com)。

2.  在**集群管理**页面，单击已创建的Hadoop集群的集群ID。

3.  在**服务列表**中，单击Hive所在行的![more](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/3042598951/p60751.png)图标，并在弹出框中单击**重启所有组件**。

    ![restart](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/3042598951/p60752.png)


## 步骤四：配置Table存储

1.  在TableStore上创建表格，具体请参见[创建数据表](/intl.zh-CN/快速入门/创建数据表.md)。

    创建好后如下截图。

    ![table](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/3042598951/p60922.png)


## 步骤五：处理TableStore数据

1.  创建表格。

    ```
    CREATE EXTERNAL TABLE pet(name STRING, owner STRING, species STRING, sex STRING, birth STRING, death STRING)
    STORED BY 'com.aliyun.openservices.tablestore.hive.TableStoreStorageHandler'
    WITH SERDEPROPERTIES(
        "tablestore.columns.mapping"="name,owner,species,sex,birth,death")
    TBLPROPERTIES (
        "tablestore.endpoint"="https://XXX.cn-beijing.ots-internal.aliyuncs.com",
        "tablestore.access_key_id"="LTAIbUa7OYIO****",
        "tablestore.access_key_secret"="u2yOdykvSNXiPChyoqbCoawZnt****",
        "tablestore.table.name"="pet");
    ```

    -   当回显信息提示`OK`时，表示表格创建成功。

    -   当提示创建失败时， 检查配置信息是否正确：

-   是：[提交工单](https://workorder-intl.console.aliyun.com/#/ticket/createIndex)。
-   否：检查所有节点/opt/apps/extra-jars/目录下JAR包是否全部拷贝，如果已经全部拷贝请[提交工单](https://workorder-intl.console.aliyun.com/#/ticket/createIndex)处理；没有全部拷贝，请依据[步骤二的子步骤1](#step_8qa_b67_8ku)中JAR包进行拷贝。
2.  向表中插入数据。

    ```
    INSERT INTO pet VALUES("Fluffy", "Harold", "cat", "f", "1993-02-04", null);
    ```

    当回显信息提示`OK`时，表示数据插入成功。

3.  执行查询操作。

    ```
    hive> SELECT * FROM pet;
    ```

    ```
    OK
    Fluffy  Harold  cat  f  1993-02-04  NULL
    Time taken: 0.23 seconds, Fetched: 3 row(s)
    ```

    当回显信息包含`OK`时，表示查询成功。


