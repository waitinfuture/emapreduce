# What is EMR {#concept_hcj_lgy_w2b .concept}

Alibaba Cloud Elastic MapReduce \(E-MapReduce\) is a system solution for big data processing that runs on the Alibaba Cloud platform. E-MapReduce is built on Alibaba Cloud Elastic Compute Service \(ECS\) and is based on open-source Apache Hadoop and Apache Spark. It facilitates the use of other peripheral systems \(for example, Apache Hive, Apache Pig, and HBase\) in the Hadoop and Spark ecosystems to analyze and process data. You can also easily import data to and export data from other cloud data storage systems and database systems, such as Alibaba Cloud OSS and Alibaba Cloud RDS.

## Use of E-MapReduce {#section_tqz_mhy_w2b .section}

In general, to use distributed processing systems, such as Hadoop and Spark, the following actions are recommended:

1.  Evaluate the business characteristics.
2.  Select the machine type.
3.  Purchase the machine.
4.  Prepare the hardware environment.
5.  Install the operating system.
6.  Deploy the applications \(such as Hadoop and Spark\).
7.  Start the cluster.
8.  Write the applications.
9.  Run a job.
10. Obtain the data and so on.

Steps 8-10 relate to the application logic of the user. Steps 1-7 are early preparations and tend to be difficult and cumbersome. E-MapReduce provides an integrated solution of cluster management tools, such as host selection, environment deployment, cluster building, cluster configuration, cluster running, job configuration, job running, cluster management, and performance monitoring.

With E-MapReduce, processes such as procurement, preparation, operation, and maintenance are managed, allowing you to focus on the processing logics of your applications. E-MapReduce also provides flexible combination modes, allowing you to select different cluster services according to your needs. For example, if you want to implement daily statistics and simple batch operations, you can choose to run only Hadoop services in E-MapReduce; if you still want to implement stream-oriented computation and real-time computation, you can add Spark services on the basis of Hadoop services.

## Composition of E-MapReduce {#section_xgt_23y_w2b .section}

The core component directly oriented to an E-MapReduce user is the cluster. An E-MapReduce cluster is a Spark and Hadoop cluster consisting of multiple ECS Alibaba Cloud instances. For example, in Hadoop, generally some daemon processes run on each ECS instance \(such as namenode, datanode, resourcemanager, and nodemanager\), which make up the Hadoop cluster. The nodes running namenode and resourcemanager are known as master nodes, while those running datanode and nodemanager are called slave nodes.

For example, the following figure shows an E-MapReduce cluster consisting of one master node and three slave nodes:

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17824/15404606909988_en-US.jpg)

