# 在 EMR 中使用 Mongo-Hadoop {#concept_l1r_pd1_kfb .concept}

Mongo-Hadoop 是 MongoDB 推出的用于 Hadoop 系列组件连接 MongoDB 的组件。其原理跟我们上一篇文章介绍的 ES-Hadoop 类似。EMR 中已经集成了 Mongo-Hadoop，用户不用做任何部署配置，即可使用 Mongo-Hadoop。下面我们通过几个例子来展示一下 Mongo-Hadoop 的用法。

## 准备 {#section_ujt_b21_kfb .section}

在下面这几个例子中，我们使用一个统一的数据模型：

```
{
  "id": long,
  "name": text,
  "age": integer,
  "birth": date
}
```

由于我们是要通过 Mongo-Hadoop 向 MongoDB 的特定 collection （可以理解成数据库中的表）写数据，因此需要首先确保 MongoDB 上存在这个 collection。为此，首先需要在一台能够连接到 MongoDB 的客户机上运行 mongo client（你可能需要安装一下客户端程序，客户端程序可在 mongo 官网下载）。我们以连接阿里云数据库 MongoDB 版为例：

```
mongo --host dds-xxxxxxxxxxxxxxxxxxxxx.mongodb.rds.aliyuncs.com:3717 --authenticationDatabase admin -u root -p 123456
```

其中 dds-xxxxxxxxxxxxxxxxxxxxx.mongodb.rds.aliyuncs.com 为 MongoDB 的主机名，3717 为端口号（该端口号根据您的 MongoDB 集群而定，对于自建集群，默认为 27017），-p 为密码（这里假设密码为 123456）。进入交互式页面，运行如下命令，在 company 数据库下创建名为 employees 的 collection：

```
> use company;
> db.createCollection("employees")
```

准备一个文件，每一行为一个 json 对象，如下所示：

```
{"id": 1, "name": "zhangsan", "birth": "1990-01-01", "addr": "No.969, wenyixi Rd, yuhang, hangzhou"}
{"id": 2, "name": "lisi", "birth": "1991-01-01", "addr": "No.556, xixi Rd, xihu, hangzhou"}
{"id": 3, "name": "wangwu", "birth": "1992-01-01", "addr": "No.699 wangshang Rd, binjiang, hangzhou"}
```

并保存至 HDFS 指定目录（如 /mongo-hadoop/employees.txt）。

## Mapreduce {#section_xlq_1f1_kfb .section}

在下面这个例子中，我们读取 HDFS 上/mongo-hadoop目录下的 json 文件，并将这些 json 文件中的每一行作为一个 document 写入 MongoDB。

```
package com.aliyun.emr;

import com.mongodb.BasicDBObject;
import com.mongodb.hadoop.MongoOutputFormat;
import com.mongodb.hadoop.io.BSONWritable;
import java.io.IOException;
import org.apache.hadoop.conf.Configuration;
import org.apache.hadoop.fs.Path;
import org.apache.hadoop.io.Text;
import org.apache.hadoop.mapreduce.Job;
import org.apache.hadoop.mapreduce.Mapper;
import org.apache.hadoop.mapreduce.lib.input.FileInputFormat;
import org.apache.hadoop.mapreduce.lib.input.TextInputFormat;
import org.apache.hadoop.util.GenericOptionsParser;
import org.apache.hadoop.util.Tool;
import org.apache.hadoop.util.ToolRunner;

public class Test implements Tool {

  private Configuration conf;

  @Override
  public int run(String[] args) throws Exception {

    String[] otherArgs = new GenericOptionsParser(conf, args).getRemainingArgs();

    conf.set("mongo.output.uri", "mongodb://<your_username>:<your_password>@dds-xxxxxxxxxxxxxxxxxxxxx.mongodb.rds.aliyuncs.com:3717/company.employees?authSource=admin");

    Job job = Job.getInstance(conf);
    job.setInputFormatClass(TextInputFormat.class);
    job.setOutputFormatClass(MongoOutputFormat.class);
    job.setMapOutputKeyClass(Text.class);
    job.setMapOutputValueClass(BSONWritable.class);

    job.setJarByClass(Test.class);
    job.setMapperClass(MongoMapper.class);

    FileInputFormat.setInputPaths(job, new Path(otherArgs[0]));

    return job.waitForCompletion(true) ? 0 : 1;
  }

  @Override
  public Configuration getConf() {
    return conf;
  }

  @Override
  public void setConf(Configuration conf) {
    this.conf = conf;
  }

  public static class MongoMapper extends Mapper<Object, Text, Text, BSONWritable> {

    private BSONWritable doc = new BSONWritable();
    private int employeeNo = 1;
    private Text id;

    @Override
    protected void map(Object key, Text value, Context context) throws IOException, InterruptedException {
      if (value.getLength() > 0) {
        doc.setDoc(BasicDBObject.parse(value.toString()));
        id = new Text("employee" + employeeNo++);
        context.write(id, doc);
      }
    }
  }

  public static void main(String[] args) throws Exception {
    int ret = ToolRunner.run(new Test(), args);
    System.exit(ret);
  }
}
```

