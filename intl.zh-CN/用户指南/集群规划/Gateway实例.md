# Gateway实例 {#concept_c3c_rm3_y2b .concept}

Gateway一般为独立的一个集群，由多台相同配置的节点组成。

在创建Gateway集群时，可以关联到一个已经存在的Hadoop集群上，该集群上会部署Hadoop（HDFS+YARN）、Hive、Spark、Sqoop、Pig等客户端，方便对集群进行操作。这样做的好处是：它可以作为一个独立的提交点，不会占用集群的资源，尤其是在Master提交的方式，可以提高Master节点的稳定性。如果作业太多，可以动态的增加节点。

您可以创建多个不同的Gateway集群，来给不同的用户使用，让他们可以使用各自独有的环境配置来满足不同的业务需求。

