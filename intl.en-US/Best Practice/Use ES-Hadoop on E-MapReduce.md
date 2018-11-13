# Use ES-Hadoop on E-MapReduce {#concept_fh4_5tz_jfb .concept}

ES-Hadoop is a tool used to connect the Hadoop ecosystem provided by Elasticsearch \(ES\). It enables users to use tools such as MapReduce \(MR\), Spark, and Hive to process data in ES \(ES-Hadoop also supports taking a snapshot of ES indices and storing it in HDFS, which is not discussed in this topic\).

## Background {#section_zd3_35z_jfb .section}

We know that the advantage of the Hadoop ecosystem is processing large data sets. But the disadvantage is also obvious: interactive analysis can be delayed. ES is adept at many types of queries, especially ad-hoc queries. Subsecond response time has been reached. ES-Hadoop has combined both advantages. With ES-Hadoop, users only need to make small changes to the code for quickly processing data stored in ES. ES also provides acceleration.

ES-Hadoop uses ES as the data source of data processing engines, such as MR, Spark, and Hive. ES plays the role of storage in architectures where compute and storage are separated. This is the same for other data sources of MR, Spark, and Hive. But ES has faster data filtering ability compared with other data sources. This ability is one of the most critical abilities of an analytics engine.

EMR has already integrated with ES-Hadoop. Users can use ES-Hadoop directly without any configurations. The following examples introduce ES-Hadoop on EMR.

## Preparation {#section_vst_2zz_jfb .section}

ES can automatically create indices and identify data types based on input data. In some cases, this feature is helpful, by avoiding many actions by users. However, it also cause problems. The biggest problem is that sometimes the data types identified by ES are not correct. For example, we define a field called age. The data type of this column is INT but it may be identified as LONG in the ES index. Users need to convert data types when performing some specified actions. We recommend that you create indices manually to avoid such problems.

In the following examples, we use the company index and the employees' type \(you can consider an ES index as a database and a type as a table in the database\). This type defines four fields \(field types are defined by ES\).

```
{
  "id": long,
  "name": text,
  "age": integer,
  "birth": date
}
```

Run the following commands to create an index in Kibana \(you can also use cURL commands\):

```
PUT company
{
  "mappings": {
    "employees": {
      "properties": {
        "id": {
          "type": "long"
        },
        "name": {
          "type": "text",
          "fields": {
            "keyword": {
              "type": "keyword",
              "ignore_above": 256
            }
          }
        },
        "birth": {
          "type": "date"
        },
        "addr": {
          "type": "text"
        }
      }
    }
  },
  "settings": {
    "index": {
      "number_of_shards": "5",
      "number_of_replicas": "1"
    }
  }
}
```

**Note:** Specify the index parameters in settings as needed. This step is optional.

Prepare a file where each row is a JSON object as follows:

```
{"id": 1, "name": "zhangsan", "birth": "1990-01-01", "addr": "No. 969, wenyixi Rd, yuhang, hangzhou"}
{"id": 2, "name": "lisi", "birth": "1991-01-01", "addr": "No. 556, xixi Rd, xihu, hangzhou"}
{"id": 3, "name": "wangwu", "birth": "1992-01-01", "addr": "No. 699 wangshang Rd, binjiang, hangzhou"}
```

Save the file to the specified directory in HDFS \(for example, /es-hadoop/employees.txt\).

## Mapreduce {#section_wwl_q11_kfb .section}

In the following example, we read the JSON files in the /es-hadoop directory in HDFS and write each row in the JSON files into ES as a document. Writing is finished in the map stage through EsOutputFormat.

Use the following options to set ES.

-   es.nodes: ES nodes. The formats is host:port. For ES hosted on Alibaba Cloud, set the value to the endpoint of ES provided by Alibaba Cloud.
-   es.net.http.auth.user: Username.
-   es.net.http.auth.pass: Password.
-   es.nodes.wan.only: For ES hosted on Alibaba Cloud, set the value to true.
-   es.resource: The indices and types of ES.
-   es.input.json: If the input file is in JSON format, set the value to true. Otherwise, you need to parse the input data using the map\(\) function and output the corresponding Writable class.

