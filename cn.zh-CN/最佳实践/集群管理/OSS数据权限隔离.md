# OSS数据权限隔离

本文介绍如何使用访问控制RAM（Resource Access Management），对不同子账号的OSS数据进行隔离。

已获取云账号。

E-MapReduce支持使用RAM来隔离不同子账号的数据。

## 操作步骤

1.  云账号登录[RAM控制台](https://ram.console.aliyun.com/)。

2.  创建RAM用户。

    1.  在左侧导航栏中，单击**人员管理** \> **用户**。

    2.  单击**创建用户**。

        **说明：** 可一次性创建多个RAM用户。

    3.  输入**登录名称**和**显示名称**。

    4.  在**访问方式**区域，选中**控制台密码登录**或**编程访问**。

        -   **控制台密码登录**：完成对登录安全的基本设置，包括自动生成或自定义登录密码、是否要求下次登录时重置密码以及是否要求开启多因素认证。
        -   **编程访问**：自动为RAM用户生成访问密钥（AccessKey），支持通过API或其他开发工具访问阿里云。
        **说明：** 为了保障账号安全，建议仅为RAM用户选择一种登录方式，避免RAM用户离开组织后仍可以通过访问密钥访问阿里云资源。

    5.  单击**确定**。

3.  新建权限策略。

    RAM除了提供常用的默认权限策略外，还支持让您自定义权限策略，使得授权更加灵活。根据不同需要，您可创建多个权限策略。

    1.  在左侧导航栏中，**权限管理** \> **权限策略管理**。

    2.  单击**创建权限策略**。

    3.  填写**策略名称**。

    4.  选中**脚本配置**。

        **脚本配置**方法请参见[语法结构](/cn.zh-CN/权限策略管理/权限策略语言/权限策略语法和结构.md)编辑策略内容。 本例分别按照以下两个脚本示例来创建两个权限策略：

        |测试环境（test-bucket）|生产环境（prod-bucket）|
        |-----------------|-----------------|
        |        ```
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

|        ```
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
        ``` |

        按上述脚本示例进行权限隔离后，RAM用户在E-MapReduce控制台的限制如下：

        -   在创建集群、创建作业和创建工作流的OSS文件页面，可以看到所有的bucket，但是只能进入被授权的bucket。
        -   只能看到被授权的bucket下的内容，无法看到其他bucket内的内容。
        -   作业中只能读写被授权的bucket，读写未被授权的bucket会报错。
    5.  单击**确定**。

4.  为RAM用户授权。

    创建的RAM用户未授权，则您可按以下方法进行授权。

    1.  单击左侧导航栏的**人员管理** \> **用户**。

    2.  单击待授权RAM用户所在行的**添加权限**。

    3.  单击需要授予RAM用户的权限策略，单击**确定**。

    4.  单击**完成**。

5.  开启RAM用户的控制台登录权限。

    如果创建RAM用户时未开启控制台登录权限，则您可按以下方法进行开启。

    1.  单击左侧导航栏的**人员管理** \> **用户**。

    2.  单击目标RAM用户的用户登录名称。

    3.  在**控制台登录管理**区域，单击**修改登录设置**。

    4.  **控制台密码登录**选中**开启**。

    5.  单击**确定**。

6.  通过RAM用户登录E-MapReduce控制台。

    1.  RAM用户登录[控制台](https://signin.aliyun.com/login.htm)。

    2.  登录控制台后，选择**E-MapReduce**产品即可。


