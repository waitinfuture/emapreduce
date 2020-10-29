# Use Jindo DistCp

This topic describes how to use the data copy tool Jindo DistCp.

-   Java Development Kit \(JDK\) 8 is installed on your computer.
-   An EMR cluster of the 3.28.0 version or later is created. For information about how to create a cluster, see [Create a cluster](/intl.en-US/Cluster Management/Configure clusters/Create a cluster.md).

## Use Jindo DistCp

Run the following command to obtain the help information:

```
[root@emr-header-1 opt]# jindo distcp --help
```

The following information is returned:

```
     --help   -   Print help text
     --src=VALUE   -   Directory to copy files from
     --dest=VALUE   -   Directory to copy files to
     --parallelism=VALUE   -   Copy task parallelism
     --outputManifest=VALUE   -   The name of the manifest file
     --previousManifest=VALUE   -   The path to an existing manifest file
     --requirePreviousManifest=VALUE   -   Require that a previous manifest is present if specified
     --copyFromManifest   -   Copy from a manifest instead of listing a directory
     --srcPrefixesFile=VALUE   -   File containing a list of source URI prefixes
     --srcPattern=VALUE   -   Include only source files matching this pattern
     --deleteOnSuccess   -   Delete input files after a successful copy
     --outputCodec=VALUE   -   Compression codec for output files
     --groupBy=VALUE   -   Pattern to group input files by
     --targetSize=VALUE   -   Target size for output files
     --enableBalancePlan   -   Enable plan copy task to make balance
     --enableDynamicPlan   -   Enable plan copy task dynamically
     --enableTransaction   -   Enable transation on Job explicitly
     --diff   -   show the difference between src and dest filelist
     --ossKey=VALUE   -   Specify your oss key if needed
     --ossSecret=VALUE   -   Specify your oss secret if needed
     --ossEndPoint=VALUE   -   Specify your oss endPoint if needed
     --policy=VALUE   -   Specify your oss storage policy
     --cleanUpPending   -   clean up the incomplete upload when distcp job finish
     --queue=VALUE   -   Specify yarn queuename if needed
     --bandwidth=VALUE   -   Specify bandwidth per map/reduce in MB if needed
     --s3Key=VALUE   -   Specify your s3 key
     --s3Secret=VALUE   -   Specify your s3 Sercet
     --s3EndPoint=VALUE   -   Specify your s3 EndPoint
```

## --src and --dest

`--src` specifies the source directory. `--dest` specifies the destination directory.

By default, Jindo DistCp copies all files in the directory specified by `--src` to the directory specified by `--dest`. If you do not specify a root directory, Jindo DistCp automatically creates a root directory.

For example, you can run the following command to copy files in the /opt/tmp directory of HDFS to an OSS bucket:

```
jindo distcp --src /opt/tmp --dest oss://yang-hhht/tmp
```

## --parallelism

`--parallelism` specifies the mapreduce.job.reduces parameter for the MapReduce job that is executed to copy files. The default value is 7. You can customize `--parallelism` based on your available cluster resources. This allows you to determine how many reduce tasks can be run in parallel.

For example, you can run the following command to copy files in the /opt/tmp directory of HDFS to an OSS bucket:

```
jindo distcp --src /opt/tmp --dest oss://yang-hhht/tmp --parallelism 20
```

## --srcPattern

`--srcPattern` specifies a regular expression that filters files for the copy operation. The regular expression must match a full path.

For example, if you need to copy all log files in the /data/incoming/hourly\_table/2017-02-01/03 directory, set `--srcPattern` to .\*\\.log.

Run the following command to view the files in the /data/incoming/hourly\_table/2017-02-01/03 directory.

```
[root@emr-header-1 opt]# hdfs dfs -ls /data/incoming/hourly_table/2017-02-01/03
```

The following information is returned:

