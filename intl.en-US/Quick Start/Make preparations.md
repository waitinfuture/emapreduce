# Make preparations

This topic describes the preparations you must make before you create an E-MapReduce \(EMR\) cluster.

## Register an Alibaba Cloud account

If you do not have an Alibaba Cloud account, register one and complete real-name verification.

## Assign default system roles to the EMR service account

After you activate the EMR service, you must assign default system roles **AliyunEMRDefaultRole** and **AliyunEmrEcsDefaultRole** to the EMR service account. For more information, see [Role authorization](/intl.en-US/Cluster Management/Cluster planning/Authorize roles.md).

## Grant permissions to RAM users

If you want to log on to the EMR console as a RAM user and use the features in the console, you must log on to the RAM console by using your Alibaba Cloud account and grant the required permissions to the RAM user. For more information, see [Grant permissions to a RAM user](/intl.en-US/Cluster Management/Cluster planning/Grant permissions to a RAM user.md).

## \(Optional\) Activate Alibaba Cloud OSS

To facilitate data storage and reduce storage costs, we recommend that you activate Alibaba Cloud OSS and store data such as job logs and operational logs in OSS buckets. For more information about how to activate Alibaba Cloud OSS, see [Activate OSS](/intl.en-US/Quick Start/Sign up for OSS.md). Create OSS buckets in the same region as the EMR cluster that you want to create. For more information, see [Create buckets](/intl.en-US/Quick Start/Create buckets.md).

## \(Optional\) Create an AccessKey pair

You must create at least one AccessKey pair to use EMR. Perform the following steps to create an AccessKey pair:

1.  Log on to the [Alibaba Cloud Management Console](https://home-intl.console.aliyun.com/) by using your Alibaba Cloud account.
2.  Move the pointer over your profile picture in the upper-right corner and click **AccessKey Management**.

    **Note:** If the following message appears, click **Use Current AccessKey Pair**.

    ![AccessKey](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/4076341061/p10452.png)

3.  On the **AccessKey Management** page, click **Create AccessKey**.
4.  An AccessKey pair is created.

## \(Optional\) Create instances with advanced configurations

If you want to create instances with eight or more vCPUs for a pay-as-you-go cluster, log on to your Alibaba Cloud account and submit a ticket. For more information about how to submit a ticket, see [submit a ticket](https://workorder-intl.console.aliyun.com/#/ticket/createIndex).

