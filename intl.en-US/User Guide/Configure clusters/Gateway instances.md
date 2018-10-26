# Gateway instances {#concept_c3c_rm3_y2b .concept}

Gateway is an independent cluster consisting of multiple nodes with same configurations.

When creating a gateway cluster, you can associate an existing Hadoop cluster on which Hadoop \(HDFS+YARN\), Hive, Spark, Sqoop, Pig, and other clients have been deployed to facilitate cluster operations. It is an independent submission point and does not take up the resources of the cluster, especially you submit jobs on the Master node, which can improve the stability of the Master node. If you have too many jobs to submit, you can add nodes for the cluster dynamically.

You can also create multiple gateway clusters for different users, allowing them to use their own environment to meet different business needs.

