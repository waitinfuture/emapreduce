# 在 EMR 中使用 ES-Hadoop {#concept_fh4_5tz_jfb .concept}

## 背景 {#section_zd3_35z_jfb .section}

ES-Hadoop 是 Elasticsearch\(ES\) 推出的专门用于对接 Hadoop 生态的工具，使得用户可以使用 Mapreduce\(MR\)、Spark、Hive 等工具处理 ES 上的数据（ES-Hadoop 还包含另外一部分：将 ES 的索引 snapshot 到 HDFS，对于该内容本文暂不讨论）。众所周知，Hadoop 生态的长处是处理大规模数据集，但是其缺点也很明显，就是当用于交互式分析时，查询时延会比较长。而 ES 是这方面的好手，对于很多查询类型，特别是 ad-hoc 查询，基本可以做到秒级。ES-Hadoop 的推出提供了一种组合两者优势的可能性。使用 ES-Hadoop，用户只需要对自己代码做出很小的改动，即可以快速处理存储在 ES 中的数据，并且能够享受到 ES 带来的加速效果。

ES-Hadoop 的逻辑是将 ES 作为 MR/Spark/Hive 等数据处理引擎的“数据源”，在计算存储分离的架构中扮演存储的角色。这和 MR/Spark/Hive 的其他数据源并无差异。但相对于其他数据源， ES 具有更快的数据选择过滤能力。这种能力正是分析引擎最为关键的能力之一。

EMR 中已经添加了对 ES-Hadoop 的支持，用户不需要做任何配置即可使用 ES-Hadoop。下面我们通过几个例子，介绍如何在 EMR 中使用 ES-Hadoop。

## 准备 {#section_vst_2zz_jfb .section}

ES 有自动创建索引的功能，能够根据输入数据自动推测数据类型。这个功能在某些情况下很方便，避免了用户很多额外的操作，但是也产生了一些问题。最重要的问题是 ES 推测的类型和我们预期的类型不一致。比如我们定义了一个字段叫 age，INT 型，在 ES 索引中可能被索引成了 LONG 型。在执行一些操作的时候会带来类型转换问题。为此，我们建议手动创建索引。

在下面几个例子中，我们将使用同一个索引 company和一个类型 employees（ES 索引可以看成一个 database，类型可以看做 database 下的一张表），该类型定义了四个字段（字段类型均为 ES 定义的类型）：

```
{
  "id": long,
  "name": text,
  "age": integer,
  "birth": date
}
```

在 kibana 中运行如下命令创建索引（或用相应的 curl 命令）：

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

**说明：** 其中 settings 中的索引参数可根据需要设定，也可以不具体设定 settings。

准备一个文件，每一行为一个 json 对象，如下所示：

```
{"id": 1, "name": "zhangsan", "birth": "1990-01-01", "addr": "No.969, wenyixi Rd, yuhang, hangzhou"}
{"id": 2, "name": "lisi", "birth": "1991-01-01", "addr": "No.556, xixi Rd, xihu, hangzhou"}
{"id": 3, "name": "wangwu", "birth": "1992-01-01", "addr": "No.699 wangshang Rd, binjiang, hangzhou"}
```

并保存至 HDFS 指定目录（如 /es-hadoop/employees.txt）。

## Mapreduce {#section_wwl_q11_kfb .section}

在下面这个例子中，我们读取hdfs 上 /es-hadoop目录下的 json 文件，并将这些 json 文件中的每一行作为一个 document 写入 es。写入过程由 EsOutputFormat 在 map 阶段完成。

这里对 ES 的设置主要是如下几个选项:

-   es.nodes: ES 节点，为 host:port 格式。对于阿里云托管式 ES，此处应为阿里云提供的 ES 访问域名
-   es.net.http.auth.user: 用户名
-   es.net.http.auth.pass: 用户密码
-   es.nodes.wan.only: 对于阿里云托管式 ES，此处应设置为 true
-   es.resource: ES 索引和类型
-   es.input.json: 如果原始文件为 json 类型，设置为 true，否则，需要在 map 函数中自己解析原始数据，生成相应的 Writable 输出

**说明：** 关闭 map 和 reduce 的推测执行机制

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

将该代码编译打包为mr-test.jar, 上传至装有 emr 客户端的机器（如 gateway，或者 EMR cluster 任意一台机器）。

在装有 EMR 客户端的机器上运行如下命令执行 mapreduce 程序：

```
hadoop jar mr-test.jar com.aliyun.emr.Test -Dmapreduce.job.reduces=0 -libjars mr-test.jar /es-hadoop
```

即可完成向 ES 写数据。具体写入的数据可以通过 kibana 查询（或者通过相应的 curl 命令）:

```
GET
{
  "query": {
    "match_all": {}
  }
}
```

## Spark {#section_wsl_nb1_kfb .section}

本示例同 mapreduce 一样，也是向 ES 的一个索引写入数据，只不过是通过 spark 来执行。这里 spark 借助 JavaEsSpark 类将一份 RDD 持久化到 es。同上述 mapreduce 程序一样，用户也需要注意上述几个选项的设置。

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
    JavaRDD<Map<Object, ?>> javaRDD = ss.read().text("hdfs://emr-header-1:9000/es-hadoop/employees.txt")
        .javaRDD().map((Function<Row, Map<Object, ?>>) row -> ImmutableMap.of("employees" + employeesNo.getAndAdd(1), row.mkString()));

    JavaEsSpark.saveToEs(javaRDD, "company/employees");
  }
}
```

将其打包成 spark-test.jar，运行如下命令执行写入过程：

```
spark-submit --master yarn --class com.aliyun.emr.Test spark-test.jar
```

待任务执行完毕后可以使用 kibana 或者 curl 命令查询结果。

除了 spark rdd 操作，es-hadoop 还提供了使用 sparksql 来读写 ES。详细请参考 ES-Hadoop [官方页面](https://www.elastic.co/guide/en/elasticsearch/hadoop/current/spark.html)。

## Hive {#section_tyw_jc1_kfb .section}

这里展示使用 Hive 通过 SQL 来读写 ES 的方法。

首先运行 hive命令进入交互式环境，先创建一个表：

```
CREATE DATABASE IF NOT EXISTS company;
```

之后创建一个外部表，表存储在 ES 上，通过 TBLPROPERTIES 来设置对接 ES 的各个选项：

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

**说明：** 在 Hive 表中我们将 birth 设置成了 TIMESTAMP 类型，而在 ES 中我们将其设置成了 DATE 型。这是因为 Hive 和 ES 对于数据格式处理不一致。在写入时，Hive 将原始 date 转换后发送给 ES 可能会解析失败，相反在读取时，ES 返回的格式 Hive 也可能解析失败。参见[这里](https://www.elastic.co/guide/en/elasticsearch/hadoop/current/mapping.html)。

往表中插入一些数据：

```
INSERT INTO TABLE employees VALUES (1, "zhangsan", "1990-01-01","No.969, wenyixi Rd, yuhang, hangzhou");
INSERT INTO TABLE employees VALUES (2, "lisi", "1991-01-01", "No.556, xixi Rd, xihu, hangzhou");
INSERT INTO TABLE employees VALUES (3, "wangwu", "1992-01-01", "No.699 wangshang Rd, binjiang, hangzhou");
```

执行查询即可看到结果：

```
SELECT * FROM employees LIMIT 100;
OK
1    zhangsan    1990-01-01    No.969, wenyixi Rd, yuhang, hangzhou
2    lisi    1991-01-01    No.556, xixi Rd, xihu, hangzhou
3    wangwu    1992-01-01    No.699 wangshang Rd, binjiang, hangzhou
```