```
Found 6 items
-rw-r-----   2 root hadoop       2252 2020-04-17 20:42 /data/incoming/hourly_table/2017-02-01/03/000151.sst
-rw-r-----   2 root hadoop       4891 2020-04-17 20:47 /data/incoming/hourly_table/2017-02-01/03/1.log
-rw-r-----   2 root hadoop       4891 2020-04-17 20:47 /data/incoming/hourly_table/2017-02-01/03/2.log
-rw-r-----   2 root hadoop       4891 2020-04-17 20:42 /data/incoming/hourly_table/2017-02-01/03/OPTIONS-000109
-rw-r-----   2 root hadoop       1016 2020-04-17 20:47 /data/incoming/hourly_table/2017-02-01/03/emp01.txt
-rw-r-----   2 root hadoop       1016 2020-04-17 20:47 /data/incoming/hourly_table/2017-02-01/03/emp06.txt
```

Run the following command to copy the log files:

```
jindo distcp --src /data/incoming/hourly_table --dest oss://yang-hhht/hourly_table --srcPattern . *\.log --parallelism 20
```

Run the following command to view files in the destination OSS bucket:

```
[root@emr-header-1 opt]# hdfs dfs -ls oss://yang-hhht/hourly_table/2017-02-01/03
```

The following information is returned. Only log files in the source directory are copied.

```
Found 2 items
-rw-rw-rw-   1       4891 2020-04-17 20:52 oss://yang-hhht/hourly_table/2017-02-01/03/1.log
-rw-rw-rw-   1       4891 2020-04-17 20:52 oss://yang-hhht/hourly_table/2017-02-01/03/2.log
```

## --deleteOnSuccess

`--deleteOnSuccess` enables Jindo DistCp to delete the copied files from the source directory after a copy operation succeeds.

For example, you can run the following command to copy files in /data/incoming/hourly\_table to an OSS bucket and delete the copied files from the source directory:

```
jindo distcp --src /data/incoming/hourly_table --dest oss://yang-hhht/hourly_table --deleteOnSuccess --parallelism 20
```

## --outputCodec

`--outputCodec` specifies the compression codec that is used to compress copied files online. Example:

```
jindo distcp --src /data/incoming/hourly_table --dest oss://yang-hhht/hourly_table --outputCodec=gz --parallelism 20
```

Run the following command to view files in the destination directory:

```
[root@emr-header-1 opt]# hdfs dfs -ls oss://yang-hhht/hourly_table/2017-02-01/03
```

The following information is returned. Files in the destination directory are compressed in the GZ format.

```
Found 6 items
-rw-rw-rw-   1        938 2020-04-17 20:58 oss://yang-hhht/hourly_table/2017-02-01/03/000151.sst.gz
-rw-rw-rw-   1       1956 2020-04-17 20:58 oss://yang-hhht/hourly_table/2017-02-01/03/1.log.gz
-rw-rw-rw-   1       1956 2020-04-17 20:58 oss://yang-hhht/hourly_table/2017-02-01/03/2.log.gz
-rw-rw-rw-   1       1956 2020-04-17 20:58 oss://yang-hhht/hourly_table/2017-02-01/03/OPTIONS-000109.gz
-rw-rw-rw-   1        506 2020-04-17 20:58 oss://yang-hhht/hourly_table/2017-02-01/03/emp01.txt.gz
-rw-rw-rw-   1        506 2020-04-17 20:58 oss://yang-hhht/hourly_table/2017-02-01/03/emp06.txt.gz
```

You can set this parameter to gzip, gz, lzo, lzop, snappy, none, or keep. Default value: keep.

-   none: Jindo DistCp does not compress copied files. If the files have been compressed, Jindo DistCp decompresses them.
-   keep: Jindo DistCp copies files with no change in the compression.

**Note:** If you want to use the LZO codec in an open source Hadoop cluster, you must install the native library of gplcompression and the hadoop-lzo package.

## --outputManifest and --requirePreviousManifest

