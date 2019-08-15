# The FAQ about product use {#concept_cwk_bkk_pgb .concept}

## Q: How do I create a Pay-As-You- Go high-specification instance for an EMR cluster? {#section_svy_rlk_pgb .section}

A Pay-As-You-Go instance that has more than eight vCPUs \(high-specification\) is not shown in the EMR console. You need to submit a ticket to apply for a high-specification instance in the ECS console. We recommend that you use a Subscription cluster, which saves your time for applying for high-specification instances.

## Q: What is a high-security cluster? {#section_nrv_5mk_pgb .section}

A high-security cluster is a Kerberos-authenticated cluster. You can turn on the High Security Mode switch to create a high-security cluster on the Create Cluster page. See [Introduction to Kerberos](../../../../reseller.en-US/Open Source Components /Kerberos authentication/Introduction to Kerberos.md#) for more information. You cannot turn off the High Security Mode switch for a cluster of a version earlier than V3.12. If you want to use a non-high security cluster, create a new cluster. V3.12 and later versions support turning off the High Security Mode.

## Q: How do I create a cluster of an instance type from d1, big data type family? {#section_wy1_zjp_pgb .section}

By default, you cannot create a cluster of an instance type from d1, big data type family in the EMR console. D1 instance types are not listed in the EMR console. You need to submit a ticket to request d1 instances. Certain ECS configurations are required. This configuration process may take a while.

## Q: Why do I fail to create a cluster? {#section_sms_skp_pgb .section}

A: This issue occurs mainly because the maximum number of Pay-As-You-Go instances that you can create is exceeded. The quota of Pay-As-You-Go instances depends on the initial purchase of a user. You can submit a ticket to apply for a higher quota of Pay-As-You-Go instances. Another cause is that you do not have permission to create instances of the type you want. You need to enable the corresponding permission for the instance type in the ECS console.

## Q: How do I renew a cluster? {#section_xmw_knp_pgb .section}

For more information, see [Cluster renewal](../../../../reseller.en-US/Cluster Planning and Configurations/Configure clusters/Cluster renewal.md#). Many users fail to renew the clusters after renewing the subscription for ECS. This issue occurs because you have not renewed the subscription for EMR, which is also required for renewing a cluster. You can view the expiration dates of ECS and EMR on the cluster renewal page.

## Q: How do I adopt automatic renewal? {#section_lb2_gpp_pgb .section}

You can enable the Automatic Renewal feature in the EMR console to renew the subscription for EMR and ECS automatically.

## Q: Can I add an existing ECS instance to an EMR cluster? {#section_cds_rpp_pgb .section}

Currently, you can only create an ECS instance in the EMR console and then add it to an EMR cluster.

## Q: Can I install software on the master node of an EMR cluster? {#section_hpf_5pp_pgb .section}

A: Technically, you are allowed to install software on the master node as long as the cluster environment is not affected. However, we recommend that you do not perform this operation because the running of software may impact the stability of a cluster.

## Q: How do I connect to a core node and gain root privileges? {#section_yz5_zpp_pgb .section}

For more information, see the Connecting to a core node section in the [Create a cluster](../../../../reseller.en-US/Cluster Planning and Configurations/Configure clusters/Create a cluster.md#).

## Q: Can I release the insecure EIP of a header node in the ECS console? Does the operation affect EMR services? {#section_ak5_3qp_pgb .section}

A: You need to use an EIP to connect to the uniform meta database. If you have not enabled the uniform meta database feature, it is safe to release the EIP.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/122115/156583971938270_en-US.png)

## Q: Do services start automatically when nodes are powered on? Do services restart automatically after stopping unexpectedly? {#section_btx_krp_pgb .section}

A: Services start and restart automatically. A service retries starting three times after it fails to start.

## Q: How do I access open source components? {#section_b5z_prp_pgb .section}

## Q: What is required for a RAM user to operate EMR? {#section_hc3_wrp_pgb .section}

A: The corresponding Alibaba Cloud account needs to enable the EMRdefaultRole and ECSdefaultRole policies first. The RAM user needs to adopt the EMRfullaccessrole policy.

## Q: How to use Hue? {#section_w4f_ssp_pgb .section}

A: For the initial username and password of Hue, see [How to use Hue](../../../../reseller.en-US/Open Source Components /Hue.md#). You can also refer to this document when you forget your username or password. If Hue fails to access HDFS, set the value of the dfs.webhdfs.enabled property to true in the EMR console and restart HDFS.

## Q: Anonymous users are allowed to access Zeppelin by default . How do I disable this feature? Do I need to modify the configuration file on the master node if I cannot find the configuration option I want on the configuration page of Zeppelin in the EMR console? {#section_dws_k5p_pgb .section}

A: You need to modify the configurations manually and restart Zeppelin. For more information, see XXX. [http://blog.csdn.net/mergerly/article/details/53196918](http://blog.csdn.net/mergerly/article/details/53196918)

## Q: Does EMR support spot instances? {#section_syl_ryp_pgb .section}

A: Currently, spot instances are not supported.

## Q: Why cannot I run Hive queries with the uniform meta database feature enabled? The error message is as follows. "FAILED: SemanticException org.apache.hadoop.hive.ql.metadata.HiveException: java.lang.RuntimeException: Unable to instantiate org.apache.hadoop.hive.ql.metadata.SessionHiveMetaStoreClient" {#section_v55_qpq_pgb .section}

A: This issue occurs mainly because you do not have an available EIP to assign to your cluster. EIPs are required for connecting to uniform metadatabases. You need to assign an EIP to your cluster manually and submit a ticket to the Alibaba Cloud product team for adding the EIP to the security group of your databases.

## Q: What is a low-specification node? {#section_u3m_ctq_pgb .section}

A: A low-specification node is configured with two vCPUs and 8 GB of memory. You can create a low-specification node for some users in your whitelist to use. Note: Issues are prone to occurring. You need to resolve issues by yourself.

## Q: Why there is no instance type available for the master node? {#section_msz_jtq_pgb .section}

A: No instance type is available for the master node in your zone. You can switch to another zone.

## Q: How can I make other ECS instances submit jobs and return results? {#section_edf_4tq_pgb .section}

A: You can use Gateway. See [Gateway instance](../../../../reseller.en-US/Cluster Planning and Configurations/Cluster planning/Gateway clusters.md#) for purchasing Gateway in the EMR console.

## Q: How do I expand the disk capacity of an EMR cluster? {#section_bwc_f5q_pgb .section}

A: For more information, see [Disk capacity expansion](../../../../reseller.en-US/Cluster Planning and Configurations/Modify configurations/Disk expansion.md#). Expanding the disk capacity in the EMR console will be supported soon.

## Q: Can I enable logging in OSS for an existing cluster? {#section_yds_mwq_pgb .section}

A: Currently, it is not supported. We recommend that you create a workflow instance to run jobs. Workflow is a new feature that you can find on the Data Platform page. By using Workflow, you can view running logs without enabling the running logs feature .

## Q: Can a Zookeeper/Kafka/Storm cluster created using EMR communicate with HBase? {#section_dvt_lzq_pgb .section}

A: Yes. The EMR cluster and the HBase cluster need to be in the same VPC. Make sure to add the IP address of the EMR cluster to the whitelist of HBase.

## Q: How do I remove core nodes and task nodes from an EMR cluster? For example, I have a cluster of four core nodes and two task nodes. Is it feasible to make the cluster contain two core nodes? {#section_tgl_yzq_pgb .section}

A: Currently, removing core nodes in the console is not supported. If you want to scale in your cluster, submit a ticket to Alibaba Cloud Customer Services to request to remove core nodes. The sequence of removing nodes that run the ZooKeeper services is based on the worker IDs. The latest node is removed first. For example, the sequence of removing worker1, worker 2, worker3, and worker4 is worker4 \> worker3 \> worker2 \> worker1. Currently, removing Subscription core nodes and tasks nodes in the console is not supported. Removing Pay-As-You-Go task nodes in the console is supported.

## Q: How to get refunds for EMR? {#section_xbt_r1r_pgb .section}

A: Submit a ticket including the reason for refunds to the EMR product team.

## Q: What is the difference between EMR and MaxCompute? {#section_asl_n2r_pgb .section}

Both EMR and MaxCompute are used for big data processing. EMR is a big data platform built completely based on open source technologies. It is 100% compatible with open sources for use and practices. MaxCompute is a proprietary platform developed by Alibaba Cloud. It is easy to use with encapsulation and saves costs for operation and maintenance. EMR is designed based on the open-source Hadoop ecosystem. It is easy to get started for developers with Hadoop prior knowledge. Using MaxCompute requires a little modification on the code.

## Q: What is the password for MySQL in an EMR cluster? {#section_mx1_dfr_pgb .section}

A: On the Clusters and Services page, click Hive and click **Configuration** to view the password.

``` {#codeblock_wth_8w4_6n3}
javax.jdo.option.ConnectionURL
```

``` {#codeblock_au5_uu2_akt}
javax.jdo.option.ConnectionUserName
```

``` {#codeblock_umh_om2_gj6}
javax.jdo.option.ConnectionPassword
```

## Q: How do I resolve the issue that Zeppelin 0.71 does not support Spark 2.2? {#section_gsv_lfr_pgb .section}

A: This issue occurs in earlier versions of EMR. EMR V3.11 and later versions have resolved this issue by upgrading Zeppelin to V0.73.

## Q: Is automatic storage balancing available? Do I manually rebalance storage? {#section_nfh_ngr_pgb .section}

A: For manual storage balancing, choose **console** \> **Cluster Management** \> **Clusters and Services** \> **HDFS**. Click **Actions** and click **rebalance**.

## Q: Does EMR support downgrading configurations? For example, reducing 16 vCPUs and 32 GB of memory to 8 vCPUS and 16 GB of memory for the master node, core nodes, and tasks nodes. {#section_rdy_zgr_pgb .section}

A: Currently, it is not supported.

## Q: Why does the " The specified DataDisk Size beyond the permitted range, or the capacity of snapshot exceeds the size limit of the specified disk category" error message appear? {#section_wyv_2hr_pgb .section}

A: The value you set for the DataDisk Size parameter is too small for a ???

## Q: Why does the "User real name authenticate failed!" message appear ? {#section_vzq_nhr_pgb .section}

A: Your Alibaba Cloud account has not been real-name authenticated. Complete the authentication in the Alibaba Cloud console. EMR also requires real-name authentication of users. You can contact the EMR product team to complete authentication.

## Q: Why does the "Your account does not have enough balance" error message appear? {#section_lj1_whr_pgb .section}

A: Your account balance is insufficient.

## Q: Why does the "The maximum number of Pay-As-You-Go instances is exceeded: create ecs vcpu quota per region limited by user quota \[xxx\]." error message appear? {#section_vv4_b3r_pgb .section}

A: Your quota for Pay-As-You-Go instances has been exceeded. You need to apply for a higher quota in the ECS console or release instances to create an EMR cluster.

## Q: I cannot find Flume in the EMR console. How do I steam data to the OSS path configured for my Hadoop cluster using Flume? {#section_hwx_m3r_pgb .section}

A: Currently, Flume has not been integrated with EMR. You need to install Flume manually.

## Q: Does Spark support submitting jobs using the standalone mode? {#section_qwx_53r_pgb .section}

A: Currently, the Spark On Yarn mode is used by default in the EMR console. The standalone mode is not supported.

## Q: How do I modify software configurations? {#section_ukm_x3r_pgb .section}

A: Earlier versions of EMR do not support modifying software configurations in the console. You can perform the following steps to modify software configurations.

1.  Log on to the master node of your cluster.
2.  Go to the directory of configuration templates.

    ``` {#codeblock_eys_dtp_b9y}
    cd /var/lib/ecm-agent/cache/ecm/service/
    ```

3.  Locate the directory of the service you want. Assuming the service is Hue, go to the directory of Hue.
4.  Go to the corresponding directory to the version of Hue. For example, /var/lib/ecm-agent/cache/ecm/service/HUE/4.1.0.1.3.
5.  Corresponding configuration files are shown in the /package/templates/ directory.
6.  Modify the configurations as needed. You can add a configuration or modify an existing configuration.
    -   If you want to add a configuration, make sure the format is correct. Note: Check whether the line breaks and spaces are used properly.
7.  After the modification is complete, restart the service for the configurations to take effect.

