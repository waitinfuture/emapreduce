# FAQ about cluster planning and configuration

This topic provides answers to some commonly asked questions about cluster planning and configuration.

-   [How do I fix the error "The specified zone is not available for purchase"?](#section_l7k_9zt_oon)
-   [How do I fix the error "The request processing has failed due to some unknown error, exception or failure"?](#section_hhi_myj_nvp)
-   [How do I fix the error "The Node Controller is temporarily unavailable"?](#section_ol8_59i_huw)
-   [How do I fix the error "No quota or zone is available"?](#section_ia4_0ll_1jw)
-   [How do I fix the error "The specified InstanceType is not authorized for use"?](#section_0wy_5ml_68l)
-   [How do I fix the error "The specified DataDisk Size beyond the permitted range, or the capacity of snapshot exceeds the size limit of the specified disk category"?](#section_zoj_lze_q9a)
-   [How do I fix the error "Your account does not have enough balance"?](#section_tgf_i33_lv4)
-   [How do I fix the error "The maximum number of Pay-As-You-Go instances is exceeded: create ecs vcpu quota per region limited by user quota \[xxx\]"?](#section_boq_zld_ifm)
-   [How do I fix the error "FAILED: SemanticException org.apache.hadoop.hive.ql.metadata.HiveException: java.lang.RuntimeException: Unable to instantiate org.apache.hadoop.hive.ql.metadata.SessionHiveMetaStoreClient"?](#section_bqm_8zo_5uj)
-   [How do I fix the error "The specified instance Type exceeds the maximum limit for the PostPaid instances"?](#section_foj_m9e_zbb)
-   [Do I need to handle a cluster creation failure?](#section_p5a_njm_evx)
-   [How do I log on to a core node?](#section_xsi_03w_1v5)
-   [Why do I still receive renewal notifications after I have renewed my cluster?](#section_8r5_jtn_qm1)
-   [Do EMR clusters support auto-renewal?](#section_n83_d54_u3e)
-   [How do I apply for a refund for an EMR cluster?](#section_14u_sfe_e57)
-   [What is the division of work in an EMR cluster?](#section_slz_k24_dn5)
-   [How do I handle disk exceptions in a Kafka cluster?](#section_9nh_khp_7qh)
-   [Can I install additional software on the master node of an EMR cluster?](#section_hmr_4or_hqp)
-   [Do services on each node automatically start when I power on the server? Do services on each node automatically restart after they are stopped unexpectedly?](#section_9e4_8hu_0bs)
-   [What is the port number of HBase Thrift servers?](#section_g0t_etl_zse)
-   [Does EMR support preemptible instances?](#section_3wl_acl_jgs)
-   [Why can I not set EMR Version to EMR-3.4.3 when I create a cluster?](#section_wah_sxw_h7e)
-   [What are the differences between EMR and MaxCompute?](#section_kfj_ydt_5fo)
-   [Does EMR support automatic storage balancing? How do I manually rebalance storage?](#section_368_yez_z0h)
-   [How do I apply for an instance type with advanced configurations?](#section_rl3_noh_rb2)

## How do I fix the error "The specified zone is not available for purchase"?

Pay-as-you-go ECS instances are unavailable in the zone you selected. We recommend that you select a different zone.

## How do I fix the error "The request processing has failed due to some unknown error, exception or failure"?

Try again later. If the error persists, [submit a ticket](https://workorder-intl.console.aliyun.com/#/ticket/createIndex). You can also submit a ticket right after the error is reported.

## How do I fix the error "The Node Controller is temporarily unavailable"?

Wait a moment and create the cluster again.

## How do I fix the error "No quota or zone is available"?

ECS instances in the zone you selected are insufficient. Create the cluster again, but select a different zone or use the default zone during the creation.

## How do I fix the error "The specified InstanceType is not authorized for use"?

Submit applications to use pay-as-you-go instances with advanced configurations \(each instance with more than eight vCPUs\). Click [submit a ticket](https://workorder-intl.console.aliyun.com/#/ticket/createIndex) to submit applications. The supported specifications include eight vCPUs and 16 GiB of memory, eight vCPUs and 32 GiB of memory, 16 vCPUs and 32 GiB of memory, and 16 vCPUs and 64 GiB of memory.

## How do I fix the error "The specified instance Type exceeds the maximum limit for the PostPaid instances"?

Possible causes:

-   Your quota for pay-as-you-go instances has been reached.
-   You do not have the permissions to create the current type of instance.

Solutions:

-   Submit an application to increase your quota.
-   Go to the ECS console and grant your account the permissions to create the current type of instance.

## How do I log on to a core node?

1.  On the master node, run the following command to switch to the hadoop user:

    ```
    su hadoop
    ```

2.  Log on to the core node in password-free mode.

    ```
    ssh emr-worker-1
    ```

3.  Run the following sudo command to obtain the root permissions:

    ```
    sudo vi /etc/hosts
    ```


## Why do I still receive renewal notifications after I have renewed my cluster?

Cause: You are charged for both EMR resources and ECS instances. You probably renewed only ECS instances.

Solution: Go to the **Cluster Overview** page for your EMR cluster. Select **Renewal** from the **Renewal** drop-down list in the upper-right corner. On the page that appears, check the expiration time of the ECS instances and EMR resources.

## Do EMR clusters support auto-renewal?

Yes, both EMR resources and ECS instances can be automatically renewed.

## How do I apply for a refund for an EMR cluster?

[submit a ticket](https://workorder-intl.console.aliyun.com/#/ticket/createIndex) to apply for a refund.

## Do I need to handle a cluster creation failure?

You do not need to handle cluster creation failures. If you fail to create a cluster, no computing resources are created. The cluster record is automatically removed from the cluster list in the EMR console after three days.

## What is the division of work in an EMR cluster?

A standard EMR cluster consists of a single master node and multiple core nodes. Only core nodes store data and implement data computing. For example, a cluster consists of three instances. Each instance has four vCPUs and 8 GiB of memory. One instance serves as the master node and the other two serve as core nodes. The available computing resources of this cluster are two instances, each with four vCPUs and 8 GiB of memory.

## How do I handle disk exceptions in a Kafka cluster?

Cause: The disk is full or damaged.

Solution:

-   If your disk is full, perform the following operations:
    1.  Log on to the server.
    2.  Find the fully occupied disk and delete unnecessary data to free up some of the disk space. Take note of the following rules:
        -   Do not delete Kafka data directories. Otherwise, you may lose all of your data.
        -   Do not delete Kafka topics, such as consumer\_offsets and schema.
        -   Find the topics that occupy a large space or that you no longer need. Delete historical log segments and the index and timeindex files of the segments from some partitions of the topics.
    3.  Restart the Kafka broker.
-   If your disk is damaged, perform the following operations:
    -   If no more than 25% of disks are damaged on a machine, you do not need to take an action.
    -   If more than 25% of disks are damaged on a machine, [submit a ticket](https://workorder-intl.console.aliyun.com/#/ticket/createIndex).

## How do I fix the error "The specified DataDisk Size beyond the permitted range, or the capacity of snapshot exceeds the size limit of the specified disk category"?

The disk size you specified is too small. Set the disk size to 40 GB or larger.

## How do I fix the error "Your account does not have enough balance"?

The account balance is insufficient. Top up your account and try again.

## How do I fix the error "The maximum number of Pay-As-You-Go instances is exceeded: create ecs vcpu quota per region limited by user quota \[xxx\]"?

Your quota for pay-as-you-go ECS instances has been reached. You can release some existing pay-as-you-go ECS instances or submit an application to increase your quota.

## Can I install additional software on the master node of an EMR cluster?

We recommend that you do not install additional software. The installation may affect the stability and reliability of the cluster.

## Do services on each node automatically start when I power on the server? Do services on each node automatically restart after they are stopped unexpectedly?

Yes, services can automatically start and restart.

## What is the port number of HBase Thrift servers?

Port 9099.

## Does EMR support preemptible instances?

If you enable the auto scaling feature for a cluster, you can use preemptible instances. For more information, see [Create a preemptible instance](/intl.en-US/Cluster Management/Configure clusters/Auto Scaling/Create a preemptible instance.md).

## How do I fix the error "`FAILED: SemanticException org.apache.hadoop.hive.ql.metadata.HiveException: java.lang.RuntimeException: Unable to instantiate org.apache.hadoop.hive.ql.metadata.SessionHiveMetaStoreClient`"?

During cluster creation, **Unified Metabases** is selected for Type. An error is reported during the execution of a Hive job.

The cluster fails to be created because the cluster does not have an elastic IP address. [submit a ticket](https://workorder-intl.console.aliyun.com/#/ticket/createIndex).

## Why can I not set EMR Version to EMR-3.4.3 when I create a cluster?

EMR is updated periodically. Some earlier versions are deprecated. Check whether the versions of components such as Hive and Spark in the available EMR versions meet your requirements. If you need to use a deprecated EMR version, [submit a ticket](https://workorder-intl.console.aliyun.com/#/ticket/createIndex).

## What are the differences between EMR and MaxCompute?

Both of them are big data processing solutions. EMR is a big data platform that is built completely based on open source technologies. It is fully compatible with open source software. MaxCompute is a platform developed by Alibaba Cloud and is not open source. It offers easy-to-use features based on the encapsulation and low costs of operations and maintenance.

## Does EMR support automatic storage balancing? How do I manually rebalance storage?

Automatic storage balancing is not supported. You can perform the following steps to manually rebalance the storage of a cluster:

1.  Log on to the [Alibaba Cloud E-MapReduce console](https://emr.console.aliyun.com/).
2.  In the top navigation bar, select the region where your cluster resides. Select the resource group as required. By default, all resources of the account appear.
3.  Click the **Cluster Management** tab.
4.  Find the cluster whose storage you want to rebalance, and click **Details** in the Actions column.
5.  In the left-side navigation pane, click **Cluster Service** and then **HDFS**.
6.  In the upper-right corner of the page that appears, select **Rebalance** from the **Actions** drop-down list.
7.  In the **Cluster Activities** dialog box, specify the related parameters and click **OK**.
8.  In the **Confirm** message, click **OK**.

## How do I apply for an instance type with advanced configurations?

[submit a ticket](https://workorder-intl.console.aliyun.com/#/ticket/createIndex).