`--outputManifest` generates a manifest file that contains the information about all the files copied by Jindo DistCp. The information includes the destination files, source files, and file sizes.

If you want to generate a manifest file, set `--requirePreviousManifest` to `false`. By default, the file is compressed in the GZ format. This is the only supported format.

```
jindo distcp --src /data/incoming/hourly_table --dest oss://yang-hhht/hourly_table --outputManifest=manifest-2020-04-17.gz --requirePreviousManifest=false --parallelism 20
```

Run the following command to view the content of the file:

```
[root@emr-header-1 opt]# hadoop fs -text oss://yang-hhht/hourly_table/manifest-2020-04-17.gz > before.lst
[root@emr-header-1 opt]# cat before.lst 
```

The following information is returned:

```
{"path":"oss://yang-hhht/hourly_table/2017-02-01/03/000151.sst","baseName":"2017-02-01/03/000151.sst","srcDir":"oss://yang-hhht/hourly_table","size":2252}
{"path":"oss://yang-hhht/hourly_table/2017-02-01/03/1.log","baseName":"2017-02-01/03/1.log","srcDir":"oss://yang-hhht/hourly_table","size":4891}
{"path":"oss://yang-hhht/hourly_table/2017-02-01/03/2.log","baseName":"2017-02-01/03/2.log","srcDir":"oss://yang-hhht/hourly_table","size":4891}
{"path":"oss://yang-hhht/hourly_table/2017-02-01/03/OPTIONS-000109","baseName":"2017-02-01/03/OPTIONS-000109","srcDir":"oss://yang-hhht/hourly_table","size":4891}
{"path":"oss://yang-hhht/hourly_table/2017-02-01/03/emp01.txt","baseName":"2017-02-01/03/emp01.txt","srcDir":"oss://yang-hhht/hourly_table","size":1016}
{"path":"oss://yang-hhht/hourly_table/2017-02-01/03/emp06.txt","baseName":"2017-02-01/03/emp06.txt","srcDir":"oss://yang-hhht/hourly_table","size":1016}
```

## --outputManifest and --previousManifest

`--outputManifest` generates a manifest file that contains a list of both previously and newly copied files. `--previousManifest` generates a manifest file that contains a list of previously copied files. This way, you can recreate the full history of operations and see what files are copied by the current job:

For example, two files are added to the source directory. Run the following command to copy the newly added files:

```
jindo distcp --src /data/incoming/hourly_table --dest oss://yang-hhht/hourly_table --outputManifest=manifest-2020-04-18.gz --previousManifest=oss://yang-hhht/hourly_table/manifest-2020-04-17.gz --parallelism 20
```

Run the following command to view the copied files:

```
[root@emr-header-1 opt]# hadoop fs -text oss://yang-hhht/hourly_table/manifest-2020-04-18.gz > current.lst
[root@emr-header-1 opt]# diff before.lst current.lst 
```

The following information is returned:

```
3a4,5
> {"path":"oss://yang-hhht/hourly_table/2017-02-01/03/5.log","baseName":"2017-02-01/03/5.log","srcDir":"oss://yang-hhht/hourly_table","size":4891}
> {"path":"oss://yang-hhht/hourly_table/2017-02-01/03/6.log","baseName":"2017-02-01/03/6.log","srcDir":"oss://yang-hhht/hourly_table","size":4891}
```

## --copyFromManifest

You can use `--copyFromManifest` to specify a manifest file that was previously generated by `--outputManifest` and copy the files listed in the manifest file to the destination directory. Example:

```
jindo distcp --src /data/incoming/hourly_table --dest oss://yang-hhht/hourly_table --previousManifest=oss://yang-hhht/hourly_table/manifest-2020-04-17.gz --copyFromManifest --parallelism 20
```

## --srcPrefixesFile

`--srcPrefixesFile` enables Jindo DistCp to copy files in multiple folders at a time.

