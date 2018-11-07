# Prerequisites {#concept_cms_hxn_y2b .concept}

Before creating E-MapReduce, you need to complete the following prerequisites:

1.  Apply for an Alibaba Cloud account.

    Before applying for the E-MapReduce cluster, you need to have an Alibaba Cloud account to identify yourself through the entire Alibaba Cloud ecosystem. This account can be used not only for E-MapReduce clusters, but also to activate Alibaba Cloud services, such as [Object Storage Service \(OSS\)](http://www.alibabacloud.com/product/oss), [ApsaraDB for RDS \(RDS\)](http://www.alibabacloud.com/product/rds).

    If you do not have any Alibaba Cloud account, apply for it by referring to [Register Cloud Account](https://www.alibabacloud.com/help/doc-detail/50482.htm).

2.  Create an AccessKey \(optional\)

    You must create at least one AccessKey according to the following steps:

    1.  Log on to the [official Alibaba Cloud website](https://www.alibabacloud.com/).

    2.  Log on to the [Alibaba Cloud console](https://account.alibabacloud.com/login/login.htm).
    3.  Click **AccessKeys**.

        **Note:** If a security prompt dialog box appears, click **Continue to manage AccessKey**.

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17837/154157626910452_en-US.png)

    4.  Click **Create AccessKey**.
    5.  AccessKey is created successfully.
3.  Activate Alibaba Cloud OSS.

    E-MapReduce will store your job logs and run logs in the Alibaba Cloud OSS storage space, so you need to [Sign up for OSS](https://www.alibabacloud.com/help/doc-detail/31884.htm). And create a bucket in the same area where you expect to create the cluster, see [Create a bucket](https://www.alibabacloud.com/help/doc-detail/31885.htm).

4.  Enable high-end models \(optional\)

    If you want to use models with 8 cores or more in clusters charged by the Pay-As-You-Go billing method, apply for opening it in ECS first. [Apply for high-end models](https://workorder.console.aliyun.com/console.htm).


