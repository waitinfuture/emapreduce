# Spark-Submit参数设置说明

本文介绍如何在E-MapReduce集群中设置Spark-Submit的参数。

## 集群配置

-   软件配置

    E-MapReduce产品版本1.1.0

    -   Hadoop 2.6.0
    -   Spark 1.6.0
-   硬件配置
    -   Master节点
        -   8 核 16 GiB 500 GB高效云盘
        -   1 台
    -   Worker节点 x 10 台
        -   8 核 16 GiB 500 GB高效云盘
        -   10 台
    -   总资源：8 核 16GiB（Worker）x 10 + 8 核 16GiB（Master）

        **说明：** 由于作业提交的时候资源只计算CPU和内存，所以这里磁盘的大小并未计算到总资源中。

    -   Yarn可分配总资源：12 核 12.8 GiB（Worker）x 10

        **说明：** 默认情况下，yarn可分配核 = 机器核 x 1.5，yarn可分配内存 = 机器内存 x 0.8。


## 提交作业

创建集群后，您可以提交作业。示例如下。

![job_set](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/3591621061/p13107.png)

上图所示的作业，直接使用了Spark官方的example包，所以不需要自己上传JAR包。

参数列表如下所示。

```
--class org.apache.spark.examples.SparkPi --master yarn --deploy-mode client --driver-memory 4g --num-executors 2 --executor-memory 2g --executor-cores 2 /opt/apps/spark-1.6.0-bin-hadoop2.6/lib/spark-examples*.jar 10
```

|参数|参考值|说明|
|--|---|--|
|class|org.apache.spark.examples.SparkPi|作业的主类。|
|master|yarn|E-MapReduce使用Yarn模式。|
|yarn-client|等同于–-master yarn —deploy-mode client， 此时不需要指定deploy-mode。|
|yarn-cluster|等同于–-master yarn —deploy-mode cluster， 此时不需要指定deploy-mode。|
|deploy-mode|client|client模式表示作业的AM会放在Master节点上运行。如果设置此参数，需要指定Master为yarn。|
|cluster|cluster模式表示AM会随机的在Worker节点中的任意一台上启动运行。如果设置此参数，需要指定Master为yarn。|
|driver-memory|4g|driver使用的内存，不可超过单机的总内存。|
|num-executors|2|创建executor的个数。|
|executor-memory|2g|各个executor使用的最大内存，不可以超过单机的最大可使用内存。|
|executor-cores|2|各个executor使用的并发线程数目，即每个executor最大可并发执行的Task数目。|

## 资源计算

在不同模式、不同的设置下运行时，作业使用的资源情况如下表所示。

-   yarn-client模式的资源计算

    |节点|资源类型|资源量（结果使用上面的例子计算得到）|
    |--|----|------------------|
    |master|core|1|
    |mem|driver-memroy = 4G|
    |worker|core|num-executors \* executor-cores = 4|
    |mem|num-executors \* executor-memory = 4G|

    -   作业主程序（Driver 程序）会在Master节点上执行。按照作业配置将分配4G（由 —driver-memroy 指定）的内存给它（当然实际上可能没有用到）。
    -   会在Worker节点上起2个（由 —num-executors 指定）executor，每一个executor最大能分配2G（由 —executor-memory 指定）的内存，并最大支持 2 个（由—executor-cores 指定）task的并发执行。
-   yarn-cluster 模式的资源计算

    |节点|资源类型|资源量（结果使用上面的例子计算得到）|
    |--|----|------------------|
    |master|-|一个很小的 client 程序，负责同步 job 信息，占用很小。|
    |worker|core|num-executors \* executor-cores+spark.driver.cores = 5|
    |mem|num-executors \* executor-memory + driver-memroy = 8g|

    **说明：** spark.driver.cores默认是1。


## 资源使用的优化

-   yarn-client模式

    若您有了一个大作业，使用yarn-client模式，想要多用一些这个集群的资源，请参见如下配置。

    ```
    --master yarn-client --driver-memory 5g –-num-executors 20 --executor-memory 4g --executor-cores 4
    ```

    **说明：**

    -   Spark在分配内存时，会在用户设定的内存值上溢出375M或7%（取大值）。
    -   Yarn分配Container内存时，遵循向上取整的原则，这里也就是需要满足1G的整数倍。
    按照上述的资源计算公式

    -   Master的资源量为：

-   core：1
-   mem：6G （5 G + 375 M向上取整为6 G）
    -   workers的资源量为：

-   core: 20\*4 = 80
-   mem：20\*5G （4G + 375M 向上取整为 5G）= 100G
    可以看到总的资源没有超过集群的总资源，那么遵循这个原则，您还可以有很多种配置，例如：

    -   ```
--master yarn-client --driver-memory 5g --num-executors 40 --executor-memory 1g --executor-cores 2
```

    -   ```
--master yarn-client --driver-memory 5g --num-executors 15 --executor-memory 4g --executor-cores 4
```

    -   ```
--master yarn-client --driver-memory 5g --num-executors 10 --executor-memory 9g --executor-cores 6
```

    原则上，按照上述的公式计算出来的需要资源不超过集群的最大资源量就可以。但在实际场景中，因为系统、HDFS以及E-MapReduce的服务会需要使用core和mem资源，如果把core和mem都占用完了，反而会导致性能的下降，甚至无法运行。

    executor-cores数一般也都会被设置成和集群的可使用核一致，因为如果设置的太多，CPU会频繁切换，性能并不会提高。

-   yarn-cluster模式

    当使用yarn-cluster模式后，Driver程序会被放到Worker节点上。会占用到Worker资源池里的资源，这时若想要多用一些这个集群的资源，请参见如下配置。

    ```
    --master yarn-cluster --driver-memory 5g --num-executors 15 --executor-memory 4g --executor-cores 4
    ```


## 配置建议

-   如果将内存设置的很大，要注意GC所产生的消耗。一般我们会推荐每一个executor的内存<=64G。
-   如果是进行HDFS读写的作业，建议是每个executor中使用<=5个并发来读写。
-   如果是进行OSS读写的作业，我们建议是将executor分布在不同的ECS上，这样可以将每一个ECS的带宽都用上。例如，有10台ECS，那么就可以配置num-executors=10，并设置合理的内存和并发。
-   如果作业中使用了非线程安全的代码，那么在设置executor-cores 的时候需要注意多并发是否会造成作业的不正常。如果会，那么推荐就设置 executor-cores=1。