Run the following command to view the files under hourly\_table:

```
[root@emr-header-1 opt]# hdfs dfs -ls oss://yang-hhht/hourly_table
```

The following information is returned:

```
Found 4 items
drwxrwxrwx   -          0 1970-01-01 08:00 oss://yang-hhht/hourly_table/2017-02-01
drwxrwxrwx   -          0 1970-01-01 08:00 oss://yang-hhht/hourly_table/2017-02-02
```

Run the following command to copy the files under hourly\_table to the folders.txt file:

```
jindo distcp --src /data/incoming/hourly_table --dest oss://yang-hhht/hourly_table --srcPrefixesFile file:///opt/folders.txt --parallelism 20
```

Run the following command to view the content of the folders.txt file:

```
[root@emr-header-1 opt]# cat folders.txt 
```

The following information is returned:

```
hdfs://emr-header-1.cluster-50466:9000/data/incoming/hourly_table/2017-02-01
hdfs://emr-header-1.cluster-50466:9000/data/incoming/hourly_table/2017-02-02
```

## --groupBy and -targetSize

Reading a large number of small files from HDFS affects the data processing performance. Therefore, we recommend that you use Jindo DistCp to merge small files into large files of a specified size. This optimizes analysis performance and reduces costs.

Run the following command to view the files in the specified folder:

```
[root@emr-header-1 opt]# hdfs dfs -ls /data/incoming/hourly_table/2017-02-01/03
```

The following information is returned:

```
Found 8 items
-rw-r-----   2 root hadoop       2252 2020-04-17 20:42 /data/incoming/hourly_table/2017-02-01/03/000151.sst
-rw-r-----   2 root hadoop       4891 2020-04-17 20:47 /data/incoming/hourly_table/2017-02-01/03/1.log
-rw-r-----   2 root hadoop       4891 2020-04-17 20:47 /data/incoming/hourly_table/2017-02-01/03/2.log
-rw-r-----   2 root hadoop       4891 2020-04-17 21:08 /data/incoming/hourly_table/2017-02-01/03/5.log
-rw-r-----   2 root hadoop       4891 2020-04-17 21:08 /data/incoming/hourly_table/2017-02-01/03/6.log
-rw-r-----   2 root hadoop       4891 2020-04-17 20:42 /data/incoming/hourly_table/2017-02-01/03/OPTIONS-000109
-rw-r-----   2 root hadoop       1016 2020-04-17 20:47 /data/incoming/hourly_table/2017-02-01/03/emp01.txt
-rw-r-----   2 root hadoop       1016 2020-04-17 20:47 /data/incoming/hourly_table/2017-02-01/03/emp06.txt
```

Run the following command to merge the TXT files in the folder into files each with a size of no more than 10 MB:

```
jindo distcp --src /data/incoming/hourly_table --dest oss://yang-hhht/hourly_table --targetSize=10 --groupBy='.*/([a-z]+). *.txt' --parallelism 20
```

Run the following command to view the files in the destination directory. The two TXT files are merged into one.

```
[root@emr-header-1 opt]# hdfs dfs -ls oss://yang-hhht/hourly_table/2017-02-01/03/
Found 1 items
-rw-rw-rw-   1       2032 2020-04-17 21:18 oss://yang-hhht/hourly_table/2017-02-01/03/emp2
```

## --enableBalancePlan

If both small and large files are to be copied but the file sizes are not significantly different among small files and among large files, you can use `--enableBalancePlan` to optimize the job allocation plan. This improves the copy performance of Jindo DistCp. If you do not specify an application plan, files are randomly allocated.

```
jindo distcp --src /data/incoming/hourly_table --dest oss://yang-hhht/hourly_table --enableBalancePlan --parallelism 20
```

**Note:** You cannot use this option in the same command as `--groupby` or `--targetSize`.

## --enableDynamicPlan

