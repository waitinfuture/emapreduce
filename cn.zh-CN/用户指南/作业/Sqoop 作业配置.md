# Sqoop 作业配置 {#concept_dlq_ckp_y2b .concept}

**说明：** 只有 E-MapReduce 产品版本 V1.3.0（包括）以上支持 Sqoop 作业类型。在低版本集群上运行 Sqoop 作业会失败，errlog 会报不支持的错误。参数细节请参见[数据传输 Sqoop](../../../../cn.zh-CN/.md#)。

## 操作步骤 {#section_kzj_3kp_y2b .section}

1.  进入[阿里云 E-MapReduce 控制台作业列表](https://emr.console.aliyun.com/)。
2.  单击该页右上角的**创建作业**，进入创建作业页面。
3.  填写作业名称。
4.  选择 Sqoop 作业类型，表示创建的作业是一个 Sqoop 作业。Sqoop 作业在 E-MapReduce 后台使用以下的方式提交：

    ```
    sqoop [args]
    ```

5.  在**应用参数**选项框中填入 Sqoop 命令后续的参数。
6.  选择执行失败后策略。
7.  单击**确定**，Sqoop 作业即定义完成。

