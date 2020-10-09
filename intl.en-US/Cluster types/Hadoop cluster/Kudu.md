# Kudu

This topic describes how to use Kudu in E-MapReduce \(EMR\).

## Typical scenarios

-   Real-time computing
-   Time series data
-   Prediction modeling
-   Tremendous baseline data

    In most cases, a large volume of baseline data exists in the production environment. The baseline data may be stored in HDFS, relational database management system \(RDBMS\), or Kudu. You can use Impala to access or query the baseline data. You do not need to migrate the data to Kudu.


## Architecture

Kudu consists of the following components:

-   Master servers: manage metadata.

    The metadata includes both the server and tablet information of tablet servers. Master servers work in high availability \(HA\) mode based on the Raft protocol.

-   Tablet servers: store tablets.

    Each tablet has multiple replicas to ensure high availability based on the Raft protocol.


The following figure shows the architecture of a Kudu cluster.

![base](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/2749841061/p63501.png)

## Create a Kudu cluster

Kudu is supported in EMR V3.22.0 and later. When you create a cluster, set Cluster Type to Hadoop and select Kudu from the optional services in the Software Settings step. This way, a Kudu cluster is created. By default, a Kudu cluster has three master nodes that work in HA mode.

## Integrate Impala with Kudu

You can set `kudu_master_hosts` to \[master1\]\[:port\],\[master2\]\[:port\],\[master3\]\[:port\] in the Impala configuration file or use the `kudu.master_addresses` parameter in the SQL statement to specify a Kudu cluster.

The following example shows how to use the `kudu.master_addresses` parameter in the SQL statement to specify a Kudu cluster:

```
CREATE TABLE my_first_table
    (
      id BIGINT,
      name STRING,
      PRIMARY KEY(id)
    )
    PARTITION BY HASH PARTITIONS 16
    STORED AS KUDU
    TBLPROPERTIES(
      'kudu.master_addresses' = 'master1:7051,master2:7051,master3:7051');
```

## Common commands

-   Check the status of master nodes.

    ```
    kudu cluster ksck <master_addresses>
    ```

    `<master_addresses>` specifies the internal IP addresses of master nodes.

-   Check the metrics of the Kudu cluster.

    ```
    kudu-tserver --dump_metrics_json
    kudu-master --dump_metrics_json
    ```

-   Query all tables and tablets.

    ```
    kudu table list <master_addresses>
    ```

-   Dump the content of a column file \(CFile\).

    ```
    kudu fs dump cfile <block_id>
    ```

-   Dump the directory structure of a Kudu file system.

    ```
    kudu fs dump tree [-fs_wal_dir=<dir>] [-fs_data_dirs=<dirs>]
    ```


