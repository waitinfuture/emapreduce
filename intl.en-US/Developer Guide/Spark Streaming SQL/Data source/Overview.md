# Overview {#concept_1181189 .concept}

This topic describes the types of data sources supported by Spark SQL and methods to process data in data sources.

## Supported data sources {#section_fr8_vkw_nda .section}

|Data source|Schema defined during table creation|Schema not defined during table creation|Read|Write|
|-----------|------------------------------------|----------------------------------------|----|-----|
|Kafka|✅|✅|✅|✅|
|LogHub|✅|None|✅|✅|
|Table Store|None|✅|None|✅|
|HBase|None|✅|None|✅|
|JDBC|None|✅|None|✅|
|Druid|None|✅|None|✅|
|Redis|None|✅|None|✅|

## Methods to process data in data sources {#section_tlk_iic_ky4 .section}

Spark SQL can use command lines or workflows to process data in data sources.

-   Command line

    -   Download the [data source JAR package](https://github.com/aliyun/aliyun-emapreduce-sdk/tree/master-2.x/jars/datasources/latest) that has been precompiled.

        The JAR package contains the implementation and related dependency packages of the LogHub, Table Store, HBase, JDBC, and Redis data sources. You can use all these data sources by downloading the JAR package once. The packages for the Kafka and Druid data sources are not contained in this JAR package and will be supplemented later. For more information, see [Release notes](https://github.com/aliyun/aliyun-emapreduce-sdk/releases).

    -   Use the streaming-sql command line for interactive development.

        ``` {#codeblock_189_dz3_b9c}
        [hadoop@emr-header-1 ~]# streaming-sql --master yarn-client --jars emr-datasources_shaded_2.11-${version}.jar --driver-class-path emr-datasources_shaded_2.11-${version}.jar
        ```

    -   You can also use the -f or -e parameter to submit your SQL statement.
    -   If you want to execute a streaming job for a long time without exiting Spark SQL, you can run the nohup command to ignore the HUP \(hangup\) signal.
-   Workflow

    For more information, see [Configure Streaming SQL jobs](../../../../intl.en-US/Data Development/Jobs/Configure Streaming SQL jobs.md#).


