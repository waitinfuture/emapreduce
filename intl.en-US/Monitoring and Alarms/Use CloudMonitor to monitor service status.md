# Use CloudMonitor to monitor service status

CloudMonitor is a service that monitors Internet applications and Alibaba Cloud resources. CloudMonitor collects data of metrics for Alibaba Cloud resources, detects network availability, and allows you to set alerts for specific metrics. This topic describes how to use CloudMonitor to monitor the status of core services in an EMR cluster and send alert notifications. Notification methods include phone calls, text messages, emails, and DingTalk chatbot.

## View monitoring data

1.  Log on to the [CloudMonitor](https://cms-intl.console.aliyun.com) console.

2.  In the left-side navigation pane, click **Cloud Service Monitoring** and then **E-MapReduce**.

3.  On the Clusters tab of the E-MapReduce Monitoring List page, find the target cluster and click the **cluster ID** or **Monitoring Charts** in the **Actions** column.

4.  Click a **time range** above the monitoring charts or click the Custom icon to customize a time range.

    You can view the monitoring data from a maximum of seven consecutive days.


## Create an alert rule

Follow these steps to create an alert rule:

1.  Log on to the [CloudMonitor](https://cms-intl.console.aliyun.com) console.

2.  In the left-side navigation pane, click **Cloud Service Monitoring** and then **E-MapReduce**.

3.  On the Clusters tab of the E-MapReduce Monitoring List page, find the target cluster and click **Alarm Rules** in the Actions column.

4.  Click **Create Alarm Rule** in the upper-right corner or click **here** in the message that prompts you to create an alert rule.

    1.  In the **Related Resource** section, specify **Resource Range**.

        If you select **All Resources** for **Resource Range**, CloudMonitor sends an alert notification when any of the clusters under your account meets the alert rule. If you select **Cluster**, you need to specify a cluster. In this case, CloudMonitor sends an alert notification only when the specified cluster meets the alert rule. If you want to create an alert rule that applies to an application group, see [Application group](/intl.en-US/Quick Start/Application group.md).

    2.  In the **Set Alarm Rules** section, set an alert rule.

        Set the required parameters. For the metrics of core services, see [Metrics of core services](#section_lxm_1f8_w5f).

        For example, to trigger an alert when the HTTP port of DataNodes is unreachable for longer than five minutes, select the **DataNodeHttpPortOpen** metric and specify the other parameters, as shown in the following figure.

        ![role](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/7829792951/p93303.png)

        **Note:** If you select **All** from the role drop-down list, the alert rule applies to all nodes in the cluster, including nodes that are added to the cluster in the future.

    3.  In the **Notification Method** section, set **Notification Contact** and **Notification Methods**.

        Notification Contact must be set to one or more contact groups. You can click **Quickly create a contact group** to create a contact group.

        You can set Notification Methods to a combination of two or more of the following methods: phone calls, text messages, emails, and DingTalk chatbot. To use the phone call method, you must purchase a phone alert resource plan.

    4.  Click **Confirm**.


## Metrics of core services

|Service|Metric|Description|
|-------|------|-----------|
|HDFS|NameNodeHttpPortOpen|The status of HTTP port 50070 of NameNode.|
|DataNodeHttpPortOpen|The status of HTTP port 50075 of DataNodes.|
|DataNodeIpcPortOpen|The status of IPC port 50020 of DataNodes.|
|TotalDFSUsedPercent|The total HDFS capacity usage of the cluster.|
|MaxDFSUsedPercent|The maximum HDFS capacity usage of all DataNodes.|
|DataNodeDfsUsedPercent|The HDFS capacity usage of a single DataNode.|
|NumDeadDataNode|The number of dead DataNodes. **Note:** This metric is obtained by using Java Management Extensions \(JMX\) of NameNode. If the heartbeat processes of DataNodes or the standby NameNode are stopped, the monitoring on this metric is suspended. |
|YARN|ResourceManagerWebappPortOpen|The status of web port 8088 of ResourceManager.|
|NodeManagerHttpPortOpen|The status of HTTP port 8042 of NodeManager.|
|NumUnhealthyNMs|The number of unhealthy NodeManagers.|
|HIVE|HiveServer2PortOpen|The status of port 10000 of HiveServer.|
|MetastorePortOpen|The status of port 9083 of Hive MetaStore.|
|ZooKeeper|ZKClientPortOpen|The status of client port 2181 of ZooKeeper.|
|ZkOutstandingRequests|The number of queued requests. If the ZooKeeper server receives more requests than it can process, this metric goes up.|
|HBase|HMasterHttpPortOpen|The status of HTTP port 16010 of HMaster.|
|HMasterIpcPortOpen|The status of IPC port 16000 of HMaster.|
|HRegionServerHttpPortOpen|The status of HTTP port 16030 of HRegionServer.|
|HRegionServerIpcPortOpen|The status of IPC port 16020 of HRegionServer.|

**Note:** For all the port metrics in this table, 1 indicates that the port is open, and 0 indicates that the port is closed.

