# ZooKeeper {#concept_cgk_bbx_y2b .concept}

The [ZooKeeper](https://zookeeper.apache.org/) service is enabled in E-MapReduce clusters by default.

**Note:** ZooKeeper only has 3 nodes, regardless of how many machines are currently in the cluster. More nodes are not currently supported.

## Create a cluster {#section_oyv_cbx_y2b .section}

When you create a cluster, select the Zookeeper service in the software configuration page.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17895/155255155810761_en-US.png)

## Node information {#section_vs2_mbx_y2b .section}

After you have created a cluster and its status is idle, in the **Clusters and Services** page, select ZooKeeper, and then click **Component Topology** to view ZooKeeper nodes. E-MapReduce enables 3 ZooKeeper nodes. The corresponding intranet IP address \(2181 is the default port\) of ZooKeeper nodes are indicated in the IP column for access to the ZooKeeper service.

