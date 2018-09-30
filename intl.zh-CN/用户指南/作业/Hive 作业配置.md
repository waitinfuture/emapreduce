# Hive 作业配置 {#concept_cf2_rfp_y2b .concept}

E-MapReduce 中，用户申请集群的时候，默认为用户提供了 Hive 环境，用户可以直接使用 Hive 来创建和操作自己的表和数据。

## 操作步骤 {#section_w5j_vfp_y2b .section}

1.  用户需要提前准备好 Hive SQL 的脚本，例如：

    ```
    USE DEFAULT;
     DROP TABLE uservisits;
     CREATE EXTERNAL TABLE IF NOT EXISTS uservisits  (sourceIP STRING,destURL STRING,visitDate STRING,adRevenue DOUBLE,user
     Agent STRING,countryCode STRING,languageCode STRING,searchWord STRING,duration INT ) ROW FORMAT DELIMITED FIELDS TERMI
     NATED BY ',' STORED AS SEQUENCEFILE LOCATION '/HiBench/Aggregation/Input/uservisits';
     DROP TABLE uservisits_aggre;
     CREATE EXTERNAL TABLE IF NOT EXISTS uservisits_aggre ( sourceIP STRING, sumAdRevenue DOUBLE) STORED AS SEQUENCEFILE LO
     CATION '/HiBench/Aggregation/Output/uservisits_aggre';
     INSERT OVERWRITE TABLE uservisits_aggre SELECT sourceIP, SUM(adRevenue) FROM uservisits GROUP BY sourceIP;
    ```

2.  将该脚本保存到一个脚本文件中，例如叫 uservisits\_aggre\_hdfs.hive，然后将该脚本上传到 OSS 的某个目录中（例如：oss://path/to/uservisits\_aggre\_hdfs.hive）。
3.  登录[阿里云 E-MapReduce 控制台作业列表](https://emr.console.aliyun.com/)。
4.  单击该页右上角的**创建作业**，进入创建作业页面。
5.  填写作业名称。
6.  选择 Hive 作业类型，表示创建的作业是一个 Hive 作业。这种类型的作业，其后台实际上是通过以下的方式提交。

    ```
    hive [user provided parameters]
    ```

7.  在**应用参数**选项框中填入 Hive 命令后续的参数。例如，如果需要使用刚刚上传到 OSS 的 Hive 脚本，则填写的内容如下：

    ```
    -f ossref://path/to/uservisits_aggre_hdfs.hive
    ```

    您也可以单击**选择 OSS 路径**，从 OSS 中进行浏览和选择，系统会自动补齐 OSS 上 Hive 脚本的绝对路径。请务必将 Hive 脚本的前缀修改为 ossref（单击**切换资源类型**），以保证 E-MapReduce 可以正确下载该文件。

8.  选择执行失败后策略。
9.  单击**确定**，Hive 作业即定义完成。

