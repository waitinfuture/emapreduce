# JindoFS AuditLog使用说明

Jindo AuditLog提供缓存和Block模式的审计功能，记录Namespace端的增加、删除和重命名操作信息。

-   已创建EMR-3.30.0版本的集群，详情请参见[创建集群](/cn.zh-CN/集群管理/集群配置/创建集群.md)。
-   已创建存储空间，详情请参见[创建存储空间](/cn.zh-CN/快速入门/创建存储空间.md)。

AuditLog可以分析Namespace端访问信息、发现异常请求和追踪错误等。JindoFS AuditLog存储日志文件至OSS，单个Log文件不超过5 GB。基于OSS的生命周期策略，您可以自定义日志文件的保留天数和清理策略等。因为JindoFS AuditLog提供分析功能，所以您可以通过Shell命令分析指定的日志文件。

## 审计信息

Block模式记录的审计信息参数如下所示。

|参数|描述|
|--|--|
|时间|时间格式yyyy-MM-dd hh:mm:ss.SSS。|
|allowed|本次操作是否被允许，取值如下：-   true
-   false |
|ugi|操作用户（包含认证方式信息）。|
|ip|Client IP。|
|ns|Block模式namespace的名称。|
|cmd|操作命令。|
|src|源路径。|
|dest|目标路径，可以为空。|
|perm|操作文件Permission信息。|

审计信息示例。

```
2020-07-09 18:29:24.689 allowed=true ugi=hadoop (auth:SIMPLE) ip=127.0.0.1 ns=test-block cmd=CreateFileletRequest src=jfs://test-block/test/test.snappy.parquet dst=null perm=::rwxrwxr-x
```

## 使用AuditLog

1.  进入SmartData服务。

    1.  登录[阿里云E-MapReduce控制台](https://emr.console.aliyun.com/)。

    2.  在顶部菜单栏处，根据实际情况选择地域（Region）和资源组。

    3.  单击上方的**集群管理**页签。

    4.  在**集群管理**页面，单击相应集群所在行的**详情**。

    5.  在左侧导航栏单击**集群服务** \> **SmartData**。

2.  进入**namespace**服务配置。

    1.  单击**配置**页签。

    2.  单击**namespace**。

        ![namespace](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/0357459951/p161094.png)

3.  配置如下参数。

    1.  在**namespace**页签，单击右上角的**自定义配置**。

    2.  在**新增配置项**对话框中，新增如下参数。

        |参数|描述|是否必填|
        |--|--|----|
        |jfs.namespaces.\{ns\}.auditlog.enable|打开指定namespaces的AuditLog开关，取值如下：        -   true：打开AuditLog功能。
        -   false：关闭AuditLog功能。
|是|
        |namespace.sysinfo.oss.uri|存储AuditLog的OSS Bucket。请参见oss://<yourbucket\>/auditLog格式配置。

<yourbucket\>请替换为待存储的Bucket的名称。

|是|
        |namespace.sysinfo.oss.access.key|存储OSS的AccessKey ID。|否|
        |namespace.sysinfo.oss.access.secret|存储OSS的AccessKey Secret。|否|
        |namespace.sysinfo.oss.endpoint|存储OSS的Endpoint。|否|

    3.  单击**部署客户端配置**。

    4.  在**执行集群操作**对话框中，输入**执行原因**，单击**确定**。

    5.  在**确认**对话框中，单击**确定**。

4.  重启服务。

    1.  单击右上角的**操作** \> **重启Jindo Namespace Service**。

    2.  在**执行集群操作**对话框中，输入**执行原因**，单击**确定**。

    3.  在**确认**对话框中，单击**确定**。

5.  配置清理策略。

    OSS提供了lifeCycle功能来管理OSS上文件的生命周期，您可以利用该功能来自定义Log文件的清理或者保存时间。

    1.  登录[OSS管理控制台](https://oss.console.aliyun.com/)。

    2.  单击创建的存储空间。

    3.  在左侧导航栏，单击**基础设置** \> **生命周期**，在**生命周期**单击**设置**。

    4.  单击**创建规则**，在**创建生命周期规则**配置各项参数。

        详情请参见[设置生命周期规则](/cn.zh-CN/控制台用户指南/存储空间管理/基础设置/设置生命周期规则.md)。

    5.  单击**确定**。


## 使用Jindo AuditLog分析功能

JindoFS为存储在OSS上的AuditLog文件提供SQL的分析功能，通过SQL分析相关表，提供Top-N活跃操作命令分析和Top-N活跃IP分析。您可以使用`jindo sql`命令，使用该功能。

`jindo sql`使用Spark-SQL语法，内部嵌入了audit\_log\_source（auditlog原始数据）、audit\_log（auditlog清洗后数据）和fs\_image（fsimage日志数据）三个表，audit\_log\_source和fs\_image均为分区表。使用方法如下：

-   `jindo sql --help`查看支持参数的详细信息。常用参数如下。

    |参数|描述|
    |--|--|
    |-f|指定运行的SQL文件。|
    |-i|启动jindo sql后自动运行初始化SQL脚本。|

-   `show partitions table_name`获取所有分区。
-   `desc formatted table_name`查看表结构。

因为Jindo sql基于Spark的程序，所以初始资源可能较小，您可以通过环境变量JINDO\_SPARK\_OPTS来修改初始资源Jindo sql的启动参数，修改示例如下。

```
 export JINDO_SPARK_OPTS="--conf spark.driver.memory=4G --conf spark.executor.instances=20 --conf spark.executor.cores=5 --conf spark.executor.memory=20G"
```

示例如下：

-   执行如下命令显示表。

    ```
    show tables;
    ```

    ![show_table](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/9992073061/p175763.png)

-   执行如下命令显示分区。

    ```
    show partitions audit_log_source;
    ```

    返回信息类似如下。

    ![show_audit_log_source](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/9992073061/p175770.png)

-   执行如下查询数据。

    ```
    select * from audit_log_source limit 10;
    ```

    返回信息类似如下。

    ![audit_log_source](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/9992073061/p175767.png)

    ```
    select * from audit_log limit 10;
    ```

    返回信息类似如下。

    ![audit_log](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/9992073061/p175788.png)

-   执行如下命令统计2020-10-20日不同命令的使用频次。

    ![rate](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/9992073061/p175791.png)


