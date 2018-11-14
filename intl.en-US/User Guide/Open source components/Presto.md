# Presto {#concept_rkf_f1x_y2b .concept}

Presto Instructions

E-MapReduce versions 2.0 and higher support [Presto](https://prestodb.io/). Presto can be used in E-MapReduce by checking presto software when selecting the mirror image.

After cluster creation, log on to the master node. Presto software will be installed in the directory /usr/lib/presto-current, and PrestoServer process can be seen by command jps.

Presto service process can be divided into coordinator and worker. Coordinator is started on the master \(the HA cluster is the master node of hostname starting with emr-header-1\), and worker process is started on the core node. The service process configuration is under the directory /usr/lib/presto-current/etc, and coordinator uses coordinator-config.properties while worker uses worker-config.preperties, and other configuration files are used for public. The web port is set as 9090.

By default, presto service is set with support from Hive. Connect the metastore of Hive on the cluster to read the table information of Hive and perform querying. The cluster is pre-installed with presto cli and can directly execute the following command to check the Hive table:

```
presto -server localhost:9090 -catalog hive -schema default -user hadoop -execute 'show tables'
```

**Note:** There is a delay of several seconds when Hive tables are synchronized.

Note that