将该代码编译打包为mr-test.jar， 运行：

```
hadoop jar mr-test.jar com.aliyun.emr.Test -Dmapreduce.job.reduces=0 -libjars mr-test.jar /mongo-hadoop
```

待任务执行完毕后可以使用 MongoDB 客户端查询结果：

```
> db.employees.find();
{ "_id" : "employee1", "id" : 1, "name" : "zhangsan", "birth" : "1990-01-01", "addr" : "No.969, wenyixi Rd, yuhang, hangzhou" }
{ "_id" : "employee2", "id" : 2, "name" : "lisi", "birth" : "1991-01-01", "addr" : "No.556, xixi Rd, xihu, hangzhou" }
{ "_id" : "employee3", "id" : 3, "name" : "wangwu", "birth" : "1992-01-01", "addr" : "No.699 wangshang Rd, binjiang, hangzhou" }
```

## Spark {#section_svb_lf1_kfb .section}

本示例同 Mapreduce 一样，也是向 MongoDB 写入数据，只不过是通过 Spark 来执行。

```
package com.aliyun.emr;

import com.mongodb.BasicDBObject;
import com.mongodb.hadoop.MongoOutputFormat;
import java.util.concurrent.atomic.AtomicInteger;
import org.apache.hadoop.conf.Configuration;
import org.apache.spark.SparkContext;
import org.apache.spark.api.java.JavaPairRDD;
import org.apache.spark.api.java.JavaRDD;
import org.apache.spark.api.java.function.Function;
import org.apache.spark.sql.Row;
import org.apache.spark.sql.SparkSession;
import org.bson.BSONObject;
import scala.Tuple2;

public class Test {

  public static void main(String[] args) {

    SparkSession ss = new SparkSession(new SparkContext());

    final AtomicInteger employeeNo = new AtomicInteger(0);
    JavaRDD<Tuple2<Object, BSONObject>> javaRDD =
        ss.read().text("hdfs://emr-header-1:9000/mongo-hadoop/employees.txt")
            .javaRDD().map((Function<Row, Tuple2<Object, BSONObject>>) row -> {
          BSONObject bson = BasicDBObject.parse(row.mkString());
          return new Tuple2<>("employee" + employeeNo.getAndAdd(1), bson);
        });

    JavaPairRDD<Object, BSONObject> documents = JavaPairRDD.fromJavaRDD(javaRDD);

    Configuration outputConfig = new Configuration();
    outputConfig.set("mongo.output.uri", "mongodb://<your_username>:<your_password>@dds-xxxxxxxxxxxxxxxxxxxxx.mongodb.rds.aliyuncs.com:3717/company.employees?authSource=admin");

    // 将其保存为一个 "hadoop 文件"，实际上通过 MongoOutputFormat 写入 mongo。
    documents.saveAsNewAPIHadoopFile(
        "file:///this-is-completely-unused",
        Object.class,
        BSONObject.class,
        MongoOutputFormat.class,
        outputConfig
    );
  }
}
```

将其打包成 spark-test.jar，运行如下命令执行写入过程：

```
spark-submit --master yarn --class com.aliyun.emr.Test spark-test.jar
```

待任务执行完毕后可以使用 MongoDB 客户端查询结果。

## Hive {#section_tmd_pf1_kfb .section}

这里展示使用 Hive 通过 SQL 来读写 MongoDB 的方法。

首先运行 hive命令进入交互式环境，先创建一个表：

```
CREATE DATABASE IF NOT EXISTS company;
```

之后创建一个外部表，表存储在 MongoDB 上。但是创建外部表之前，注意像第一小节中介绍的，需要首先创建 MongoDB Collection —— employees。

下面回到 Hive 交互式控制台，运行如下 SQL 创建一个外部表，MongoDB 连接通过 TBLPROPERTIES 来设置：

```
CREATE EXTERNAL TABLE IF NOT EXISTS employees(
  id BIGINT,
  name STRING,
  birth STRING,
  addr STRING
)
STORED BY 'com.mongodb.hadoop.hive.MongoStorageHandler'
WITH SERDEPROPERTIES('mongo.columns.mapping'='{"id":"_id"}')
TBLPROPERTIES('mongo.uri'='mongodb://<your_username>:<your_password>@dds-xxxxxxxxxxxxxxxxxxxxx.mongodb.rds.aliyuncs.com:3717/company.employees?authSource=admin');
```

**说明：** 这里通过 SERDEPROPERTIES 把 Hive 的字段id和 MongoDB 的字段 \_id 做了映射（用户可以根据自身需要选择做或者不做某些映射）。另外注意在 Hive 表中我们将 birth 设置成了 STRING 类型。这是因为 Hive 和 MongoDB 对于数据格式处理的不一致造成的问题。Hive 将原始 date 转换后发送给 MongoDB，然后再从 Hive 中查询可能会得到 NULL。

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

