# 简单操作 OSS 文件 {#concept_hdg_h1h_hfb .concept}

本文介绍使用 OSS SDK 存在的问题以及推荐做法。

## 使用 OSS SDK 存在的问题 {#section_tmc_31h_hfb .section}

若在 Spark 或者 Hadoop 作业中无法直接使用 OSS SDK 来操作 OSS 中的文件，是因为OSS SDK 中依赖的 http-client-4.4.x 版本与 Spark 或者 Hadoop 运行环境中的 http-client 存在版本冲突。如果要这么做，就必须先解决这个依赖冲突问题。实际上在 E-MapReduce 中，Spark 和 Hadoop 已经对 OSS 做了无缝兼容，可以像使用 HDFS 一样来操作 OSS 文件。

-   当前E-MapReduce环境支持MetaService服务，可以支持在E-MapReduce环境免AccessKey访问OSS数据。旧的显式输入AccessKey的方式依旧支持，请注意在操作OSS的时候优先使用内网的Endpoint。
-   当您需要在本地进行测试的时候，才要用到OSS的外网的Endpoint，这样才能从本地访问到OSS的数据。

所有的Endpint可以参考[OSS Endpoint](../../../../../intl.zh-CN/开发指南/访问域名（Endpoint）/访问域名和数据中心.md#)。

## 推荐做法（以免AccessKey方式为例） {#section_ugz_j1h_hfb .section}

请您使用如下方法来查询 OSS 目录下的文件：

```
[Scala] 
   import org.apache.hadoop.conf.Configuration
   import org.apache.hadoop.fs.{Path, FileSystem}
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

