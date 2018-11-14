# Use Spark Streaming to process Kafka data {#concept_gh4_zfh_hfb .concept}

This topic describes how to run Spark Streaming jobs in Hadoop clusters of E-MapReduce to process data in Kafka clusters.

## Programming reference {#section_twr_bgh_hfb .section}

The Hadoop and Kafka clusters in E-MapReduce are based on completely open source software. Therefore, you can refer to the corresponding official documentation during your development.

-   Spark official documentation: [streaming-kafka-integration](http://spark.apache.org/docs/latest/streaming-kafka-integration.html)
-   Spark official documentation: [structured-streaming-kafka-integration](http://spark.apache.org/docs/latest/structured-streaming-kafka-integration.html)
-   E-MapReduce-demo: [Github](https://github.com/aliyun/aliyun-emapreduce-demo)

## Allow Spark Streaming jobs to access Kerberos Kafka clusters {#section_vwr_bgh_hfb .section}

E-MapReduce allows you to create Kafka clusters based on Kerberos authentication. Jobs in Hadoop clusters can access Kerberos Kafka clusters in two ways:

-   Non-Kerberos Hadoop cluster: Provide the kafka\_client\_jaas.conf file for Kerberos authentication of Kafka clusters.
-   Kerberos Hadoop cluster: Cross-domain communication based on Kerberos clusters. Provide the kafka\_client\_jaas.conf file for Kerberos authentication of Hadoop clusters.

Both methods require that you provide the kafka\_client\_jaas.conf file for Kerberos authentication when you run a job.

The file format of kafka\_client\_jaas.conf is as follows:

```
KafkaClient {
    com.sun.security.auth.module.Krb5LoginModule required
    useKeyTab=true
    storeKey=true
    serviceName="kafka"
    keyTab="/path/to/kafka.keytab"
    principal="kafka/emr-header-1.cluster-12345@EMR. 12345. COM";
};
```

For more information about how to obtain the keytab file, see **Kerberos authentication** in the User Guide.

## Allow Spark Streaming jobs to access Kerberos Kafka clusters {#section_zwr_bgh_hfb .section}

When you run Spark Streaming jobs to access Kerberos Kafka clusters, you can provide the kafka\_client\_jaas.conf file and the kafka.keytab file as needed in the spark-submit command line parameters.

```
spark-submit --conf spark.driver.extraJavaOptions=-Djava.security.auth.login.config={{PWD}}/kafka_client_jaas.conf --conf spark.executor.extraJavaOptions=-Djava.security.auth.login.config={{PWD}}/kafka_client_jaas.conf --files /local/path/to/kafka_client_jaas.conf,/local/path/to/kafka.keytab --class xx.xx.xx.KafkaSample --num-executors 2 --executor-cores 2 --executor-memory 1g --master yarn-cluster xxx.jar arg1 arg2 arg3
```

Note that in the kafka\_client\_jaas.conf file, the keytab file path must be a relative path. Make sure that you configure the path in the following format:

```
KafkaClient {
    com.sun.security.auth.module.Krb5LoginModule required
    useKeyTab=true
    storeKey=true
    serviceName="kafka"
    keyTab="kafka.keytab"
    principal="kafka/emr-header-1.cluster-12345@EMR. 12345. COM";
};
```

