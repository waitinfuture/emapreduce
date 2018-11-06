# ZooKeeper {#concept_cgk_bbx_y2b .concept}

The [ZooKeeper](https://zookeeper.apache.org/) service is currently enabled in E-MapReduce clusters by default.

**Note:** ZooKeeper will have only 3 nodes no matter how many machines are currently in the cluster. More nodes are not supported currently.

## Create a cluster {#section_oyv_cbx_y2b .section}

Select the Zookeeper service in the software configuration page when you create a cluster.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17895/154148583210761_en-US.png)

## Node information {#section_vs2_mbx_y2b .section}

After the cluster is successfully created and the status is idle, in the **Clusters and Services** page, select ZooKeeper, and then click **Component Topology** to view ZooKeeper nodes. E-MapReduce enables 3 ZooKeeper nodes. Corresponding intranet IP address \(2181 for port by default\) of ZooKeeper nodes are indicated in the IP column for the access to the ZooKeeper service.

