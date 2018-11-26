# MetaService {#concept_wj5_fpt_1fb .concept}

MetaService is provided under the E-MapReduce environment. MetaService allows you to access Alibaba Cloud resources in the E-MapReduce cluster without using AK.

## Default roles {#section_xwf_3pt_1fb .section}

By default, you must authorize an application role \(AliyunEmrEcsDefaultRole\) to E-MapReduce when creating a cluster. After authorization, you can perform operations on E-MapReduce to access Alibaba Cloud resources without the need to explicitly input AK. By default, the following permission policies are granted to AliyunEmrEcsDefaultRole:

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

By default, your operations based on MetaService can Access OSS Data Only. If you want to access other Alibaba Cloud resources, such as LogService by using MetaService, you must grant permissions to AliyunEmrEcsDefaultRole. You can perform the preceding operations by using the [RAM console](https://ram.console.aliyun.com/#/role/list).

**Note:** MetaService only supports AK-free operations on the OSS, LogService, and MNS data. You must edit and delete the default role with caution. Otherwise, your cluster creation or operations may fail.

## Custom application roles {#section_nwz_mpt_1fb .section}

In most cases, you only need to use a default role or modify the default role. E-MapReduce also allows you to create your own application role. When creating a cluster, you can use a default role or create your own application role. For more information about how to create and authorize a role to E-MapReduce, see [RAM documentations](https://www.alibabacloud.com/help/product/28625.html).

## Access to MetaService {#section_djk_npt_1fb .section}

MetaService is an HTTP service which can be accessed directly to obtain metadata information. For example, you can obtain the region where the current cluster is located using the curl http://localhost:10011/cluster-region command.

MetaService supports the following types of information:

-   Region: /cluster-region
-   Role name: /cluster-role-name
-   AccessKeyId: /role-access-key-id
-   AccessKeySecret: /role-access-key-secret
-   SecurityToken: /role-security-token
-   Network type: /cluster-network-type

## Use MetaService {#section_bm5_npt_1fb .section}

You can use MetaSerivce to access Alibaba Cloud resources without the need to use AK, which can:

-   Reduce the risk of an AK leak. The RAM-based usage can minimize security risk. The permissions are minimized by only granting the required permissions to the role.
-   Improve user experience. This is intended especially when you interactively access the OSS resources by using MetaService. You do not need to write a long string of OSS path.

Several usage methods are introduced as follows:

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

