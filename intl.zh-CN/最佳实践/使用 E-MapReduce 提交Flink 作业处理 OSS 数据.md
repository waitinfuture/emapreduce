# 使用 E-MapReduce 提交Flink 作业处理 OSS 数据 {#task_e4x_1cw_dgb .task}

本文介绍如何在 E-MapReduce 上创建 Hadoop 集群，运行 Flink 作业来消费 OSS 数据。

在开发过程中我们通常会碰到需要需要消费阿里云 OSS数据的场景，本文通过使用阿里云 E-MapReduc e创建一个 Hadoop 集群并运行 Flink 作业来消费OSS上的数据。

1.  环境准备 

    创建 Hadoop 集群时，在可选服务中选择 Flink。

2.  开发 Flink 作业 
    1.  打包工程，执行`mvn clean package -DskipTests`命令 

        E-MapReduce 已经提供了现成的示例代码，直接使用即可，地址为[e-mapreduce-demo](https://github.com/aliyun/aliyun-emapreduce-demo)​ ，打包好的 jar 包路径为target/examples-1.1.jar

    2.  OSS 准备测试数据 

        创建 OSS 路径，准备测试数据

    3.  运行 Flink 作业 

        ​ 将打包好的 `examples-1.1.jar` 上传至 Hadoop 集群的emr-header-1 节点上，登录到 Hadoop 集群提交作业，命令如下：

        ```
        flink run -m yarn-cluster  -yjm 1024 -ytm 1024 -yn 4 -ys 4 -ynm flink-oss-sample -c com.aliyun.emr.example.flink.FlinkOSSSample examples-1.1.jar --input oss://bucket/path
        ```

        其中，oss://bucket/path为上一步准备 OSS 数据的路径。

    4.  查看作业信息 

        通过 Yarn 的 Web UI 可以查看 Flink 作业的信息。访问 Yarn Web UI 有2 种方式: 使用 SSH 隧道， 参考文档 [SSH 登录集群](../../../../../intl.zh-CN/用户指南/SSH 登录集群.md#)；或者通过Knox方式，参考文档[Knox 使用说明](../../../../../intl.zh-CN/用户指南/开源组件介绍/Knox 使用说明.md#)。

        -   查看运行中作业信息

            本文选择使用SSH隧道方式，访问地址： http://emr-header-1:8088/index.html 。在 Yarn 的 Web U I中，可以看到我们刚刚提交的作业如下图所示：

            ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/80562/154874757334444_zh-CN.png)

            进入作业后，通过作业的 Tracking URL 链接可以跳转至 Flink Dashboard，查看运行中的作业。

            ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/80562/154874757334445_zh-CN.png)

            ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/80562/154874757334446_zh-CN.png)

        -   查看历史作业信息

            在作业运行结束后，通过访问http://emr-header-1:8082，可以查看所有已经完成作业的列表。

            ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/80562/154874757334447_zh-CN.png)


至此，我们成功实现了在 E-MapReduce 集群上运行Flink 作业消费 OSS 数据。

