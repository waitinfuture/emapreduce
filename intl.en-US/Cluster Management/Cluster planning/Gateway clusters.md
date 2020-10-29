# Gateway clusters

Gateway clusters can serve as a separate job submission node for Hadoop clusters. When you create a gateway cluster, you must associate it with an existing cluster. This facilitates operations on the associated cluster.

A gateway cluster is an independent cluster that consists of multiple instances with the same configurations. Clients, such as Hadoop \(HDFS+YARN\), Hive, Spark, and Sqoop clients, are deployed on the cluster.

If no gateway cluster is created, jobs of a Hadoop cluster are submitted on the master or core instance of the Hadoop cluster, which consumes the resources of this cluster. After a gateway cluster is created, you can use it to submit jobs of the cluster associated with this gateway cluster. This way, the jobs do not occupy the resources of the associated cluster, and the stability of the master and core instances, especially the master instance, in the associated cluster is improved.

Each gateway cluster can have an independent configuration environment. For example, you can create multiple gateway clusters for one cluster that is shared by multiple departments to meet their business requirements.

