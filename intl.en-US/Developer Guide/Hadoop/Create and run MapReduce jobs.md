# Create and run MapReduce jobs {#concept_mpt_zgm_hfb .concept}

This topic describes how to create and run a MapReduce job.

## Use OSS in MapReduce {#section_ahh_mhm_hfb .section}

To read and write data to OSS in MapReduce, configure the following parameters:

```
conf.set("fs.oss.accessKeyId", "${accessKeyId}");
    conf.set("fs.oss.accessKeySecret", "${accessKeySecret}");
    conf.set("fs.oss.endpoint","${endpoint}");
```

Parameter descriptions:

-   $\{accessKeyId\}: The AccessKey ID of your account.

-   $\{accessKeySecret\}: The AccessKey Secret corresponding to the AccessKey ID.

-   $\{endpoint\}: The network used for accessing OSS. It depends on the region in which your cluster exists, and the corresponding OSS must also be in the region in which your cluster exists.

    For more information about specific values, see [OSS endpoints](../../SP_21/DNOSS11827291/EN-US_TP_4350.dita#concept_zt4_cvy_5db).


## Word count {#section_jvz_phm_hfb .section}

The following example describes how to read text from OSS and calculate the word count. The procedure is as follows:

1.  Write the program

    Take the Java code as an example. Modify the WordCount sample on the official website of Hadoop as follows: Modify the instance by adding the configuration of the AccessKey ID and AccessKey Secret in the code so that the job has the permission to access OSS files.

    ```
    package org.apache.hadoop.examples;
     import java.io.IOException;
     import java.util.StringTokenizer;
     import org.apache.hadoop.conf.Configuration;
     import org.apache.hadoop.fs.Path;
     import org.apache.hadoop.io.IntWritable;
     import org.apache.hadoop.io.Text;
     import org.apache.hadoop.mapreduce.Job;
     import org.apache.hadoop.mapreduce.Mapper;
     import org.apache.hadoop.mapreduce.Reducer;
     import org.apache.hadoop.mapreduce.lib.input.FileInputFormat;
     import org.apache.hadoop.mapreduce.lib.output.FileOutputFormat;
     import org.apache.hadoop.util.GenericOptionsParser;
     public class EmrWordCount {
      public static class TokenizerMapper
            extends Mapper<Object, Text, Text, IntWritable>{
         private final static IntWritable one = new IntWritable(1);
         private Text word = new Text();
         public void map(Object key, Text value, Context context
                         ) throws IOException, InterruptedException {
           StringTokenizer itr = new StringTokenizer(value.toString());
           while (itr.hasMoreTokens()) {
             word.set(itr.nextToken());
             context.write(word, one);
           }
         }
       }
       public static class IntSumReducer
            extends Reducer<Text,IntWritable,Text,IntWritable> {
         private IntWritable result = new IntWritable();
         public void reduce(Text key, Iterable<IntWritable> values,
                            Context context
                            ) throws IOException, InterruptedException {
           int sum = 0;
           for (IntWritable val : values) {
             sum += val.get();
           }
           result.set(sum);
           context.write(key, result);
         }
       }
       public static void main(String[] args) throws Exception {
         Configuration conf = new Configuration();
         String[] otherArgs = new GenericOptionsParser(conf, args).getRemainingArgs();
         if (otherArgs.length < 2) {
           System.err.println("Usage: wordcount <in> [<in>...] <out>");
           System.exit(2);
         }
         conf.set("fs.oss.accessKeyId", "${accessKeyId}");
         conf.set("fs.oss.accessKeySecret", "${accessKeySecret}");
         conf.set("fs.oss.endpoint","${endpoint}");
         Job job = Job.getInstance(conf, "word count");
         job.setJarByClass(EmrWordCount.class);
         job.setMapperClass(TokenizerMapper.class);
         job.setCombinerClass(IntSumReducer.class);
         job.setReducerClass(IntSumReducer.class);
         job.setOutputKeyClass(Text.class);
         job.setOutputValueClass(IntWritable.class);
         for (int i = 0; i < otherArgs.length - 1; ++i) {
           FileInputFormat.addInputPath(job, new Path(otherArgs[i]));
         }
         FileOutputFormat.setOutputPath(job,
           new Path(otherArgs[otherArgs.length - 1]));
         System.exit(job.waitForCompletion(true) ? 0 : 1);
       }
     }
    ```

2.  Compile the program

    First, you need to configure the JDK and Hadoop environments and then perform the following operations:

    ```
    mkdir wordcount_classes
     javac -classpath ${HADOOP_HOME}/share/hadoop/common/hadoop-common-2.6.0.jar:${HADOOP_HOME}/share/hadoop/mapreduce/hadoop-mapreduce-client-core-2.6.0.jar:${HADOOP_HOME}/share/hadoop/common/lib/commons-cli-1.2.jar -d wordcount_classes EmrWordCount.java
     jar cvf wordcount.jar -C wordcount_classes .
    ```

3.  Create a job
    -   Upload the JAR file prepared in the previous step to OSS. Log on to the OSS website for detailed operations. Assume that the path of the JAR file on OSS is oss://emr/jars/wordcount.jar and the input and output paths are oss://emr/data/WordCount/Input and oss://emr/data/WordCount/Output.
    -   Create [E-MapReduce jobs](http://emr.console.aliyun.com/#/job/region/cn-hangzhou) as follows:

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17984/154286698613188_en-US.png)

4.  Create an execution plan

    Create an execution plan in E-MapReduce and add the created job to the execution plan. Select **Run Now** as the policy so that the WordCount job runs in the selected cluster.


## Manage MR jobs using Maven {#section_ymj_v3m_hfb .section}

When your project grows larger in size, it becomes considerably complicated to manage. We recommend that you use Maven or similar software management tools to manage projects. The procedure is as follows:

1.  Install Maven

    First, make sure that you have installed [Maven](http://maven.apache.org/index.html?spm=a2c4g.11186623.2.18.4c3d19d6YlAWbs).

2.  Generate the project framework.

    At the root directory of your project \(suppose the root directory of your project is D:/workspace\), execute the following commands:

    ```
    mvn archetype:generate -DgroupId=com.aliyun.emr.hadoop.examples -DartifactId=wordcountv2 -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
    ```

    Maven automatically generates an empty sample project at D:/workspace/wordcountv2 \(same with the artifactId you specified\) containing a basic pom.xml file and an App class \(the class package path is the same as the groupId you specified\).

3.  Add Hadoop dependencies

    Open the project with your favorite IDE and edit the pom.xml file. Add the following content to the dependencies:

    ```
    <dependency>
             <groupId>org.apache.hadoop</groupId>
             <artifactId>hadoop-mapreduce-client-common</artifactId>
             <version>2.6.0</version>
         </dependency>
         <dependency>
             <groupId>org.apache.hadoop</groupId>
             <artifactId>hadoop-common</artifactId>
             <version>2.6.0</version>
         </dependency>
    ```

4.  Write the program

    Add a new class named WordCount2.java at the same directory level as that of the App class under the com.aliyun.emr.hadoop.examples package. The content is as follows:

    ```
    package com.aliyun.emr.hadoop.examples;
     import java.io.BufferedReader;
     import java.io.FileReader;
     import java.io.IOException;
     import java.net.URI;
     import java.util.ArrayList;
     import java.util.HashSet;
     import java.util.List;
     import java.util.Set;
     import java.util.StringTokenizer;
     import org.apache.hadoop.conf.Configuration;
     import org.apache.hadoop.fs.Path;
     import org.apache.hadoop.io.IntWritable;
     import org.apache.hadoop.io.Text;
     import org.apache.hadoop.mapreduce.Job;
     import org.apache.hadoop.mapreduce.Mapper;
     import org.apache.hadoop.mapreduce.Reducer;
     import org.apache.hadoop.mapreduce.lib.input.FileInputFormat;
     import org.apache.hadoop.mapreduce.lib.output.FileOutputFormat;
     import org.apache.hadoop.mapreduce.Counter;
     import org.apache.hadoop.util.GenericOptionsParser;
     import org.apache.hadoop.util.StringUtils;
     public class WordCount2 {
         public static class TokenizerMapper
                 extends Mapper<Object, Text, Text, IntWritable>{
             static enum CountersEnum { INPUT_WORDS }
             private final static IntWritable one = new IntWritable(1);
             private Text word = new Text();
             private boolean caseSensitive;
             private Set<String> patternsToSkip = new HashSet<String>();
             private Configuration conf;
             private BufferedReader fis;
             @Override
             public void setup(Context context) throws IOException,
                     InterruptedException {
                 conf = context.getConfiguration();
                 caseSensitive = conf.getBoolean("wordcount.case.sensitive", true);
                 if (conf.getBoolean("wordcount.skip.patterns", true)) {
                     URI[] patternsURIs = Job.getInstance(conf).getCacheFiles();
                     for (URI patternsURI : patternsURIs) {
                         Path patternsPath = new Path(patternsURI.getPath());
                         String patternsFileName = patternsPath.getName().toString();
                         parseSkipFile(patternsFileName);
                     }
                 }
             }
             private void parseSkipFile(String fileName) {
                 try {
                     fis = new BufferedReader(new FileReader(fileName));
                     String pattern = null;
                     while ((pattern = fis.readLine()) ! = null) {
                         patternsToSkip.add(pattern);
                     }
                 } catch (IOException ioe) {
                     System.err.println("Caught exception while parsing the cached file '"
                             + StringUtils.stringifyException(ioe));
                 }
             }
             @Override
             public void map(Object key, Text value, Context context
             ) throws IOException, InterruptedException {
                 String line = (caseSensitive) ?
                         value.toString() : value.toString().toLowerCase();
                 for (String pattern : patternsToSkip) {
                     line = line.replaceAll(pattern, "");
                 }
                 StringTokenizer itr = new StringTokenizer(line);
                 while (itr.hasMoreTokens()) {
                     word.set(itr.nextToken());
                     context.write(word, one);
                     Counter counter = context.getCounter(CountersEnum.class.getName(),
                             CountersEnum.INPUT_WORDS.toString());
                     counter.increment(1);
                 }
             }
         }
         public static class IntSumReducer
                 extends Reducer<Text,IntWritable,Text,IntWritable> {
             private IntWritable result = new IntWritable();
             public void reduce(Text key, Iterable<IntWritable> values,
                                Context context
             ) throws IOException, InterruptedException {
                 int sum = 0;
                 for (IntWritable val : values) {
                     sum += val.get();
                 }
                 result.set(sum);
                 context.write(key, result);
             }
         }
         public static void main(String[] args) throws Exception {
             Configuration conf = new Configuration();
             conf.set("fs.oss.accessKeyId", "${accessKeyId}");
            conf.set("fs.oss.accessKeySecret", "${accessKeySecret}");
             conf.set("fs.oss.endpoint","${endpoint}");
             GenericOptionsParser optionParser = new GenericOptionsParser(conf, args);
             String[] remainingArgs = optionParser.getRemainingArgs();
             if (!( remainingArgs.length ! = 2 || remainingArgs.length ! = 4)) {
                 System.err.println("Usage: wordcount <in> <out> [-skip skipPatternFile]");
                 System.exit(2);
             }
             Job job = Job.getInstance(conf, "word count");
             job.setJarByClass(WordCount2.class);
             job.setMapperClass(TokenizerMapper.class);
             job.setCombinerClass(IntSumReducer.class);
             job.setReducerClass(IntSumReducer.class);
             job.setOutputKeyClass(Text.class);
             job.setOutputValueClass(IntWritable.class);
             List<String> otherArgs = new ArrayList<String>();
             for (int i=0; i < remainingArgs.length; ++i) {
                 if ("-skip".equals(remainingArgs[i])) {
                     job.addCacheFile(new Path(EMapReduceOSSUtil.buildOSSCompleteUri(remainingArgs[++i], conf)).toUri());
                     job.getConfiguration().setBoolean("wordcount.skip.patterns", true);
                 } else {
                     otherArgs.add(remainingArgs[i]);
                 }
             }
             FileInputFormat.addInputPath(job, new Path(EMapReduceOSSUtil.buildOSSCompleteUri(otherArgs.get(0), conf)));
             FileOutputFormat.setOutputPath(job, new Path(EMapReduceOSSUtil.buildOSSCompleteUri(otherArgs.get(1), conf)));
             System.exit(job.waitForCompletion(true) ? 0 : 1);
         }
     }
    ```

    See the following sample code for the EMapReduceOSSUtil class, which is located in the same directory of WordCount2:

    ```
    package com.aliyun.emr.hadoop.examples;
     import org.apache.hadoop.conf.Configuration;
     public class EMapReduceOSSUtil {
         private static String SCHEMA = "oss://";
         private static String AKSEP = ":";
         private static String BKTSEP = "@";
         private static String EPSEP = ".";
         private static String HTTP_HEADER = "http://";
         /**
          * complete OSS uri
          * convert uri like: oss://bucket/path  to  oss://accessKeyId:accessKeySecret@bucket.endpoint/path
          * ossref do not need this
          *
          * @param oriUri original OSS uri
          */
         public static String buildOSSCompleteUri(String oriUri, String akId, String akSecret, String endpoint) {
             if (akId == null) {
                 System.err.println("miss accessKeyId");
                 return oriUri;
             }
             if (akSecret == null) {
                 System.err.println("miss accessKeySecret");
                 return oriUri;
             }
             if (endpoint == null) {
                 System.err.println("miss endpoint");
                 return oriUri;
             }
             int index = oriUri.indexOf(SCHEMA);
             if (index == -1 || index ! = 0) {
                 return oriUri;
             }
             int bucketIndex = index + SCHEMA.length();
             int pathIndex = oriUri.indexOf("/", bucketIndex);
             String bucket = null;
             if (pathIndex == -1) {
                 bucket = oriUri.substring(bucketIndex);
             } else {
                 bucket = oriUri.substring(bucketIndex, pathIndex);
             }
             StringBuilder retUri = new StringBuilder();
             retUri.append(SCHEMA)
                     .append(akId)
                     .append(AKSEP)
                     .append(akSecret)
                     .append(BKTSEP)
                     .append(bucket)
                     .append(EPSEP)
                     .append(stripHttp(endpoint));
             if (pathIndex > 0) {
                 retUri.append(oriUri.substring(pathIndex));
             }
             return retUri.toString();
         }
         public static String buildOSSCompleteUri(String oriUri, Configuration conf) {
             return buildOSSCompleteUri(oriUri, conf.get("fs.oss.accessKeyId"), conf.get("fs.oss.accessKeySecret"), conf.get("fs.oss.endpoint"));
         }
         private static String stripHttp(String endpoint) {
             if (endpoint.startsWith(HTTP_HEADER)) {
                 return endpoint.substring(HTTP_HEADER.length());
             }
             return endpoint;
         }
     }
    ```

5.  Compile, package, and upload the code

    In the project directory, run the following commands:

    ```
    mvn clean package -DskipTests
    ```

    You can see the wordcountv2-1.0-SNAPSHOT.jar file in the target directory of your project directory, which is the JAR package of the job. Upload the JAR package to OSS.

6.  Create a job

    Create a new job in E-MapReduce with the following parameters:

    ```
    jar ossref://yourBucket/yourPath/wordcountv2-1.0-SNAPSHOT.jar com.aliyun.emr.hadoop.examples.WordCount2 -Dwordcount.case.sensitive=true oss://yourBucket/yourPath/The_Sorrows_of_Young_Werther.txt oss://yourBucket/yourPath/output -skip oss://yourBucket/yourPath/patterns.txt
    ```

    Here, yourBucket represents your OSS bucket, and yourPath represents a path in the bucket. Configure the settings as needed. You need to download the files for processing related resources, which are oss://yourBucket/yourPath/The\_Sorrows\_of\_Young\_Werther.txt and oss://yourBucket/yourPath/patterns.txt, and then store them in OSS. You can download the resources needed for the jobs and store these resources in the corresponding directories in OSS.

    Download: [The\_Sorrows\_of\_Young\_Werther.txt](https://docs-aliyun.cn-hangzhou.oss.aliyun-inc.com/cn/emr/1.3.7/assets/res/The_Sorrows_of_Young_Werther.txt?spm=a2c4g.11186623.2.19.4c3d19d6YlAWbs&file=The_Sorrows_of_Young_Werther.txt)[patterns.txt](https://docs-aliyun.cn-hangzhou.oss.aliyun-inc.com/cn/emr/1.3.7/assets/res/patterns.txt?spm=a2c4g.11186623.2.20.4c3d19d6YlAWbs&file=patterns.txt)

7.  Create and run the execution plan

    Create the execution plan in E-MapReduce, associate it with the job, and then run the execution plan.


