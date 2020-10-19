# Jindo AuditLog使用说明

Jindo AuditLog提供块存储模式的审计功能，记录Namespace端的增加、删除和重命名操作信息。

-   已创建EMR-3.29.x版本的集群，详情请参见[创建集群](/cn.zh-CN/集群管理/集群配置/创建集群.md)。
-   已创建存储空间，详情请参见[创建存储空间](/cn.zh-CN/快速入门/创建存储空间.md)。

AuditLog可以分析Namespace端访问信息、发现异常请求和追踪错误等。JindoFS AuditLog存储日志文件至OSS，单个Log文件不超过5 GB。基于OSS的生命周期策略，您可以自定义日志文件的保留天数，清理策略等。因为JindoFS AuditLog提供分析功能，所以您可以通过Shell命令分析指定的日志文件。

## 审计信息

审计信息示例。

```
2020-07-09 18:29:24.689 allowed=true ugi=hadoop (auth:SIMPLE) ip=127.0.0.1 ns=test-block cmd=CreateFileletRequest src=jfs://test-block/test/test.snappy.parquet dst=null perm=::rwxrwxr-x
```

块存储模式记录的审计信息参数如下所示。

|参数|描述|
|--|--|
|时间|时间格式yyyy-MM-dd hh:mm:ss.SSS。|
|allowed|本次操作是否被允许：-   true
-   false |
|ugi|操作用户（包含认证方式信息）。|
|ip|Client IP。|
|ns|块存储模式Namespace的名称。|
|cmd|操作命令。|
|src|源路径。|
|dest|目标路径，可以为空。|
|perm|操作文件Permission信息。|

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

        ![namespace](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/0357459951/p161094.png)

3.  配置如下参数。

    1.  在**namespace**页签，单击右上角的**自定义配置**。

    2.  在**新增配置项**对话框中，新增如下参数。

        |参数|描述|是否必填|
        |--|--|----|
        |namespace.auditlog.enable|        -   true：打开AuditLog功能。
        -   false：关闭AuditLog功能。
|是|
        |namespace.auditlog.oss.uri|存储AuditLog的OSS Bucket。请参见oss://<yourbucket\>/auditLog格式配置。

<yourbucket\>请替换为待存储的Bucket的名称。

|是|
        |namespace.auditlog.oss.accessKey|存储OSS的AccessKey ID。|否|
        |namespace.auditlog.oss.accessSecret|存储OSS的AccessKey Secret。|否|
        |namespace.auditlog.oss.endpoint|存储OSS的Endpoint。|否|

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


## 使用Jindo auditLog分析功能

JindoFS为存储在OSS上的AuditLog文件提供Shell命令的分析功能，通过MR任务分析Log文件，提供Top-N活跃操作命令分析、Top-N活跃IP分析。您可以使用`jindo auditlog`命令，使用该功能。

Jindo Auditlog的参数说明如下表。

|参数|描述|是否必填|
|--|--|----|
|--src|存储AuditLog的OSS Bucket。默认为[步骤3](#step_pld_w9e_qxj)中namespace.auditlog.oss.uri的值，您也可以自定义该参数。|否|
|--ns|指定待分析的Namespace。默认为block模式下所有ns。|否|
|--type|指定分析：-   ip：IP地址活跃度。
-   cmd： 操作命令活跃度。

|是|
|--min|指定时间范围，分钟级别。|否**说明：** --min和--day，需要二选一。 |
|--day|指定时间范围，天级别。day 1，表示当天。 |

在E-MapReduce控制台，创建MR类型作业，作业内容示例如下。

```
jindo auditlog --src oss://<yourbucket>/auditlog/ --ns test --type ip --day 1 --top 2
```

返回信息如下。

```
16 openFileStatusRequest
6  deleteFileletRequest
```

