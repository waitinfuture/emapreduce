# MetaService {#concept_wj5_fpt_1fb .concept}

E-MapReduce环境下提供MetaService服务。基于此服务，您可以在E-MapReduce集群中以免AK的方式访问阿里云资源。

## 默认应用角色 {#section_xwf_3pt_1fb .section}

默认地，您在创建集群时将需要向E-MapReduce服务授权一个应用角色（AliyunEmrEcsDefaultRole）。授权之后，您在E-MapReduce上的作业将可以无需显式输入AK来访问阿里云资源。AliyunEmrEcsDefaultRole默认授予以下权限策略：

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

所以默认情况下，基于MetaService的作业将只能访问OSS数据。如果您想基于MetaService访问其他阿里云资源，例如LogService等等，则需要给AliyunEmrEcsDefaultRole补充授予相应的权限。以上操作需要登录[RAM控制台](https://ram.console.aliyun.com/#/role/list)完成。

**说明：** 当前metaservice服务只支持OSS，LogService和MNS数据的免AK操作。请谨慎编辑，删除默认角色，否则会造成集群创建失败或者作业运行失败。

## 自定义应用角色 {#section_nwz_mpt_1fb .section}

大多数情况下，您只需要使用默认应用角色或者修改默认应用角色即可。E-MapReduce同时支持您使用自定义的应用角色。在创建集群时，您既可以使用默认应用角色，也可以选择自定义应用角色。如何创建角色并授权给服务，请参考[RAM的相关文档](https://help.aliyun.com/product/28625.html)。

## 访问MetaService {#section_djk_npt_1fb .section}

MetaService是一个HTTP服务，您可以直接访问这个HTTP服务来获取相关Meta信息：例如 “ curl http://localhost:10011/cluster-region” 可以获得当前集群所在Region。

当前MetaService支持以下几类信息：

-   Region： /cluster-region
-   角色名： /cluster-role-name
-   AccessKeyId：/role-access-key-id
-   AccessKeySecret：/role-access-key-secret
-   SecurityToken：/role-security-token
-   网络类型：/cluster-network-type

## 使用MetaService {#section_bm5_npt_1fb .section}

基于MetaSerivce服务，我们可以在作业中免AK地访问阿里云资源，这样可以带来两个优势：

-   降低AK泄漏的风险。基于RAM的使用方式，可以将安全风险降到最低。需要什么权限就给角色授予什么权限，做到权限最小化。
-   提高用户体验。尤其在交互式访问OSS资源时，可以避免写一长串的OSS路径。

下面示例几种使用方式：

```
I. Hadoop命令行查看OSS数据
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

## 支持MetaService的数据源 {#section_efh_grt_1fb .section}

目前在E-MapReduce上支持MetaService的有OSS，LogService和MNS。您可以在E-MapReduce集群上使用E-MapReduce SDK的接口免AK来读写上述数据源。需要强调的是，MetaService默认只有OSS的读写权限，如果您希望MetaService支持LogService或者MNS，请前往RAM控制台修改AliyunEmrEcsDefaultRole，增加LogService和MNS的权限。具体如何配置角色的权限，参考RAM的相关文档说明。下面将举例说明如何在给AliyunEmrEcsDefaultRole添加访问LogService的权限：

-   在RAM的角色管理中找到AliyunEmrEcsDefaultRole，并单击**授权**。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17925/153690916111310_zh-CN.png)

-   搜索找到AliyunLogFullAccess权限，并添加。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17925/153690916111311_zh-CN.png)

-   添加完之后，我们就可以在AliyunEmrEcsDefaultRole的角色权限策略中看到已经添加的AliyunLogFullAccess权限策略。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17925/153690916111312_zh-CN.png)


这样，我们就可以在E-MapReduce集群中免AK访问LogService数据了。

**说明：** 这里演示了添加AliyunLogFullAccess权限策略，权限比较大，建议根据自己的实际需求，自定义一个权限策略，然后再授权给AliyunEmrEcsDefaultRole。

