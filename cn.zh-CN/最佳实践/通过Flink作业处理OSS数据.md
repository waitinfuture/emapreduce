# 通过Flink作业处理OSS数据 {#task_1358170 .task}

本节介绍如何在E-MapReduce上创建Hadoop集群，然后在Hadoop集群中运行Flink作业来消费OSS数据。

-   已注册阿里云账号，详情请参见[注册云账号](http://help.aliyun.com/knowledge_detail/5974387.html)。
-   已开通E-MapReduce服务和OSS服务。
-   已完成云账号的授权，详情请参见[角色授权](../../../../cn.zh-CN/集群规划与配置/集群规划/角色授权.md#)。

在开发过程中，通常会遇到需要消费存储在阿里云OSS中的数据的场景。在阿里云E-MapReduce中，您可通过运行Flink作业来消费OSS存储空间中的数据。本节将在E-MapReduce上创建一个Flink作业，然后在Hadoop集群上运行这个Flink作业来读取并打印OSS中指定文件的内容。

## 步骤一 准备环境 {#section_sc4_0m8_rk0 .section}

在创建Flink作业前，您需要在本地安装Maven和Java环境，以及在E-MapReduce上创建Hadoop集群。如果Maven是3.0以上版本，则建议Java选择2.0及以下版本，否则会造成不兼容情况。

1.  在本地安装Maven和Java环境。
2.  登录[阿里云 E-MapReduce 控制台](https://emr.console.aliyun.com)。
3.  创建Hadoop集群（**可选服务**中必须选中Flink服务），详情请参见[创建集群](../../../../cn.zh-CN/快速入门/步骤三：创建集群.md#)。

## 步骤二 准备测试数据 {#section_54u_7zi_0rt .section}

在创建Flink作业前，您需要在OSS上传测试数据。本例以上传一个test.txt文件为例，文件内容为：Nothing is impossible for a willing heart. While there is a life, there is a hope~。

1.  登录[OSS 管理控制台](https://oss.console.aliyun.com/)。
2.  创建存储空间并上传测试数据文件，详情请参见[创建存储空间](../../../../cn.zh-CN/快速入门/创建存储空间.md#)和[上传文件](../../../../cn.zh-CN/快速入门/上传文件.md#)。 

    测试数据的上传路径在后续步骤的代码中会使用，本例的上传路径为oss://emr-logs2/hengwu/test.txt。

    **说明：** 上传文件后，请保留OSS的登录窗口，后续仍会使用。


## 步骤三 制作JAR包并上传到OSS或Hadoop集群 {#section_lkr_3dw_7yu .section}

本例JAR包来源：下载E-MapReduce示例代码[aliyun-emapreduce-demo](https://github.com/aliyun/aliyun-emapreduce-demo)，编译生成JAR包。JAR包可上传到Hadoop集群的header主机中，也可上传到OSS中，本例以上传到OSS为例。

1.  下载E-MapReduce示例代码[aliyun-emapreduce-demo](https://github.com/aliyun/aliyun-emapreduce-demo)到本地。
2.  运行mvn clean package -DskipTests命令打包代码。 

    打包好的JAR包为存储在../target/目录下，例如，target/examples-1.2.0.jar。

3.  返回到[OSS 管理控制台](https://oss.console.aliyun.com/)。
4.  上传JAR包到OSS任一路径下。 

    JAR包的上传路径在后续步骤的代码中会使用，本例的上传路径为oss://emr-logs2/hengwu/examples-1.2.0.jar。


## 步骤四 创建并运行Flink作业 {#section_qzw_s78_er5 .section}

1.  返回到[阿里云 E-MapReduce 控制台](https://emr.console.aliyun.com)。
2.  在**数据开发**页面创建项目，详情请参见[项目管理](../../../../cn.zh-CN/数据开发/项目管理.md#)。
3.  进入新建的项目，在作业编辑页面新建**Flink**类型的作业。
4.  新建Flink作业后，配置其**作业内容**。 

    ![Flink作业内容](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/1082642/156695803153103_zh-CN.png)

    **作业内容**是一段代码，本节使用的示例代码如下：

    ``` {#codeblock_y24_sjw_1gy}
    run -m yarn-cluster  -yjm 1024 -ytm 1024 -yn 4 -ys 4 -ynm flink-oss-sample -c com.aliyun.emr.example.flink.FlinkOSSSample  ossref://emr-logs2/hengwu/examples-1.2.0.jar --input oss://emr-logs2/hengwu/test.txt
    ```

    示例代码中的关键参数说明如下：

    -   ossref://emr-logs2/hengwu/examples-1.2.0.jar：上传至OSS的JAR包。
    -   oss://emr-logs2/hengwu/test.txt：上传到OSS的测试数据。
    **说明：** 实际操作时，您需要根据[步骤一 准备环境](#section_sc4_0m8_rk0)和[步骤三 制作JAR包并上传到OSS或Hadoop集群](#section_lkr_3dw_7yu)中的配置来替换这两个参数。

5.  作业配置完成后，单击右上方的**运行**，在弹出的对话框中选择**执行集群**为新建的Hadoop集群。
6.  单击**确定**，运行Flink作业。 

    作业开始运行时，会自动弹出日志。作业成功运行后，会从OSS读取指定文件内容并打印在日志中。至此，我们成功实现了在E-MapReduce集群上运行Flink作业消费 OSS 数据。

    ![Flink作业结果](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/1082642/156695803153150_zh-CN.png)


## 步骤五 查看作业提交日志和作业信息（可选） {#section_xcd_kdu_25t .section}

如果需要定位作业失败的原因或了解作业的详细信息，则您可查看作业的日志和作业信息。

1.  查看作业提交日志。 当前提交日志支持在E-MapReduce控制台查看，也支持在SSH客户端查看。
    -   登录[阿里云 E-MapReduce 控制台](https://emr.console.aliyun.com)查看提交日志。

        在控制台提交作业后，可通过运行记录列表进入某次作业运行的详情页面，在详情页面可查看作业的日志。

        ![Flink作业执行记录](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/1082642/156695803153277_zh-CN.png)

        ![Flink作业日志](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/1082642/156695803153291_zh-CN.png)

    -   通过SSH客户端登录到Hadoop集群的header主机查看提交日志。

        默认情况下，根据 Flink的**log4j**配置（详情请参见/etc/ecm/flink-conf/log4j-yarn-session.properties），提交日志会保存在/mnt/disk1/log/flink/flink-\{user\}-client-\{hostname\}.log。

        其中，user为提交Flink作业的用户，hostname为提交作业所在的节点。以root用户在 **emr-header-1**节点提交Flink作业为例，日志的路径为/mnt/disk1/log/flink/flink-flink-historyserver-0-emr-header-1.cluster-126601.log。

2.  查看作业信息。 

    通过**Yarn UI**可查看Flink作业的信息。访问**Yarn UI**有SSH隧道和Knox两种方式，SSH隧道方式请参见[SSH 登录集群](../../../../cn.zh-CN/集群规划与配置/集群配置/SSH 登录集群.md#)，Knox方式请参见[Knox 使用说明](../../../../cn.zh-CN/开源组件介绍/Knox 使用说明.md#)和[访问链接与端口](../../../../cn.zh-CN/集群规划与配置/集群配置/访问链接与端口.md#)。下面以Knox方式为例进行介绍。

    1.  在Hadoop集群的**访问链接与端口**页面中，单击**Yarn UI**后的链接，进入Hadoop控制台。 

        ![YARN UI链接](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/1082642/156695803153382_zh-CN.png)

    2.  在Hadoop控制台，单击作业的**ID**，查看作业运行详情。 

        ![Hadoop控制台>Flink作业列表](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/1082642/156695803153394_zh-CN.png)

        ![Hadoop控制台>Flink作业详情](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/1082642/156695803153402_zh-CN.png)

    3.  如果需要查看运行中的Flink作业，则可在作业详情页面单击**Tracking URL**后面的链接，进入Flink Dashboard查看。 

        在作业运行结束后，通过访问http://emr-header-1:8082，可以查看所有已经完成的作业列表。


