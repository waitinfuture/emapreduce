# 改写Jindo HDFS客户端路径

SmartData 3.1.0版本支持改写Jindo HDFS客户端级别的路径，以减少集群迁移时路径修改的工作量。例如，通过重写HDFS地址至OSS地址，方便您迁移HDFS中的数据至OSS后，无需改动业务逻辑中的数据地址，即可访问数据。

## 开启路径改写功能

1.  进入SmartData服务。

    1.  登录[阿里云E-MapReduce控制台](https://emr.console.aliyun.com/)。

    2.  在顶部菜单栏处，根据实际情况选择地域（Region）和资源组。

    3.  单击上方的**集群管理**页签。

    4.  在**集群管理**页面，单击相应集群所在行的**详情**。

    5.  在左侧导航栏单击**集群服务** \> **SmartData**。

2.  单击**配置**页签。

3.  在**服务配置**区域，单击**smartdata-site**页签。

4.  在**服务配置**区域，单击右侧的**自定义配置**。

5.  在**新增配置项**对话框中，添加**Key**为fs.hdfs.impl，**Value**为com.aliyun.emr.fs.hdfs.JindoHdfsShimsFileSystem的配置项。

    ![add-fs](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/8333816061/p184664.png)

6.  单击**确定**。

7.  保存配置。

    1.  单击右上角的**保存**。

    2.  在**确认修改**对话框中，输入执行原因，开启**自动更新配置**。

    3.  单击**确定**。


## 改写配置路径

1.  进入SmartData服务。

    1.  登录[阿里云E-MapReduce控制台](https://emr.console.aliyun.com/)。

    2.  在顶部菜单栏处，根据实际情况选择地域（Region）和资源组。

    3.  单击上方的**集群管理**页签。

    4.  在**集群管理**页面，单击相应集群所在行的**详情**。

    5.  在左侧导航栏单击**集群服务** \> **SmartData**。

2.  单击**配置**页签。

3.  在**服务配置**区域，单击**smartdata-site**页签。

4.  在**服务配置**区域，单击右侧的**自定义配置**。

5.  在**新增配置项**对话框中，添加如下两个配置项。

    |参数|描述|参数值|
    |--|--|---|
    |fs.jindo.shim.path-rewrite.<RULE-NAME\>.source|路径重写的挂载点。|    -   HA集群

hdfs://emr-cluster/<osspath\>

    -   非HA集群

hdfs://<your\_hostname\>:9000/<osspath\>

**说明：** <your\_hostname\>您可以通过SSH登录主节点，执行`hostname`命令获取，SSH登录主节点详情请参见[使用SSH连接主节点](/cn.zh-CN/集群管理/集群配置/连接集群/使用SSH连接主节点.md)。 |
    |fs.jindo.shim.path-rewrite.<RULE-NAME\>.target|您实际访问的路径。|oss://<your\_bucket\>/<testpath\>|

    RULE-NAME需要您自定义。

6.  单击**确定**。

7.  保存配置。

    1.  单击右上角的**保存**。

    2.  在**确认修改**对话框中，输入执行原因，开启**自动更新配置**。

    3.  单击**确定**。


## 示例

HA集群，在**smartdata-site**页签，添加如下参数后，您访问hdfs://emr-cluster/osspath时实际访问的是oss://jindo-bucket/<testpath\>的数据。

|参数|参数值|
|--|---|
|fs.jindo.shim.path-rewrite.testrule.source|hdfs://emr-cluster/osspath|
|fs.jindo.shim.path-rewrite.testrule.target|oss://jindo-bucket/<testpath\>|

您可以通过SSH登录集群的主节点，执行如下命令，查看改写情况。

```
hadoop fs -ls /
```

通过如下信息，看到osspath已经挂载在根目录下。

![demo](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/5275926061/p184665.png)

