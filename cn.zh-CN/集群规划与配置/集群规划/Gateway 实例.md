# Gateway 实例 {#concept_c3c_rm3_y2b .concept}

Gateway 一般为独立的一个集群，由多台相同配置的节点组成。

在创建 Gateway 集群时，可以关联到一个已经存在的 Hadoop 集群上，该集群上会部署 Hadoop（HDFS+YARN）、Hive、Spark、Sqoop、Pig 等客户端，方便对集群进行操作。这样做的好处是：Gateway 可以作为一个独立的提交点，不会占用 Hadoop 集群的资源，尤其是在 Master 提交的方式，可以提高 Master 节点的稳定性。

您可以创建多个不同的 Gateway 集群，来给不同的用户使用，让他们可以使用各自独有的环境配置来满足不同的业务需求。

