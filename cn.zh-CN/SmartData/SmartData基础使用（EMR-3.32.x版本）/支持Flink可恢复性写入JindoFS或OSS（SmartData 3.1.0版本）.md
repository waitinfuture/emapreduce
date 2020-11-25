# 支持Flink可恢复性写入JindoFS或OSS（SmartData 3.1.0版本）

SmartData 3.1.0版本支持Flink可恢复性写入JindoFS或OSS。通过Flink自有的检查点（Checkpoint）机制，当写入存储介质的作业发生局部失败时，作业可以迅速自动恢复，并继续写入。

可恢复性写入功能支持将数据以EXACTLY\_ONCE语义写入存储介质，在大数据场景下保证了数据的安全性和一致性。

## 在Flink作业中的用法

-   通用配置

    为了支持EXACTLY\_ONCE语义写入JindoFS或OSS，您需要执行如下配置：

    1.  打开Flink的检查点（Checkpoint）。

        示例如下。

        1.  通过如下方式建立的StreamExecutionEnvironment。

            ```
            StreamExecutionEnvironment env = StreamExecutionEnvironment.getExecutionEnvironment();
            ```

        2.  执行如下命令，启动Checkpoint。

            ```
            env.enableCheckpointing(<userDefinedCheckpointInterval>, CheckpointingMode.EXACTLY_ONCE);
            ```

    2.  使用可重发的数据源，例如Kafka。
-   便捷使用

    您无需额外引入依赖，只需使用带有jfs://或oss://前缀的路径，就可以使用该功能。JindoFS可以自动识别jfs://或oss://前缀，并启用该功能。

    例如，以DataStream<String\>的对象OutputStream为例。

    1.  添加Sink。
        -   将其写入JindoFS时，你可以执行如下命令添加Sink。

            ```
            String outputPath = "jfs://<user-defined-jfs-namespace>/<user-defined-jfs-dir>"
            StreamingFileSink<String> sink = StreamingFileSink.forRowFormat(
                    new Path(outputPath),
                    new SimpleStringEncoder<String>("UTF-8")
            ).build();
            outputStream.addSink(sink);
            ```

        -   将其写入OSS，你可以执行如下命令添加Sink。

            ```
            String outputPath = "oss://<user-defined-oss-bucket>/<user-defined-oss-dir>"
            StreamingFileSink<String> sink = StreamingFileSink.forRowFormat(
                    new Path(outputPath),
                    new SimpleStringEncoder<String>("UTF-8")
            ).build();
            outputStream.addSink(sink);
            ```

    2.  使用`env.execute()`执行Flink作业即可。

## 自定义配置

您在提交Flink作业时，可以自定义参数，以开启或控制特定功能。

例如，以yarn-cluster模式提交Flink作业时，通过`-yD`配置。示例如下。

```
<flink_home>/bin/flink run -m yarn-cluster -yD key1=value1 -yD key2=value2 ...
```

支持通过配置开启熵注入（Entropy Injection）功能或控制分片上传（Multipart Upload）的并行度。

-   熵注入

    该功能可以匹配写入路径的一段特定字符串，用一段随机的字符串进行替换，以削弱所谓片区效应，提高写入效率。

    -   如果是写入JindoFS（Block或Cache模式），则需要提供下列配置。

        ```
        jfs.entropy.key=<user-defined-key>
        jfs.entropy.length=<user-defined-length>
        ```

    -   如果是写入OSS，则需要提供下列配置。

        ```
        oss.entropy.key=<user-defined-key>
        oss.entropy.length=<user-defined-length>
        ```

-   分片上传

    当写入场景为OSS或JindoFS Cache模式时，可恢复性读写功能会自动调用高效的分片上传机制，将待上传的文件分为多个数据块分别上传，最后组合。目前支持配置参数oss.upload.max.concurrent.uploads，用来控制上传数据块的并行度，如果设置较高的数值则可能会提高写入效率（但也会占用更多资源）。默认情况下，该值为当前可用的处理器数量。


