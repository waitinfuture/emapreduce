# Gateway 实例

本节介绍Gateway 集群可作为 Hadoop 等集群的一个独立的作业提交点，创建时必须关联到一个已经存在的集群，以便您更好的对关联集群进行操作。

Gateway 集群一般是一个独立的集群，由多台相同配置的节点组成，集群上会部署 Hadoop（HDFS+YARN）、Hive、Spark 和 Sqoop 等客户端。

未创建 Gateway 集群时，Hadoop 等集群的作业是在本集群的 Master 或 Core 节点上进行提交的，会占用本集群的资源。创建 Gateway 集群后，您可通过 Gateway 集群来提交其关联的集群的作业，这样既不会占用关联集群的资源，也可提高关联集群 Master 或 Core 节点的稳定性，尤其是 Master 节点。

每一个 Gateway 集群均支持独立的环境配置，例如，在多个部门共用一个集群的场景下，您可为这个集群创建多个 Gateway 集群，以满足不同部门的业务需求。

