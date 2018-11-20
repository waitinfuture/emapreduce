# Back up HBase {#concept_hf3_zd4_hfb .concept}

The HBase cluster created in E-MapReduce allows you to use the snapshot feature integrated into HBase to back up HBase tables, and export the backup to [OSS](https://oss.console.aliyun.com/).

Example:

1.  Create an HBase cluster

    For more information, see [Create clusters](../DNemapreduce1876943/EN-US_TP_17853.dita#concept_olg_vq3_y2b).

2.  Create a table

    ```
    >create 'test','cf'
    ```

3.  Add data

    ```
    > put 'test','a','cf:c1',1
    > put 'test','a','cf:c2',2
    > put 'test','b','cf:c1',3
    > put 'test','b','cf:c2',4
    > put 'test','c','cf:c1',5
    > put 'test','c','cf:c2',6
    ```

4.  Create a snapshot

    ```
    hbase snapshot create -n test_snapshot -t test
    ```

    List snapshots

    ```
    >list_snapshots
    SNAPSHOT                TABLE + CREATION TIME
     test_snapshot          test (Sun Sep 04 20:31:00 +0800 2016)
    1 row(s) in 0.2080 seconds
    ```

5.  Export the snapshot to OSS.

    ```
    hbase org.apache.hadoop.hbase.snapshot.ExportSnapshot -snapshot test_snapshot -copy-to oss://$accessKeyId:$accessKeySecret@$bucket.oss-cn-hangzhou-internal.aliyuncs.com/hbase/snapshot/test
    ```

    **Note:** Access OSS using [internal endpoints](../../SP_21/DNOSS11827291/EN-US_TP_4344.dita#concept_hh2_4tv_tdb).

6.  Create another HBase cluster
7.  Export snapshots from OSS

    ```
    hbase org.apache.hadoop.hbase.snapshot.ExportSnapshot -snapshot test_snapshot -copy-from oss://$accessKeyId:$accessKeySecret@$bucket.oss-cn-hangzhou-internal.aliyuncs.com/hbase/snapshot/test -copy-to /hbase/
    ```

8.  Restore data from snapshots

    ```
    >restore _snapshot 'test_snapshot'
    ```

    ```
    >scan 'test'
    ROW                     COLUMN+CELL
     a                      column=cf:c1, timestamp=1472992081375, value=1
     a                      column=cf:c2, timestamp=1472992090434, value=2
     b                      column=cf:c1, timestamp=1472992104339, value=3
     b                      column=cf:c2, timestamp=1472992099611, value=4
     c                      column=cf:c1, timestamp=1472992112657, value=5
     c                      column=cf:c2, timestamp=1472992118964, value=6
    3 row(s) in 0.0540 seconds
    ```

9.  Create new tables from snapshots

    ```
    >clone_snapshot 'test_snapshot','test_2'
    ```

    ```
    >scan 'test_2'
    ROW                     COLUMN+CELL
     a                      column=cf:c1, timestamp=1472992081375, value=1
     a                      column=cf:c2, timestamp=1472992090434, value=2
     b                      column=cf:c1, timestamp=1472992104339, value=3
     b                      column=cf:c2, timestamp=1472992099611, value=4
     c                      column=cf:c1, timestamp=1472992112657, value=5
     c                      column=cf:c2, timestamp=1472992118964, value=6
    3 row(s) in 0.0540 seconds
    ```


