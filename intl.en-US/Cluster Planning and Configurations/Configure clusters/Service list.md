# Service list {#concept_w2h_5tn_y2b .concept}

HDFS, YARN, and other services are listed on the **Clusters and Services** tab of the cluster management page. You can view the running statuses of these services.

The service list shows the following information.

![Clusters and Services](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17858/155704098010445_en-US.jpg)

A service's running status is only showed for clusters that are in the Idle or Running status. Services that are not checked when creating a cluster, such as Storm, are not listed.

Click a service to view the corresponding tabs, including **Status**, **Component Topology**, **Configuration**, and **Configuration Change History**. The status can either be **Normal** or **Error**. If the status of a service on a node is **Error**, you can use the master node to log on to the node and check the service process.

