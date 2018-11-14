# Operate OSS files {#concept_hdg_h1h_hfb .concept}

This topic describes the possible issues of using OSS SDK and the recommended practices.

## Issues in the use of OSS SDK {#section_tmc_31h_hfb .section}

If you cannot directly use OSS SDK to operate OSS files in Spark or Hadoop jobs, it is because conflicts exist between the http-client-4.4.x version on which the OSS SDK is dependent and the http-client version in the Spark or Hadoop running environments. If such operations are required, you must resolve the dependency conflicts first. In fact, Spark and Hadoop are already seamlessly compatible with OSS in E-MapReduce, and you can operate OSS files as you are using HDFS.

-   The current E-MapReduce environment supports MetaService, which allows you to access OSS data in the E-MapReduce environment without an AccessKey. The previous way of using an AccessKey is still supported. Internal network endpoints are preferred when using OSS.
-   When you test locally, you need to use a public network endpoint of OSS so that you can access OSS data from your local machine.

For more information about a complete endpoint list, see [OSS endpoints](../../SP_21/DNOSS11827291/EN-US_TP_4350.dita#concept_zt4_cvy_5db).

## Recommended practices \(without using an AccessKey\) {#section_ugz_j1h_hfb .section}

We recommend that you use the following methods to query files under OSS directories:

```
[Scala] 
   import org.apache.hadoop.conf.Configuration
   import org.apache.hadoop.fs.{ Path, FileSystem}
   val dir = "oss://bucket/dir"
   val path = new Path(dir)
   val conf = new Configuration()
   conf.set("fs.oss.impl", "com.aliyun.fs.oss.nat.NativeOssFileSystem")
   val fs = FileSystem.get(path.toUri, conf)
   val fileList = fs.listStatus(path)
   ...
   [Java]
   import org.apache.hadoop.conf.Configuration;
   import org.apache.hadoop.fs.Path;
   import org.apache.hadoop.fs.FileStatus;
   import org.apache.hadoop.fs.FileSystem;
   String dir = "oss://bucket/dir";
   Path path = new Path(dir);
   Configuration conf = new Configuration();
   conf.set("fs.oss.impl", "com.aliyun.fs.oss.nat.NativeOssFileSystem");
   FileSystem fs = FileSystem.get(path.toUri(), conf);
   FileStatus[] fileList = fs.listStatus(path);
   ...
```

