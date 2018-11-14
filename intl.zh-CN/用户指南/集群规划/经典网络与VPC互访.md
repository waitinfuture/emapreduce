# 经典网络与VPC互访 {#concept_e4h_gp3_y2b .concept}

目前阿里云存在两种网络类型，一个是经典网络，一个是VPC。很多用户的业务系统因为历史的原因还会在经典网络中，而EMR集群是在VPC中，本节将会介绍如何使经典网络的ECS可以和VPC网络下的EMR集群网络互访。

## ClassicLink {#section_xs1_jp3_y2b .section}

为了解决这个问题，阿里云推出了[ClassLink方案](../../../../intl.zh-CN/用户指南/ClassicLink/ClassicLink概述.md#)。

大致步骤如下，详细的请参考上面的ClassLink文档：

1.  首先按照上面文档中指定网段创建vswitch。
2.  在创建集群的时候，请使用该网段的vswitch来部署集群。
3.  在ECS控制台将对应的经典网络节点连接到这个VPC。
4.  设置安全组访问规则。

然后就可以让经典网络的ECS和VPC的集群互访了。

