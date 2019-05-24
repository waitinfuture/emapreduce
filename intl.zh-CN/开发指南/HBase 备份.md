# HBase 备份 {#concept_hf3_zd4_hfb .concept}

E-MapReduce 的 HBase 集群，可以通过 HBase 内置的快照（Snapshot）功能对 HBase 的表进行备份，并将备份的数据导出到阿里云存储服务 [OSS](https://oss.console.aliyun.com/)中

示例如下：

1.  创建 HBase 集群

    详见 [集群创建文档](../../../../intl.zh-CN/集群规划与配置/集群配置/创建集群.md#)

2.  创建 Table

    ```
    >create 'test','cf'
    ```

3.  添加数据

    ```
    > put 'test','a','cf:c1',1
    > put 'test','a','cf:c2',2
    > put 'test','b','cf:c1',3
    > put 'test','b','cf:c2',4
    > put 'test','c','cf:c1',5
    > put 'test','c','cf:c2',6
    ```

4.  创建快照

    ```
    hbase snapshot create -n test_snapshot -t test
    ```

    查看快照

    ```
    >list_snapshots
    SNAPSHOT                TABLE + CREATION TIME
     test_snapshot          test (Sun Sep 04 20:31:00 +0800 2016)
    1 row(s) in 0.2080 seconds
    ```

5.  导出到 OSS

    ```
    hbase org.apache.hadoop.hbase.snapshot.ExportSnapshot -snapshot test_snapshot -copy-to oss://$accessKeyId:$accessKeySecret@$bucket.oss-cn-hangzhou-internal.aliyuncs.com/hbase/snapshot/test
    ```

    **说明：** OSS 使用[内网Endpoint](../../../../intl.zh-CN/开发指南/访问域名（Endpoint）/OSS访问域名使用规则.md#)

6.  创建另一个 HBase 集群
7.  OSS 导出快照

    ```
    hbase org.apache.hadoop.hbase.snapshot.ExportSnapshot -snapshot test_snapshot -copy-from oss://$accessKeyId:$accessKeySecret@$bucket.oss-cn-hangzhou-internal.aliyuncs.com/hbase/snapshot/test -copy-to /hbase/
    ```

8.  从快照恢复数据

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

9.  从快照创建新表

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


