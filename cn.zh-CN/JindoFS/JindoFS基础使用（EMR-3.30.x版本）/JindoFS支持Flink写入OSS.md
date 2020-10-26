# JindoFS支持Flink写入OSS

EMR-3.30.0版本及后续版本，JindoFS支持Flink写入OSS。当写入OSS的作业发生局部失败时，您可以通过Flink自有的检查点（checkpoint）机制，能够迅速恢复作业，并继续写入。

开源Flink版本对写入OSS的支持尚不完整。在流式数据处理场景，如果数据是直接写入OSS的，则不能支持作业的可恢复性写入，即对于一个大规模分布式流处理系统，一旦发生局部失败（通常认为是很难避免的），就会丢失数据或出现重复数据（即不支持EXACTLY\_ONCE语义）。

## 在Flink作业中的用法

-   通用配置

    为了支持EXACTLY\_ONCE语义写入OSS，您需要执行如下配置：

    1.  打开Flink的检查点（checkpoint）。

        示例如下。

        1.  通过如下方式建立的 StreamExecutionEnvironment。

            ```
            StreamExecutionEnvironment env = StreamExecutionEnvironment.getExecutionEnvironment();
            ```

        2.  执行如下命令，启动checkpoint。

            ```
            env.enableCheckpointing(<userDefinedCheckpointInterval>, CheckpointingMode.EXACTLY_ONCE);
            ```

    2.  使用可重发的数据源，例如Kafka。
-   便捷使用

    您无需额外引入依赖，只需要选择合适的EMR版本，并使用带oss://前缀的路径，就可以使用该功能。EMR版本详情请参见[版本概述](/cn.zh-CN/发行版本/版本概述.md)。

    例如，您通过计算和转换，最终形成一个DataStream<String\>的对象OutputStream，并期望将其写入OSS，你可以执行如下命令添加sink。

    ```
    String outputPath = "oss://<user-defined-oss-bucket>/<user-defined-oss-dir>"
    StreamingFileSink<String> sink = StreamingFileSink.forRowFormat(
            new Path(outputPath),
            new SimpleStringEncoder<String>("UTF-8")
    ).build();
    outputStream.addSink(sink);
    ```


## 自定义配置

您在提交Flink作业时，可以自定义参数。

例如，以yarn-cluster模式提交Flink作业时，通过`-yD`配置的`oss.upload.max.concurrent.uploads`参数，以控制允许同时上传数据块（part）数量的最大值。示例如下。

```
<flink_home>/bin/flink run -m yarn-cluster -yD oss.upload.max.concurrent.uploads=2 ...
```

本文介绍的功能会自动调用OSS提供的高效的分片上传（Multipart Upload）机制，将待上传的文件分为多个part分别上传，最后组合。默认情况下，该值为当前可用的处理器数量。

