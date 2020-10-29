# 为RAM用户授权

为确保RAM用户能正常使用E-MapReduce控制台的功能，您需要使用云账号登录访问控制RAM（Resource Access Management），授予RAM用户相应的权限。

访问控制RAM是阿里云提供的资源访问控制服务，更多详情请参见[什么是访问控制](/intl.zh-CN/产品简介/什么是访问控制.md)。以下举例访问控制RAM的典型场景：

-   用户：如果您购买了多台E-MapReduce集群实例，您的组织里有多个用户（如运维、开发或数据分析）需要使用这些实例，您可以创建一个策略允许部分用户使用这些实例。避免了将同一个AccessKey泄露给多人的风险。
-   用户组：您可以创建多个用户组，并授予不同权限策略，授权过程与授权用户过程相同，可以起到批量管理的效果。

## 权限策略

权限策略分为系统策略和自定义策略。

-   系统策略：阿里云提供多种具有不同管理目的的默认权限策略。E-MapReduce经常使用的系统策略：
    -   AliyunEMRFullAccess：管理E-MapReduce的权限，主要包括对E-MapReduce的所有资源的所有操作权限。
    -   AliyunEMRDevelopAccess：E-MapReduce开发者权限，与AliyunEMRFullAccess策略相比，不授予集群的创建和释放等操作权限。
    -   AliyunEMRFlowAdmin：E-MapReduce数据开发的管理员权限，支持创建项目、开发和管理作业，但不支持添加项目成员和管理集群。
-   自定义策略：需要您精准地设计权限策略，适用于熟悉阿里云各种云服务API以及具有精细化控制需求的用户。您可以参见[权限策略语法和结构](/intl.zh-CN/权限策略管理/权限策略语言/权限策略语法和结构.md)创建自定义策略。

## 授权RAM用户

执行以下步骤在访问控制RAM控制台授权RAM用户E-MapReduce相关权限。

1.  使用云账号登录[RAM控制台](https://ram.console.aliyun.com/?spm=a2c4g.11186623.2.6.77bd72fe3PD5pf#/policy/detail/system/AliyunEMRRolePolicy/info)。

2.  单击左侧导航栏的**人员管理** \> **用户**。

3.  单击待授权RAM用户所在行的**添加权限**。

4.  单击需要授予RAM用户的权限策略，单击**确定**。

    具体权限策略请参见[权限策略](#section_efm_tri_nux)。

5.  单击**完成**。

    完成授权后，权限立即生效，被授权的RAM用户可以登录[RAM控制台](https://ram.console.aliyun.com/?spm=a2c4g.11186623.2.6.77bd72fe3PD5pf#/policy/detail/system/AliyunEMRRolePolicy/info)。


