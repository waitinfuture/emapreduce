# Bootstrap action {#concept_q52_vln_y2b .concept}

Bootstrap action is used to run your customized script before the cluster starts up Hadoop. The customized script is used to install your required third-party software or change the cluster operating environment.

## Function of bootstrap operation {#section_hw3_dt5_y2b .section}

With bootstrap action, you can install many things to your cluster that are not currently supported by clusters. For example:

-   Install provided software with Yum.
-   Directly download open software from a public network.
-   Read your data from OSS.
-   Install and operate a service, such as Flink or Impala, but the script to be compiled is more complex.

We strongly recommend that you test the bootstrap action with a Pay-As-You-Go cluster and create a subscription cluster only after the test is successful.

## How to use {#section_kw3_dt5_y2b .section}

1.  Log on to [Alibaba Cloud E-MapReduce Console Cluster List](https://emr.console.aliyun.com/).
2.  Select the region where the created cluster associated with the region is listed.
3.  Click **Create cluster** to enter the cluster creation page.
4.  Click **Add bootstrap operation** to enter the operation page.
5.  Enter the configuration item and click Complete after adding.

You can add up to 16 bootstrap actions to be performed during cluster initialization in the designated sequence. By default, your designated script is run with the root account. You can switch to a Hadoop account with su hadoop in the script.

It is possible that the bootstrap action fails. For ease of use, bootstrap action failure does not affect the creation of the cluster. After the cluster is created successfully, you can view any abnormality in the Bootstrap/software configuration column of cluster information in the cluster details page. In case of any abnormality, you can log on to all nodes to view the operation logs in the directory of /var/log/bootstrap-actions.

## Bootstrap action type {#section_nw3_dt5_y2b .section}

The bootstrap action is categorized into customized bootstrap action and operating-condition bootstrap action. The main difference is that the operating-condition bootstrap action can only operate your designated operation in the node that meets the requirements.

## Customized bootstrap action {#section_ow3_dt5_y2b .section}

For the customized bootstrap action, the position of the bootstrap action name and the execution script in OSS must be designated and the optional parameters are set as required. During cluster initialization, all nodes download the designated OSS scripts to run them directly or after adding the optional parameters.

You can designate the files that need to be downloaded from OSS in the script. The following example downloads the file oss://yourbucket/myfile.tar.gz locally and extract it to the directory of /yourdir:

```
#! #!/bin/bash
osscmd --id=<yourid> --key=<yourkey> --host=oss-cn-hangzhou-internal.aliyuncs.com get oss://<yourbucket>/<myfile>.tar.gz ./<myfile>.tar.gz
mkdir -p /<yourdir>
tar -zxvf <myfile>.tar.gz -C /<yourdir>
```

The osscmd has been preinstalled on the node and can be invoked directly to download the file.

**Note:** OSS address host contains intranet address, Internet address, and VPC network address. For the classic network, the intranet address is designated. The address in Hangzhou is oss-cn-hangzhou-internal.aliyuncs.com. For VPC network, the domain name that VPC intranet can visit is designated. The name in Hangzhou is vpc100-oss-cn-hangzhou.aliyuncs.com.

The bootstrap action can install additional system software packages through Yum. The following example shows the installation of ld-linux.so. 2:

```
#! #!/bin/bash
yum install -y ld-linux.so. 2
```

## Operating-condition bootstrap action {#section_wdb_4t5_y2b .section}

The execution script of an operating-condition bootstrap action is predefined, you do not need to make additional designations. You do need to designate the name and optional parameters. The operating-condition bootstrap action must provide the optional parameters, including the spaced operation conditions and commands. The operation conditions support instance.isMaster=true/false and is designated to only operate on the master or non-master nodes. The following example shows that the optional parameters of an operating-condition bootstrap action are only designated to create the directory on the master node.

```
instance.isMaster=true mkdir -p /tmp/abc
```

If multiple operation commands are designated, you can divide several statements with the semicolon “;”. For example: `instance.isMaster=true mkdir -p /tmp/abc;mkdir -p /tmp/def`.

