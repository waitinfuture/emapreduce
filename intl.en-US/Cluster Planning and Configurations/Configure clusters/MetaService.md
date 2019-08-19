# MetaService {#concept_wj5_fpt_1fb .concept}

E-MapReduce supports MetaService. MetaService allows you to access Alibaba Cloud resources from E-MapReduce clusters without providing the AccessKey.

## Default application role {#section_xwf_3pt_1fb .section}

You can grant the default application role AliyunEmrEcsDefaultRole to E-MapReduce when creating an E-MapReduce cluster. Then, your E-MapReduce jobs can access Alibaba Cloud resources without explicitly providing the AccessKey. By default, the following permissions are granted to AliyunEmrEcsDefaultRole:

``` {#codeblock_t59_2h5_moi}
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

That is, MetaService-based jobs can access only Object Storage Service \(OSS\) data by default. To allow MetaService-based jobs to access other Alibaba Cloud resources, such as Log Service data, you must grant the required permissions to AliyunEmrEcsDefaultRole. You can grant the default application role to E-MapReduce and configure the permissions of the role in the [Resource Access Management \(RAM\) console](https://partners-intl.aliyun.com/login-required#/ram/role/list).

**Note:** Currently, MetaService allows you to access only the data of OSS, Log Service, and Message Service \(MNS\) without providing the AccessKey. Edit or delete the default application role with caution. If the role is edited or deleted by mistake, you may fail to create clusters or run jobs.

## Custom application role {#section_nwz_mpt_1fb .section}

The default application role can meet most business requirements. You can directly use it or edit it as required. E-MapReduce also allows you to use a custom application role. That is, when creating a cluster, you can use the default application role or select a custom application role. For more information about how to create a role and grant it to a service, see [RAM documentation](https://partners-intl.aliyun.com/help/product/28625.htm?).

## Access MetaService {#section_djk_npt_1fb .section}

MetaService is a Hypertext Transfer Protocol \(HTTP\). You can access MetaService to obtain metadata information. For example, you can obtain the region where the current cluster resides by running the curl http://localhost:10011/cluster-region command.

Currently, you can use MetaService to obtain the following information:

-   Region: /cluster-region
-   Role name: /cluster-role-name
-   AccessKey ID: /role-access-key-id
-   AccessKey secret: /role-access-key-secret
-   Security token: /role-security-token
-   Network type: /cluster-network-type

## Use MetaService {#section_bm5_npt_1fb .section}

Your jobs can use MetaService to access Alibaba Cloud resources without providing the AccessKey, which brings the following benefits:

-   Reduces the risk of an AccessKey leakage. The use of a RAM role can minimize the security risk. You can grant only the required permissions to a role. This minimizes the permissions that are granted.
-   Improves user experience. MetaService is especially useful when you access OSS resources because it shortens the OSS path that you need to enter.

The following examples show how to use MetaService.

``` {#codeblock_gnz_8jd_c02}
I. Hadoop
    Previously, we used: hadoop fs -ls oss://ZaH******As1s:Ba23N**************sdaBj2@bucket.oss-cn-hangzhou-internal.aliyuncs.com/a/b/c
    Now, we use: hadoop fs -ls oss://bucket/a/b/c
II. Hive
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