**Note:** Disable speculative execution for map tasks and reduce tasks

```
package com.aliyun.emr;

import java.io.IOException;
import org.apache.hadoop.conf.Configuration;
import org.apache.hadoop.fs.Path;
import org.apache.hadoop.io.NullWritable;
import org.apache.hadoop.io.Text;
import org.apache.hadoop.mapreduce.Job;
import org.apache.hadoop.mapreduce.Mapper;
import org.apache.hadoop.mapreduce.lib.input.FileInputFormat;
import org.apache.hadoop.mapreduce.lib.input.TextInputFormat;
import org.apache.hadoop.util.GenericOptionsParser;
import org.apache.hadoop.util.Tool;
import org.apache.hadoop.util.ToolRunner;
import org.elasticsearch.hadoop.mr.EsOutputFormat;

public class Test implements Tool {

  private Configuration conf;

  @Override
  public int run(String[] args) throws Exception {

    String[] otherArgs = new GenericOptionsParser(conf, args).getRemainingArgs();

    conf.setBoolean("mapreduce.map.speculative", false);
    conf.setBoolean("mapreduce.reduce.speculative", false);
    conf.set("es.nodes", "<your_es_host>:9200");
    conf.set("es.net.http.auth.user", "<your_username>");
    conf.set("es.net.http.auth.pass", "<your_password>");
    conf.set("es.nodes.wan.only", "true");
    conf.set("es.resource", "company/employees");
    conf.set("es.input.json", "yes");

    Job job = Job.getInstance(conf);
    job.setInputFormatClass(TextInputFormat.class);
    job.setOutputFormatClass(EsOutputFormat.class);
    job.setMapOutputKeyClass(NullWritable.class);
    job.setMapOutputValueClass(Text.class);
    job.setJarByClass(Test.class);
    job.setMapperClass(EsMapper.class);

    FileInputFormat.setInputPaths(job, new Path(otherArgs[0]));

    return job.waitForCompletion(true) ? 0 : 1;
  }

  @Override
  public void setConf(Configuration conf) {
    this.conf = conf;
  }

  @Override
  public Configuration getConf() {
    return conf;
  }

  public static class EsMapper extends Mapper<Object, Text, NullWritable, Text> {
    private Text doc = new Text();

    @Override
    protected void map(Object key, Text value, Context context) throws IOException, InterruptedException {
      if (value.getLength() > 0) {
        doc.set(value);
        context.write(NullWritable.get(), doc);
      }
    }
  }

  public static void main(String[] args) throws Exception {
    int ret = ToolRunner.run(new Test(), args);
    System.exit(ret);
  }
}
```

Compile and package the code into a JAR file called mr-test.jar. Submit it to an instance that has installed an EMR client program \(such as a gateway, or any node in an EMR cluster\).

Run the following commands on any node that has installed an EMR client to run the MapReduce program:

```
hadoop jar mr-test.jar com.aliyun.emr.Test -Dmapreduce.job.reduces=0 -libjars mr-test.jar /es-hadoop
```

At this point, writing data to ES has finished. You can query the written data through Kibana \( or by using the cURL commands\).

```
GET
{
  "query": {
    "match_all": {}
  }
}
```

## Spark {#section_wsl_nb1_kfb .section}

In this example, we write data to an index in ES using Spark instead of MapReduce. Spark persists a resilient distributed dataset \(RDD\) to ES using the JavaEsSpark class. Users also need to use the options mentioned above in the MapReduce section to set ES.

