# Service list {#concept_w2h_5tn_y2b .concept}

The **Clusters and Services** tab has been added to the tab list of the cluster management page to show the running status of HDFS, YARN, and other services.

The service list shows the information as follows.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17858/154106093210445_en-US.jpg)

Running statuses of services are only showed for clusters that are in Idle or Running status. When creating a cluster, if the service you click is unchecked \(such as Storm\), it will not be listed.

Click a service to view the corresponding tabs including **Status**, **Component Topology**, **Configuration**, and **Configuration Change History**. The status is divided into **Normal**and **Error**. If the status of a service on a node is **Error**, you can use the master node to jump to the corresponding node, and check the service process.

