# Zeppelin 使用说明 {#concept_vpf_j1x_y2b .concept}

阿里云EMR可以通过Apache Knox访问Zeppelin。

## 准备工作 {#section_zsr_k1x_y2b .section}

1.  在集群[安全组](cn.zh-CN/用户指南/集群/安全组.md#)中[设置安全组规则](https://help.aliyun.com/document_detail/25471.html)，打开8080端口。
2.  在Knox中，添加访问用户名和密码，详细参见[Knox 使用说明](cn.zh-CN/用户指南/开源组件介绍/Knox 使用说明.md#)，设置Knox用户。用户名和密码仅用于登录Knox的各项服务，与阿里云RAM的用户名无关。

**说明：** 设置安全组规则时要针对有限的IP范围。禁止在配置的时候对0.0.0.0/0开放规则。

## 访问Zeppelin {#section_btr_k1x_y2b .section}

访问链接查看方式如下：

1.  在EMR管理控制单击集群ID右侧的**配置管理**。
2.  页面左侧单击**访问链接与端口**。