```
package com.aliyun.emr;

import java.util.Map;
import java.util.concurrent.atomic.AtomicInteger;
import org.apache.spark.SparkConf;
import org.apache.spark.SparkContext;
import org.apache.spark.api.java.JavaRDD;
import org.apache.spark.api.java.function.Function;
import org.apache.spark.sql.Row;
import org.apache.spark.sql.SparkSession;
import org.elasticsearch.spark.rdd.api.java.JavaEsSpark;
import org.spark_project.guava.collect.ImmutableMap;

public class Test {

  public static void main(String[] args) {
    SparkConf conf = new SparkConf();
    conf.setAppName("Es-test");
    conf.set("es.nodes", "<your_es_host>:9200");
    conf.set("es.net.http.auth.user", "<your_username>");
    conf.set("es.net.http.auth.pass", "<your_password>");
    conf.set("es.nodes.wan.only", "true");

    SparkSession ss = new SparkSession(new SparkContext(conf));
    final AtomicInteger employeesNo = new AtomicInteger(0);
    JavaRDD<Map<Object, ? >> javaRDD = ss.read().text("hdfs://emr-header-1:9000/es-hadoop/employees.txt")
        .javaRDD().map((Function<Row, Map<Object, ? >>) row -> ImmutableMap.of("employees" + employeesNo.getAndAdd(1), row.mkString()));

    JavaEsSpark.saveToEs(javaRDD, "company/employees");
  }
}
```

Package the code in a JAR file called spark-test.jar. Run the following command to write data:

```
spark-submit --master yarn --class com.aliyun.emr.Test spark-test.jar
```

After the task has finished, you can query the results through Kibana or the cURL commands.

In addition to Spark RDD. ES-Hadoop also provides a Spark SQL component to read and write ES data. For more information, see the [official website](https://www.elastic.co/guide/en/elasticsearch/hadoop/current/spark.html) of ES-Hadoop.

## Hive {#section_tyw_jc1_kfb .section}

This example introduces SQL statements to read and write ES data through Hive.

First, run the hivecommand to enter CLI and create a table:

```
CREATE DATABASE IF NOT EXISTS company;
```

Then create an external table that is stored in ES. Specify the option using TBLPROPERTIES.

```
CREATE EXTERNAL table IF NOT EXISTS employees(
  id BIGINT,
  name STRING,
  birth TIMESTAMP,
  addr STRING
)
STORED BY 'org.elasticsearch.hadoop.hive.EsStorageHandler'
TBLPROPERTIES(
    'es.resource' = 'tpcds/ss',
    'es.nodes' = '<your_es_host>',
    'es.net.http.auth.user' = '<your_username>',
    'es.net.http.auth.pass' = '<your_password>',
    'es.nodes.wan.only' = 'true',
    'es.resource' = 'company/employees'
);
```

**Note:** We set the data type of the birth columns to TIMESTAMP in the Hive table. In ES, we set it to DATE. This is because Hive and EC handle data types differently. Parsing of converted date data can fail when Hive writes data to ES. In contrast, parsing of returned data can also fail when Hive reads ES data. For more information, click [here](https://www.elastic.co/guide/en/elasticsearch/hadoop/current/mapping.html).

Insert some data into the table:

```
INSERT INTO TABLE employees VALUES (1, "zhangsan", "1990-01-01","No. 969, wenyixi Rd, yuhang, hangzhou");
INSERT INTO TABLE employees VALUES (2, "lisi", "1991-01-01", "No. 556, xixi Rd, xihu, hangzhou");
INSERT INTO TABLE employees VALUES (3, "wangwu", "1992-01-01", "No. 699 wangshang Rd, binjiang, hangzhou");
```

Execute queries to view the results:

```
SELECT * FROM employees LIMIT 100;
OK
1    zhangsan    1990-01-01    No. 969, wenyixi Rd, yuhang, hangzhou
2    lisi    1991-01-01    No. 556, xixi Rd, xihu, hangzhou
3    wangwu    1992-01-01    No. 699 wangshang Rd, binjiang, hangzhou
```