If the files to be copied are significantly different in size and most of the files are small files, you can use `--enableDynamicPlan` to optimize the job allocation plan. This improves the copy performance of Jindo DistCp.

```
jindo distcp --src /data/incoming/hourly_table --dest oss://yang-hhht/hourly_table --enableDynamicPlan --parallelism 20
```

**Note:** You cannot use this option in the same command as `--groupby` or `--targetSize`.

## --enableTransaction

`--enableTransaction` ensures the integrity of job levels and transaction support among jobs. Example:

```
jindo distcp --src /data/incoming/hourly_table --dest oss://yang-hhht/hourly_table --enableTransaction --parallelism 20
```

## --diff

After files are copied, you can use `--diff` to check the differences between the source and destination directories.

Example:

```
jindo distcp --src /data/incoming/hourly_table --dest oss://yang-hhht/hourly_table --diff
```

If all files are copied, the following information is returned:

```
INFO distcp.JindoDistCp: distcp has been done completely
```

If some files are not copied, a manifest file that contains a list of these files is generated in the destination directory. Then, you can use `--copyFromManifest` and `--previousManifest` to copy the files in the list to the destination directory. This way, the data volume and file quantity are verified. If Jindo DistCp has performed compression or decompression operations during the copy process, `--diff` does not return accurate file size differences.

```
jindo distcp --src /data/incoming/hourly_table --dest oss://yang-hhht/hourly_table --dest oss://yang-hhht/hourly_table --previousManifest=file:///opt/manifest-2020-04-17.gz --copyFromManifest --parallelism 20
```

**Note:** If your destination directory is an HDFS directory, you must specify `--dest` in the format of /path, hdfs://hostname:ip/path, or hdfs://headerIp:ip/path. Other formats, such as hdfs:///path and hdfs:/path, are not supported.

## --queue

--queue specifies the name of the YARN queue where the current DistCp task resides.

Example:

```
jindo distcp --src /data/incoming/hourly_table --dest oss://<your_bucket>/hourly_table --queue yarnqueue
```

## --bandwidth

--bandwidth specifies the bandwidth allocated to the current DistCp task. This prevents the task from using excessive bandwidth. Unit: MB/s.

## Use Amazon S3 as a data source

You can use the --s3Key, --s3Secret, and --s3EndPoint parameters in a command to specify information related to Amazon S3.

Example:

```
jindo distcp jindo-distcp-2.7.3.jar --src s3a://yourbucket/ --dest oss://<your_bucket>/hourly_table --s3Key yourkey --s3Secret yoursecret --s3EndPoint s3-us-west-1.amazonaws.com 
```

You can configure the s3Key, s3Secret, and s3EndPoint parameters in the core-site.xml file of Hadoop. This way, you do not need to specify an AccessKey pair each time.

```
<configuration>
    <property>
        <name>fs.s3a.access.key</name>
        <value>xxx</value>
    </property>

    <property>
        <name>fs.s3a.secret.key</name>
        <value>xxx</value>
    </property>

    <property>
        <name>fs.s3.endpoint</name>
        <value>s3-us-west-1.amazonaws.com</value>
    </property>
</configuration>
```

Example:

```
jindo distcp /tmp/jindo-distcp-2.7.3.jar --src s3://smartdata1/ --dest s3://smartdata1/tmp --s3EndPoint  s3-us-west-1.amazonaws.com
```

## Check DistCp Counters

Run the following commands to check DistCp Counters in the counter information of MapReduce:

```
Distcp Counters
        Bytes Destination Copied=11010048000
        Bytes Source Read=11010048000
        Files Copied=1001

Shuffle Errors
        BAD_ID=0
        CONNECTION=0
        IO_ERROR=0
        WRONG_LENGTH=0
        WRONG_MAP=0
        WRONG_REDUCE=0
```

**Note:** If Jindo DistCp has performed compression or decompression operations during the copy process, the values of `Bytes Destination Copied` and `Bytes Source Read` may be different.

