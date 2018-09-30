# OSS 数据权限隔离 {#concept_jrw_vnl_z2b .concept}

## 操作步骤 {#section_nbm_14l_z2b .section}

E-MapReduce 支持使用 RAM 来隔离不同子账号的数据。操作步骤如下所示：

1.  登录[阿里云 RAM 的管理控制台](https://ram.console.aliyun.com/)。
2.  在RAM中创建子账号，具体流程请参见[如何在 RAM 中创建子账号](https://www.alibabacloud.com/help/zh/doc-detail/28637.html)。
3.  单击[阿里云 RAM 的管理控制台](https://ram.console.aliyun.com/)页面左侧的**授权策略管理**，进入授权策略管理界面。
4.  单击**自定义授权策略**。
5.  单击页面右上方的**新建授权策略**按钮，即进入创建授权策略界面，然后按照提示步骤进行创建。您需要多少套不同的权限控制，就创建多少个策略。

    假设您需要以下 2 套数据控制策略：

    -   测试环境， bucketname：test-bucket。其所对应的完整策略如下：

```
{
"Version": "1",
"Statement": [
{
"Effect": "Allow",
"Action": [
  "oss:ListBuckets"
],
"Resource": [
  "acs:oss:*:*:*"
]
},
{
"Effect": "Allow",
"Action": [
  "oss:Listobjects",
  "oss:GetObject",
  "oss:PutObject",
  "oss:DeleteObject"
],
"Resource": [
  "acs:oss:*:*:test-bucket",
  "acs:oss:*:*:test-bucket/*"
]
}
]
}
```

    -   生产环境， bucketname：prod-bucket。其所对应的完整策略如下：

        ```
        {
        "Version": "1",
        "Statement": [
        {
        "Effect": "Allow",
        "Action": [
          "oss:ListBuckets"
        ],
        "Resource": [
          "acs:oss:*:*:*"
        ]
        },
        {
        "Effect": "Allow",
        "Action": [
          "oss:Listobjects",
          "oss:GetObject",
          "oss:PutObject"
        ],
        "Resource": [
          "acs:oss:*:*:prod-bucket",
          "acs:oss:*:*:prod-bucket/*"
        ]
        }
        ]
        }
        ```

6.  单击[阿里云 RAM 的管理控制台](https://ram.console.aliyun.com/?spm=5176.6660585.774526198.1.2yNQJH#/policy/list/system)页面左侧的**用户管理**。
7.  找到需要将策略赋给的子账号条目，单击其右侧的**管理**按钮，进入用户管理页面。
8.  单击页面左侧的**用户授权策略**。
9.  单击右上角的**编辑授权策略**按钮，进入策略授权页面。
10. 选择并添加授权策略。
11. 单击**确定**，完成对子账号的策略授权。
12. 单击用户管理页面左侧的**用户详情**，进入子账号的用户详情页面。
13. 在 Web 控制台登录管理栏中，单击**启用控制台登录**，以打开子账号的登录控制台的权限。

## 完成并使用 {#section_dcf_jpl_z2b .section}

完成以上所有步骤以后，使用对应的子账号登录 E-MapReduce，会有以下限制：

-   在创建集群、创建作业和创建执行计划的 OSS 选择界面，可以看到所有的 bucket，但是只能进入被授权的 bucket。

-   只能看到被授权的 bucket 下的内容，无法看到其他 bucket 内的内容。

-   作业中只能读写被授权的 bucket，读写未被授权的 bucket 会报错。


