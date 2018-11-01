# Cluster script {#concept_xv2_j5n_y2b .concept}

To modify the cluster running environment, you can install third-party software in a cluster, especially a subscription cluster. After a cluster is created, the cluster script feature allows you to select nodes in batches and run your specified script to fulfill individual requirements.

## The role of cluster scripts {#section_w1w_p5n_y2b .section}

A cluster script is similar to a bootstrap action. After creating a cluster, you can install software packages to your cluster, for example:

-   Use Yum to install the software that has been provided.
-   Directly download public software packages from the public network.
-   Read your data from OSS.
-   Install and run a service like Flink or Impala which requires a more complex script.

We strongly recommend that you test the cluster script on a node first. After the script is verified, you can perform operations on the whole cluster.

## How to create and run a cluster script {#section_y1w_p5n_y2b .section}

1.  A cluster script can run on an idle or running cluster. On the **Cluster Management**page, click **View Details** of the corresponding cluster.
2.  On the left-side menu, click **Cluster Scripts** to enter the cluster script execution interface.
3.  In the upper right corner, click **Create and Run** to enter the creation interface.
4.  Enter configurations in the script creation pane. Select a node for execution and click **OK**.

You can click **View Details** to display the running status of a script on each node or click **Refresh** to update the running status of a cluster on each node.

Cluster scripts can only run on available clusters that are idle or running. Cluster scripts are applicable for long-standing clusters. For on-demand clusters, perform a bootstrap action to initialize the clusters.

The cluster script feature downloads a script from the OSS and run it on the specified node. If the returned value is 0, the execution has failed. If the execution fails, you can log on to the node to check the running log. The running log for each node is located at /var/log/cluster-scripts/clusterScriptId. If the cluster is configured with an OSS log directory, the running log is also uploaded to osslogpath/clusterId/ip/cluster-scripts/clusterScriptId.

By default, the root account is used to run the specified script. In the script, you can use su hadoop to switch to a Hadoop account.

A cluster script can successfully run on some nodes, but fails on others. For example, the restart of a node can lead to a failure in script operation. After resolving the error, you can run the cluster script again. After a cluster is expanded, you can specify the expanded node for separate execution of the cluster script.

Only one cluster script can run on a cluster at a time. If a cluster script is running, you cannot submit a new cluster script for execution. For each cluster, you can retain up to ten cluster script records. Therefore, if you already have ten records, to create a new cluster script, you must delete the previous records first.

## Script example {#section_bbw_p5n_y2b .section}

For a script similar to a bootstrap action, you can specify the file in the script to be downloaded from OSS. In the following example, the file oss://yourbucket/myfile.tar.gz is downloaded and decompressed to the directory /yourdir:

```
#! #!/bin/bash
osscmd --id=<yourid> --key=<yourkey> --host=oss-cn-hangzhou-internal.aliyuncs.com get oss://<yourbucket>/<myfile>.tar.gz ./<myfile>.tar.gz
mkdir -p /<yourdir>
tar -zxvf <myfile>.tar.gz -C /<yourdir>
```

OSSCMD is pre-installed on the node and can be called directly to download the file.

**Note:** The OSS host address can be an intranet address, an Internet address, or a VPC network address. If a classic network is used, you must specify an intranet address. If the network is located in Hangzhou, the intranet address is oss-cn-hangzhou-internal.aliyuncs.com. If a VPC network is used, you must specify a domain name that can be accessed from the VPC intranet. If the network is located in Hangzhou, the domain name is vpc100-oss-cn-hangzhou.aliyuncs.com.

Additional system software packages can be installed to the script using Yum, for example, ld-linux.so. 2：

```
#! /bin/bash
yum install -y ld-linux.so. 2
```

