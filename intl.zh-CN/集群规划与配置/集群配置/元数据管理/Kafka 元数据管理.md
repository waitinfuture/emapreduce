# Kafka 元数据管理 {#concept_p2h_tdk_wgb .concept}

Kafka 元数据管理 适用于 E-MapReduce-3.17.0 以上版本。对于新创建的 Kafka 集群，后台会自动创建 Kafka 元数据。

当前 Kafka 元数据管理支持以下功能：

-   新增与删除 Topic
-   查看 Topic 详情
-   修改 Topic 分区数与配置

**说明：** 由于后台回调限制，Topic 列表与 Topic 信息的更新有一定延迟。

## 添加 Topic {#section_jnc_dpf_xgb .section}

1.  打开 Kafka 数据管理首页，单击右上角**添加 Topic**。
2.  指定 Topic 名，分区数等必填信息，以及对应的集群，在高级配置中可单击右上角**+**号增加配置项。
3.  创建完成后打开任务列表查看执行结果。

## 删除 Topic {#section_ywk_qpf_xgb .section}

您可以根据 Kafka 集群（只显示活跃的集群）和 Topic 名字筛选 Topic。对于已经释放的集群，Topic 列表只展示最后一次更新的详情。可以对活跃的 Topic 执行**删除**和**更新配置**，请在**任务列表**中查看执行结果。

**说明：** 内置的 Topic（如 \_\_consumer\_offsets）不支持删除与更新。

## 查看 Topic 详情 {#section_f1d_xpf_xgb .section}

在 Topic 列表页单击对应 Topic 后的**详情**可查看该 Topic 的摘要信息，数据分布和消费组。

-   数据分布： 根据分区数显示各个分区的状态。
-   消费组： 当前 Topic 关联的所有消费组的概况，单击名称可查看消费组对各分区的 offset 与 lag。

## 修改 Topic 分区数与配置 {#section_arl_mgk_wgb .section}

-   批量调整分区
    1.  打开 Kafka 数据管理首页，单击右上角**批量调整分区**。
    2.  在弹出的操作框中，单击**选择 Topic**，在一个集群下选择一到多个 Topic，单击**确定**。
    3.  分别指定调整后的分区数，单击**确定**。
    4.  在任务列表查看执行结果，如果操作成功，稍后控制台上的 Topic 详情将发生变化。

