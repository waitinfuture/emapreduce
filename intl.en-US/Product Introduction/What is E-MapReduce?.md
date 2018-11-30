# What is E-MapReduce? {#concept_hcj_lgy_w2b .concept}

Alibaba Cloud Elastic MapReduce \(or E-MapReduce\) is a big data processing solution that facilitates the processing and analysis of massive amounts of data. Built on Alibaba Cloud Elastic Compute Service \(ECS\) and based on open-source Apache Hadoop and Apache Spark, E-MapReduce flexibly manages your data in a wide range of scenarios, such as trend analysis, data warehousing, and online and offline data processing. It also makes it easy for you to import and export data to and from other cloud storage systems and database systems, such as Alibaba Cloud OSS and Alibaba Cloud RDS.

## Using E-MapReduce {#section_tqz_mhy_w2b .section}

If you are using a distributed processing system such as Hadoop or Spark, follow these steps:

1.  Evaluate the business characteristics.
2.  Select a machine type.
3.  Purchase a machine.
4.  Prepare the hardware environment.
5.  Install an operating system.
6.  Deploy applications \(such as Hadoop and Spark\).
7.  Start a cluster.
8.  Write applications.
9.  Run a job.
10. Obtain data or perform another operation.

Steps 1-7 are preliminary tasks and may take some time to complete. Steps 8-10, however, concern application logic. E-MapReduce provides an integrated set of cluster management tools, including those used to build, configure, run, and manage clusters, configure and run jobs, as well as select hosts, deploy environments, and monitor performance.

With E-MapReduce, processes such as procurement, preparation, operation, and maintenance are all managed, allowing you to focus on the processing logic of your applications. E-MapReduce also provides flexible combination modes, allowing you to select different cluster services according to your needs. For example, if you want to receive daily statistics or perform simple batch operations, you can choose to only run Hadoop services in E-MapReduce. If you then want to implement stream-oriented and real-time computing at a later stage, you can add in Spark.

## Structure of E-MapReduce {#section_xgt_23y_w2b .section}

Clusters are the core component of E-MapReduce. An E-MapReduce cluster is essentially a Spark or Hadoop cluster that consists of multiple Alibaba Cloud ECS instances. For example, in Hadoop, the daemons that typically run on each ECS instance \(such as namenode, datanode, resourcemanager, and nodemanager\) form a Hadoop cluster. The nodes that run namenode and resourcemanager are known as master nodes, while those that run datanode and nodemanager are called slave nodes.

The following figure shows an E-MapReduce cluster that consists of one master node and three slave nodes:

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17824/15435696809988_en-US.jpg)

