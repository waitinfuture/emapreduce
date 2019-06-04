# Zeppelin 使用说明 {#concept_vpf_j1x_y2b .concept}

阿里云 E-MapReduce 可以通过 Apache Knox 访问 Zeppelin。

## 准备工作 {#section_zsr_k1x_y2b .section}

1.  在集群[安全组](../../../../intl.zh-CN/集群规划与配置/集群配置/安全组.md#)中[设置安全组规则](../../../../intl.zh-CN/安全/安全组/添加安全组规则.md#)，打开 8080 端口。
2.  在 Knox 中，添加访问用户名和密码，详细参见 [Knox 使用说明](intl.zh-CN/开源组件介绍/Knox 使用说明.md#)，设置 Knox 用户。用户名和密码仅用于登录 Knox 的各项服务，与阿里云 RAM 的用户名无关。

**说明：** 设置安全组规则时要针对有限的IP范围。禁止在配置的时候对 0.0.0.0/0 开放规则。

## 访问 Zeppelin {#section_btr_k1x_y2b .section}

访问链接查看方式如下：

1.  在 E-MapReduce 集群管理页面单击集群 ID 右侧的**管理**。
2.  在页面左侧导航栏单击**访问链接与端口**。

