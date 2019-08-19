# MetaService {#concept_wj5_fpt_1fb .concept}

E-MapReduce 环境下提供 MetaService 服务。基于此服务，您可以在 E-MapReduce 集群中以免 AK 的方式访问阿里云资源。

## 默认应用角色 {#section_xwf_3pt_1fb .section}

默认地，您在创建集群时将需要向 E-MapReduce 服务授权一个应用角色（AliyunEmrEcsDefaultRole）。授权之后，您在 E-MapReduce 上的作业将可以无需显式输入 AK 来访问阿里云资源。AliyunEmrEcsDefaultRole 默认授予以下权限策略：

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

所以默认情况下，基于 MetaService 的作业将只能访问 OSS 数据。如果您想基于 MetaService 访问其他阿里云资源，例如 LogService 等等，则需要给 AliyunEmrEcsDefaultRole 补充授予相应的权限。以上操作需要登录[RAM 控制台](https://ram.console.aliyun.com/#/role/list)完成。

**说明：** 当前 MetaService 服务只支持 OSS，LogService 和 MNS 数据的免 AK 操作。请谨慎编辑，删除默认角色，否则会造成集群创建失败或者作业运行失败。

## 自定义应用角色 {#section_nwz_mpt_1fb .section}

大多数情况下，您只需要使用默认应用角色或者修改默认应用角色即可。E-MapReduce 同时支持您使用自定义的应用角色。在创建集群时，您既可以使用默认应用角色，也可以选择自定义应用角色。如何创建角色并授权给服务，请参考 [RAM 的相关文档](https://www.alibabacloud.com/help/product/28625.html)。

## 访问 MetaService {#section_djk_npt_1fb .section}

MetaService 是一个 HTTP 服务，您可以直接访问这个 HTTP 服务来获取相关 Meta 信息：例如 curl http://localhost:10011/cluster-region可以获得当前集群所在 Region。

当前 MetaService 支持以下几类信息：

-   Region： /cluster-region
-   角色名： /cluster-role-name
-   AccessKeyId：/role-access-key-id
-   AccessKeySecret：/role-access-key-secret
-   SecurityToken：/role-security-token
-   网络类型：/cluster-network-type

## 使用MetaService {#section_bm5_npt_1fb .section}

基于 MetaSerivce 服务，我们可以在作业中免 AK 地访问阿里云资源，这样可以带来两个优势：

-   降低 AK 泄漏的风险。基于 RAM 的使用方式，可以将安全风险降到最低。需要什么权限就给角色授予什么权限，做到权限最小化。
-   提高用户体验。尤其在交互式访问 OSS 资源时，可以避免写一长串的 OSS 路径。

下面示例几种使用方式：

``` {#codeblock_gnz_8jd_c02}
I. Hadoop 命令行查看 OSS 数据
    旧方式： hadoop fs -ls oss://ZaH******As1s:Ba23N**************sdaBj2@bucket.oss-cn-hangzhou-internal.aliyuncs.com/a/b/c
    新方式： hadoop fs -ls oss://bucket/a/b/c
II. Hive建表
    旧方式：
        CREATE EXTERNAL TABLE test_table(id INT, name string)
        ROW FORMAT DELIMITED
        FIELDS TERMINATED BY '/t'
        LOCATION 'oss://ZaH******As1s:Ba23N**************sdaBj2@bucket.oss-cn-hangzhou-internal.aliyuncs.com/a/b/c';
    新方式：
        CREATE EXTERNAL TABLE test_table(id INT, name string)
        ROW FORMAT DELIMITED
        FIELDS TERMINATED BY '/t'
        LOCATION 'oss://bucket/a/b/c';
III. Spark
    旧方式： val data = sc.textFile("oss://ZaH******As1s:Ba23N**************sdaBj2@bucket.oss-cn-hangzhou-internal.aliyuncs.com/a/b/c")
    新方式： val data = sc.textFile("oss://bucket/a/b/c")
```

