# E-MapReduce SDK 发布说明 {#concept_dx5_gmt_hfb .concept}

E-MapReduce 各版本SDK 发布说明。

**说明：** 

-   emr-core：支持Hadoop/Spark与OSS数据源的交互，默认已经存在集群的运行环境中，作业打包时 不需要 将emr-core打进去。
-   emr-tablestore: 支持Hadoop/Hive/Spark与OTS数据源的交互，使用时需要打进作业Jar包。
-   emr-mns\_2.10／emr-mns\_2.11：支持Spark读MNS数据源，使用时需要打进作业Jar包。
-   emr-ons\_2.10／emr-ons\_2.11：支持Spark读ONS数据源，使用时需要打进作业Jar包。
-   emr-logservice\_2.10／emr-logservice\_2.11：支持Spark读LogService数据源，使用时需要打进作业Jar包。
-   emr-maxcompute\_2.10／emr-maxcompute\_2.11: 支持Spark读写MaxCompute数据源，使用时需要打进作业Jar包。

```
<!--支持OSS数据源 -->
    <dependency>
        <groupId>com.aliyun.emr</groupId>
        <artifactId>emr-core</artifactId>
        <version>1.4.1</version>
    </dependency>
    <!--支持OTS数据源-->
    <dependency>
        <groupId>com.aliyun.emr</groupId>
        <artifactId>emr-tablestore</artifactId>
        <version>1.4.1</version>
    </dependency>
    <!-- 支持 MNS、ONS、LogService、MaxCompute数据源 (Spark 1.x环境)-->
    <dependency>
        <groupId>com.aliyun.emr</groupId>
        <artifactId>emr-mns_2.10</artifactId>
        <version>1.4.1</version>
    </dependency>
    <dependency>
        <groupId>com.aliyun.emr</groupId>
        <artifactId>emr-logservice_2.10</artifactId>
        <version>1.4.1</version>
    </dependency>
    <dependency>
        <groupId>com.aliyun.emr</groupId>
        <artifactId>emr-maxcompute_2.10</artifactId>
        <version>1.4.1</version>
    </dependency>
    <dependency>
        <groupId>com.aliyun.emr</groupId>
        <artifactId>emr-ons_2.10</artifactId>
        <version>1.4.1</version>
    </dependency>
    <!-- 支持 MNS、ONS、LogService、MaxCompute数据源 (Spark 2.x环境)-->
    <dependency>
        <groupId>com.aliyun.emr</groupId>
        <artifactId>emr-mns_2.11</artifactId>
        <version>1.4.1</version>
    </dependency>
    <dependency>
        <groupId>com.aliyun.emr</groupId>
        <artifactId>emr-logservice_2.11</artifactId>
        <version>1.4.1</version>
    </dependency>
    <dependency>
        <groupId>com.aliyun.emr</groupId>
        <artifactId>emr-maxcompute_2.11</artifactId>
        <version>1.4.1</version>
    </dependency>
    <dependency>
        <groupId>com.aliyun.emr</groupId>
        <artifactId>emr-ons_2.11</artifactId>
        <version>1.4.1</version>
    </dependency>
```

-   v1.4.1
    -   MaxCompute: 修复datetime类型时间截断的问题
    -   MaxCompute: 修复SimpleDateFormat的线程安全问题
-   v1.4.0
    -   MaxCompute：新增datasource的实现方式 \(只支持Spark2.x以上版本\)。
    -   LogService：新增Direct API的实现方式 \(只支持Spark2.x以上版本\)。
    -   OTS：一些读写的优化。
    -   修复读取LogService数据源时，用户AK被错误替换成集群应用角色AK的BUG。
-   v1.3.2
    -   修复OTS的一些BUG。
-   v1.3.1
    -   修复Spark+LogService部分场景下抛空指针问题。
    -   从这个版本开始，SDK支持Spark2.x环境。
-   v1.3.0
    -   HadoopMR, Spark, SparkSQL, Hive读取OTS数据。
    -   MNS和LogService支持E-MapReduce的MetaServie功能，支持在E-MapReduce环境下免AK访问MNS和LogService数据。
    -   升级部分依赖包版本。
-   v1.1.3.1

    SDK：

    -   解决MNS与Spark/Hadoop包的依赖冲突问题。
    -   解决Spark Streaming + MNS某些场景下抛空指针问题。
    -   解决python sdk的部分BUG。
    -   Spark Streaming + Loghub支持自定义时间位置的功能。
-   Core
    -   解决Hadoop无法支持原生Snappy文件问题。目前E-MapReduce支持处理LogService以Snappy格式归档到OSS的文件。
    -   解决Spark无法支持Snappy压缩文件的问题。
    -   解决OSS不支持Hadoop2.7.2 OutputCommitter两种算法的问题。
    -   改善Hadoop/Spark读写OSS的性能。
    -   解决Spark作业打印的Log4j异常输出的问题。
-   v1.1.2
    -   解决作业慢读写OSS出现的ConnectionClosedException问题。
    -   解决OSS数据源时部分hadoop命令不可用问题。
    -   解决java.text.ParseException: Unparseable date”问题。
    -   优化emr-core支持本地调试运行。
    -   兼容老版本的产生的“\_$folder$”文件，解释成目录，不再当作普通文件处理。
    -   Hadoop/Spark读写OSS增加失败重试机制。
-   v1.1.1
    -   解决本地写OSS临时文件时导致多磁盘使用不均衡的问题。
    -   去除作业执行过程中创建OSS目录时同时创建的$\_folder$标记文件。
-   v1.1.0
    -   升级LogHub SDK到0.6.2，废弃Client DB模式，使用Server DB模式。
    -   升级OSS SDK到2.2.0，修复OSS SDK BUG导致的运行异常。
    -   新增对MNS的支持。
    -   兼容性
        -   对于1.0.x系列SDK
            -   接口：
                -   兼容
            -   命名空间：
                -   不兼容：调整包结构，将包名称com.aliyun更换为com.aliyun.emr
    -   修改项目的groupId，从com.aliyun改为com.aliyun.emr。修改后的POM依赖为

        ```
        <dependency>
              <groupId>com.aliyun.emr</groupId>
              <artifactId>emr-sdk_2.10</artifactId>
              <version>1.1.3.1</version>
          </dependency>
        ```

-   v1.0.5
    -   优化LoghubUtils接口，优化参数输入。
    -   优化LogStore数据的输出格式，增加topic和source两个字段。
    -   增加LogStore数据拉取的时间间隔参数配置。参数spark.logservice.fetch.interval.millis，默认值200毫秒。
    -   更新依赖ODPS SDK版本到0.20.7-public。
-   v1.0.4
    -   将guava的依赖版本降为11.0.2，避免和Hadoop中的guava版本冲突。
    -   计算任务支持数据超过5GB的文件大小。
-   v1.0.3
    -   增加OSS Client相关的配置参数。
-   v1.0.2
    -   修复OSS URI解析出错的BUG。
-   v1.0.1
    -   优化OSS URI设置。
    -   增加对ONS的支持。
    -   增加LogService的支持。
    -   支持OSS的追加写特性。
    -   支持以multi part方式上传OSS数据。
    -   支持以upload part copy方式拷贝OSS数据。

