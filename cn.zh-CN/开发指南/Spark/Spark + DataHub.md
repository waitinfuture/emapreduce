# Spark + DataHub {#concept_iwd_43h_hfb .concept}

本文介绍在E-MapReduce的Hadoop集群运行Spark Streaming作业消费DataHub数据，统计数据个数并打印出来。

## Spark 接入 DataHub {#section_h4b_q3h_hfb .section}

-   准备工作

    使用DataHub的订阅功能订阅Topic，详细步骤见[DataHub订阅功能介绍](https://help.aliyun.com/document_detail/57622.html?spm=a2c4g.11174283.6.557.255d63eflzHV3m)

-   消费DataHub数据

    运行Spark Streaming作业消费DataHub数据有两种使用方式：

    -   指定特定的shard id，消费该shard id的数据:

        ```
        datahubStream = DatahubUtils.createStream(
                  ssc,
                  project, // DataHub Project
                  topic, // DataHub topic
                  subId, // DataHub的订阅ID
                  accessKeyId,
                  accessKeySecret,
                  endpoint, // DataHub endpoint
                  shardId, // DataHub topic其中的一个ShardId
                  read, // 对DataHub数据RecordEntry的处理
                  StorageLevel.MEMORY_AND_DISK)
        datahubStream.foreachRDD(rdd => println(rdd.count()))
        
        // 对DataHub数据RecordEntry的处理函数，取出RecordEntry中第一个field的数据
        def read(record: RecordEntry): String = {
          record.getString(0)
        }
        ```

    -   消费所有shard的数据:

        ```
        datahubStream = DatahubUtils.createStream(
                  ssc,
                  project, // DataHub Project
                  topic, // DataHub topic
                  subId, // DataHub的订阅ID
                  accessKeyId,
                  accessKeySecret,
                  endpoint, // DataHub endpoint
                  read, // 对DataHub数据RecordEntry的处理
                  StorageLevel.MEMORY_AND_DISK)
        datahubStream.foreachRDD(rdd => println(rdd.count()))
        
        // 对DataHub数据RecordEntry的处理函数，取出RecordEntry中第一个field的数据
        def read(record: RecordEntry): String = {
          record.getString(0)
        }
        ```


## 附录 {#section_td1_gjh_hfb .section}

完整示例代码请参考：

-   [Spark对接DataHub](https://github.com/aliyun/aliyun-emapreduce-sdk/blob/master-2.x/examples/src/main/scala/com/aliyun/emr/examples/streaming/TestDatahub.scala)

