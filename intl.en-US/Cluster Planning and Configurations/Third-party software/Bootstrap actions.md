# Bootstrap actions {#concept_q52_vln_y2b .concept}

Bootstrap actions are used to run a customized script before the cluster starts Hadoop. The customized script is used to install your required third-party software or change the cluster operating environment.

## Function {#section_hw3_dt5_y2b .section}

With bootstrap actions, you can install and perform a variety of things to your cluster that are not currently supported by clusters, such as:

-   Install software with Yum.
-   Directly download open software from a public network.
-   Read your data from OSS.
-   Install and operate a service, such as Flink or Impala. \(The script for this is more complex.\)

We strongly recommend that you test the bootstrap action with a Pay-As-You-Go cluster and create a Subscription cluster only after the test is successful.

## Procedure {#section_kw3_dt5_y2b .section}

1.  Log on to the [Alibaba Cloud E-MapReduce console](https://partners-intl.console.aliyun.com/#/emr).
2.  Select a region. The cluster associated with this region is listed.
3.  Click **Create Cluster** to enter the cluster creation page.
4.  At the bottom of the basic configuration page, click **Add** to enter the operation page.
5.  Enter the configuration items.

You can add up to 16 bootstrap actions to be performed during cluster initialization in a specified sequence. By default, your customized script is run with the root account. Use su hadoop in the script to switch to a Hadoop account.

It is possible for a bootstrap action to fail, but this does not affect the creation of a cluster. After a cluster is created successfully, you can view all abnormalities in the bootstrap/software configuration column in the cluster details page. In the event of an abnormality, you can log on to all nodes to view the operation logs in the /var/log/bootstrap-actions directory.

## Bootstrap action type {#section_nw3_dt5_y2b .section}

Bootstrap actions are categorized into customized bootstrap actions and operating-condition bootstrap action. The main difference is that the operating-condition bootstrap action can only operate your operation in nodes that meet the requirements.

## Customized bootstrap action {#section_ow3_dt5_y2b .section}

For customized bootstrap actions, the position of the bootstrap action name and the execution script in OSS must be specified and, where necessary, optional parameters filled in. During cluster initialization, all nodes download the specified OSS scripts and runs them directly or after adding the optional parameters.

You can specify the files that need to be downloaded from OSS in the script. In the following example, the oss://yourbucket/myfile.tar.gz file is downloaded locally and extracted to the /yourdir directory:

```
#! #!/bin/bash
osscmd --id=<yourid> --key=<yourkey> --host=oss-cn-hangzhou-internal.aliyuncs.com get oss://<yourbucket>/<myfile>.tar.gz ./<myfile>.tar.gz
mkdir -p /<yourdir>
tar -zxvf <myfile>.tar.gz -C /<yourdir>
```

The osscmd is preinstalled on the node and can be invoked directly to download the file.

**Note:** OSS address host has an intranet address, an Internet address, and a VPC network address. If you use a classic network, you need to specify the intranet address. For Hangzhou, this is oss-cn-hangzhou-internal.aliyuncs.com. If you use a VPC network, you need to specify the domain name that the VPC intranet can access. For Hangzhou, this is vpc100-oss-cn-hangzhou.aliyuncs.com.

Bootstrap actions can install additional system software packages through Yum. The following example shows the installation of ld-linux.so. 2:

```
#! #!/bin/bash
yum install -y ld-linux.so. 2
```

## Operating-condition bootstrap action {#section_wdb_4t5_y2b .section}

The execution script of an operating-condition bootstrap action is predefined. This means that you only need to specify its name and optional parameters. Operating-condition bootstrap actions must provide optional parameters, including operating conditions and commands \(separated by spaces\). The operating conditions support instance.isMaster=true/false, which specifies that it only operates on master or non-master nodes. The following example shows that the optional parameters of an operating-condition bootstrap action are specified to only create a directory on a master node:

```
instance.isMaster=true mkdir -p /tmp/abc
```

If you need to specify multiple operating commands, you can divide them by using a semicolon \(;\). For example: `instance.isMaster=true mkdir -p /tmp/abc;mkdir -p /tmp/def`.

