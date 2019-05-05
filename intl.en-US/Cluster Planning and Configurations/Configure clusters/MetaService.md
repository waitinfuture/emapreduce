# MetaService {#concept_wj5_fpt_1fb .concept}

MetaService allows you to access Alibaba Cloud resources in the E-MapReduce cluster without using an AccessKey \(AK\).

## Default roles {#section_xwf_3pt_1fb .section}

When creating a cluster, you must authorize an application role \(AliyunEmrEcsDefaultRole\) to E-MapReduce. After you do so, you can perform operations on E-MapReduce to access Alibaba Cloud resources without using an AK. By default, the following permission policies are granted to AliyunEmrEcsDefaultRole:

```
{
  "Version": "1",
  "Statement": [
    {
      "Action": [
        "oss:GetObject",
        "oss:ListObjects",
        "oss:PutObject",
        "oss:DeleteObject",
        "oss:ListBuckets",
        "oss:AbortMultipartUpload"
      ],
      "Resource": "*",
      "Effect": "Allow"
    }
  ]
}
```

By default, operations based on MetaService can only access OSS data. If you want to use MetaService to access other Alibaba Cloud resources, such as LogService, you must grant permissions to AliyunEmrEcsDefaultRole. Perform the preceding operations on the [RAM console](https://partners-intl.aliyun.com/login-required#/ram/role/list).

**Note:** MetaService only supports AK-free operations on OSS, LogService, and MNS data. Modify and delete the default role with caution. Otherwise, you may fail to create or perform operations on clusters.

## Custom application roles {#section_nwz_mpt_1fb .section}

When creating a cluster, you can use a default role or create your own application role. In most cases, you only need to use or modify the default role. For more information about how to create and authorize a role to E-MapReduce, see [RAM](https://partners-intl.aliyun.com/help/product/28625.htm?).

## Access to MetaService {#section_djk_npt_1fb .section}

MetaService is an HTTP service that can be accessed directly to obtain metadata. For example, by using the curl http://localhost:10011/cluster-region command, you can obtain the region where the current cluster is located.

MetaService supports the following types of information:

-   Region: /cluster-region
-   Role name: /cluster-role-name
-   AccessKeyId: /role-access-key-id
-   AccessKeySecret: /role-access-key-secret
-   SecurityToken: /role-security-token
-   Network type: /cluster-network-type

## Use MetaService {#section_bm5_npt_1fb .section}

You can use MetaService to access Alibaba Cloud resources without using an AK. This has the following advantages:

-   Reduces the risk of an AK leak. The use of RAM also minimizes security risks, as only the required permissions are granted to the role.
-   Improves user experience. For instance, when you access OSS resources interactively, you no longer need to write a long string of OSS paths.

The usage methods are as follows:

```
I. Using the Hadoop command line to display OSS data
    Previously, we used: hadoop fs -ls oss://ZaH******As1s:Ba23N**************sdaBj2@bucket.oss-cn-hangzhou-internal.aliyuncs.com/a/b/c
    Now, we use: hadoop fs -ls oss://bucket/a/b/c
II. Using Hive to create a table
    Previously, we used:
        CREATE EXTERNAL TABLE test_table(id INT, name string)
        ROW FORMAT DELIMITED
        FIELDS TERMINATED BY '/t'
        LOCATION 'oss://ZaH******As1s:Ba23N**************sdaBj2@bucket.oss-cn-hangzhou-internal.aliyuncs.com/a/b/c';
    Now, we use:
        CREATE EXTERNAL TABLE test_table(id INT, name string)
        ROW FORMAT DELIMITED
        FIELDS TERMINATED BY '/t'
        LOCATION 'oss://bucket/a/b/c';
III. Spark
    Previously, we used: val data = sc.textFile("oss://ZaH******As1s:Ba23N**************sdaBj2@bucket.oss-cn-hangzhou-internal.aliyuncs.com/a/b/c")
    Now, we use: val data = sc.textFile("oss://bucket/a/b/c")
```

