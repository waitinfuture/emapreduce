# ZooKeeper 使用说明 {#concept_cgk_bbx_y2b .concept}

目前 E-MapReduce 集群中默认启动了 [ZooKeeper](https://zookeeper.apache.org/) 服务。

**说明：** 目前无论集群内有多少台机器，ZooKeeper 只会有 3 个节点。目前还不支持更多的节点。

## 创建集群 {#section_oyv_cbx_y2b .section}

E-MapReduce 创建集群的软件配置页面，可选择ZooKeeper服务 。

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17895/154157912610761_zh-CN.png)

## 节点信息 {#section_vs2_mbx_y2b .section}

集群创建成功，状态空闲后，在集群与服务管理页面选择ZooKeeper，单击**部署拓扑**可以查看到 ZooKeeper 的节点信息，E-MapReduce 会启动 3 个 ZooKeeper 节点。IP一栏标有 ZooKeeper 节点对应的内网 IP \(端口默认为 2181\)，即可访问 ZooKeeper 服务。

