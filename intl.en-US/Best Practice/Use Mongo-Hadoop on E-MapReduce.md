# Use Mongo-Hadoop on E-MapReduce {#concept_l1r_pd1_kfb .concept}

Mongo-Hadoop is a component provided by MongoDB for Hadoop components to connect to MongoDB. Using Mongo-Hadoop is similar to using ES-Hadoop which is described in the previous topic. EMR has already integrated with Mongo-Hadoop. Users can directly use Mongo-Hadoop without any deployment configuration. This topic describes how to use Mongo-Hadoop using some examples.

## Preparation {#section_ujt_b21_kfb .section}

We use the same data model for the following examples:

```
{
  "id": long,
  "name": text,
  "age": integer,
  "birth": date
}
```

We write data into the specified collection \(similar to a table in a database\) in a MongoDB database. Therefore, we need to first ensure that the collection exists in the MongoDB database. First, run the MongoDB client program on a client node that can access the MongoDB database. You may need to download the client program from the MongoDB website and install it. Take the connection to ApsaraDB for MongoDB as an example:

```
mongo --host dds-xxxxxxxxxxxxxxxxxxxxx.mongodb.rds.aliyuncs.com:3717 --authenticationDatabase admin -u root -p 123456
```

The hostname of the MongoDB database is dds-xxxxxxxxxxxxxxxxxxxxx.mongodb.rds.aliyuncs.com. The port number is 3717. The actual port number depends on the MongoDB cluster. For an external MongoDB cluster that you have deployed on your own, the default port number is 27017. In this example, the password is set to 123456 using the -p option. Run the following commands in CLI to create a collection named employees in the company database:

```
> use company;
> db.createCollection("employees")
```

Prepare a file where each row is a JSON object as follows.

```
{"id": 1, "name": "zhangsan", "birth": "1990-01-01", "addr": "No. 969, wenyixi Rd, yuhang, hangzhou"}
{"id": 2, "name": "lisi", "birth": "1991-01-01", "addr": "No. 556, xixi Rd, xihu, hangzhou"}
{"id": 3, "name": "wangwu", "birth": "1992-01-01", "addr": "No. 699 wangshang Rd, binjiang, hangzhou"}
```

Save the file to the specified directory on HDFS \(for example, the file path can be /mongo-hadoop/employees.txt\).

## Mapreduce {#section_xlq_1f1_kfb .section}

In the following example, we read JSON files in the /mongo-hadoop directory on HDFS and write each row in the JSON files as a document to the MongoDB database.

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

    conf.set("mongo.output.uri", "mongodb://<your_username>:<your_password>@dds-xxxxxxxxxxxxxxxxxxxxx.mongodb.rds.aliyuncs.com:3717/company.employees? authSource=admin");

    Job job = Job.getInstance(conf);
    job.setInputFormatClass(TextInputFormat.class);
    job.setOutputFormatClass(MongoOutputFormat.class);
    job.setOutputKeyClass(Text.class);
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

Compile and package the code into a JAR file called mr-test.jar. Run the following command:

```
hadoop jar mr-test.jar com.aliyun.emr.Test -Dmapreduce.job.reduces=0 -libjars mr-test.jar /mongo-hadoop
```

After the execution is complete, you can view the results using the MongoDB client program:

```
> db.employees.find();
{ "_id" : "employee1", "id" : 1, "name" : "zhangsan", "birth" : "1990-01-01", "addr" : "No. 969, wenyixi Rd, yuhang, hangzhou" }
{ "_id" : "employee2", "id" : 2, "name" : "lisi", "birth" : "1991-01-01", "addr" : "No. 556, xixi Rd, xihu, hangzhou" }
{ "_id" : "employee3", "id" : 3, "name" : "wangwu", "birth" : "1992-01-01", "addr" : "No. 699 wangshang Rd, binjiang, hangzhou" }
```

## Spark {#section_svb_lf1_kfb .section}

In this example, we write data to a MongoDB database using Spark instead of MapReduce.

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
    outputConfig.set("mongo.output.uri", "mongodb://<your_username>:<your_password>@dds-xxxxxxxxxxxxxxxxxxxxx.mongodb.rds.aliyuncs.com:3717/company.employees? authSource=admin");

    // It is saved as a "Hadoop file." Actually, the data is written into the MongoDB database through the MongoOutputFormat class.
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

Package the code into a JAR file named spark-test.jar. Run the following command to write data.

```
spark-submit --master yarn --class com.aliyun.emr.Test spark-test.jar
```

After the writing has finished, you can use the MongoDB client to view the results.

## Hive {#section_tmd_pf1_kfb .section}

This example describes how to use Hive to read and write data in MongoDB databases through SQL statements.

First, run the hive command to enter CLI mode and create a table:

```
CREATE DATABASE IF NOT EXISTS company;
```

You need to create an external table that is stored in a MongoDB database. Before you do that, create a MongoDB collection named employees as described in the Preparation section.

Go back to CLI mode, execute the following SQL statements to create an external table. Connection to MongoDB is set through the TBLPROPERTIES clause.

```
CREATE EXTERNAL TABLE IF NOT EXISTS employees(
  id BIGINT,
  name STRING,
  birth STRING,
  addr STRING
)
STORED BY 'com.mongodb.hadoop.hive.MongoStorageHandler'
WITH SERDEPROPERTIES('mongo.columns.mapping'='{"id":"_id"}')
TBLPROPERTIES('mongo.uri'='mongodb://<your_username>:<your_password>@dds-xxxxxxxxxxxxxxxxxxxxx.mongodb.rds.aliyuncs.com:3717/company.employees? authSource=admin');
```

**Note:** Values of the id column in Hive are mapped to values of the \_id column in MongoDB through SERDEPROPERTIES. You can map column values as needed. Note that the data type of the birth column is set to STRING. The reason is that Hive and MongoDB handle DATE format differently. After Hive sends data in DATE format to MongoDB, NULL may be returned when the data is queried in Hive.

Insert some data into the table:

```
INSERT INTO TABLE employees VALUES (1, "zhangsan", "1990-01-01","No. 969, wenyixi Rd, yuhang, hangzhou");
INSERT INTO TABLE employees VALUES (2, "lisi", "1991-01-01", "No. 556, xixi Rd, xihu, hangzhou");
INSERT INTO TABLE employees VALUES (3, "wangwu", "1992-01-01", "No. 699 wangshang Rd, binjiang, hangzhou");
```

Execute the following statement to see the results:

```
SELECT * FROM employees LIMIT 100;
OK
1    zhangsan    1990-01-01    No. 969, wenyixi Rd, yuhang, hangzhou
2    lisi    1991-01-01    No. 556, xixi Rd, xihu, hangzhou
3    wangwu    1992-01-01    No. 699 wangshang Rd, binjiang, hangzhou
```

