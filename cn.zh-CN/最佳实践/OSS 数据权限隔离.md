# OSS 数据权限隔离 {#concept_jrw_vnl_z2b .concept}

本文将介绍如何使用RAM访问控制对不同子账号的OSS数据进行隔离。

## 前提条件 {#section_xk8_xsb_b6x .section}

已获取云账号。

## 背景信息 {#section_dcf_jpl_z2b .section}

E-MapReduce支持使用RAM来隔离不同子账号的数据。

## 步骤一 登录RAM控制台 {#section_wen_dqp_m4b .section}

1.  云账号登录[RAM控制台](https://ram.console.aliyun.com/)。

## 步骤二 创建RAM用户 {#section_scp_1y3_lp2 .section}

1.  在左侧导航栏的**人员管理**菜单下，单击**用户**。
2.  单击**新建用户**。

    **说明：** 单击**添加用户**，可一次性创建多个RAM用户。

3.  输入**登录名称**和**显示名称**。
4.  在**访问方式**区域下，选择**控制台密码登录**或**编程访问**。

    **说明：** 为了保障账号安全，建议仅为RAM用户选择一种登录方式，避免RAM用户离开组织后仍可以通过访问密钥访问阿里云资源。

5.  单击**确认**。

## 步骤三 新建权限策略 {#section_fzt_gus_48m .section}

RAM除了提供常用的默认权限策略外，还支持让您自定义权限策略，使得授权更加灵活。根据不同需要，您可创建多个权限策略。

1.  在左侧导航栏的**权限管理**菜单下，单击**权限策略管理**。
2.  单击**新建权限策略**。
3.  填写**策略名称**和**备注**。
4.  **配置模式**选择**脚本配置**。

    **脚本配置**方法请参见[语法结构](../../cn.zh-CN/用户指南/权限策略/权限策略语言/权限策略语法和结构.md#)编辑策略内容。 本例分别按照以下两个脚本示例来创建两个权限策略：

    |测试环境（test-bucket）|生产环境（prod-bucket）|
    |-----------------|-----------------|
    |     ``` {#codeblock_h5z_9ni_e3u}
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

 |     ``` {#codeblock_1g5_77a_u0x}
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

 |

    按上述脚本示例进行权限隔离后，RAM用户在E-MapReduce控制台的限制如下：

    -   在创建集群、创建作业和创建执行计划的 OSS 选择界面，可以看到所有的 bucket，但是只能进入被授权的 bucket。
    -   只能看到被授权的 bucket 下的内容，无法看到其他 bucket 内的内容。
    -   作业中只能读写被授权的 bucket，读写未被授权的 bucket 会报错。
5.  单击**确认**。

## 步骤四 为RAM用户授权 {#section_p68_c2w_zjf .section}

1.  在左侧导航栏的**人员管理**菜单下，单击**用户**。
2.  在**用户登录名称/显示名称**列表下，找到目标RAM用户。
3.  单击**添加权限**，被授权主体会自动填入。
4.  在左侧**权限策略名称**列表下，单击需要授予RAM用户的权限策略。

    **说明：** 在右侧区域框，选择某条策略并单击**×**，可撤销该策略。

5.  单击**确定**。
6.  单击**完成**。

## 步骤五 开启RAM用户的控制台登录权限（可选） {#section_nhy_pqf_2ad .section}

如果创建RAM用户时，未开启控制台登录权限，则您可按以下方法进行开启。

1.  在左侧导航栏的**人员管理**菜单下，单击**用户**。
2.  在认证管理区域，开启RAM用户的控制台登录权限。

## 步骤六 通过RAM用户登录E-MapReduce控制台 {#section_eoq_vld_2k3 .section}

1.  RAM用户登录[控制台](https://signin.aliyun.com/login.htm)。
2.  登录控制台后，选择**E-MapReduce**产品即可。

