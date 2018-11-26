# Shell 作业配置 {#concept_thc_rjp_y2b .concept}

本文将介绍Shell作业配置的操作步骤。

**说明：** 目前 Shell 脚本默认是使用 Hadoop 用户执行的，如果需要使用 root 用户，可以使用 sudo。请谨慎使用 Shell 脚本作业。

## 操作步骤 {#section_eww_tjp_y2b .section}

1.  通过主账号登录[阿里云 E-MapReduce 控制台](https://emr.console.aliyun.com/)，进入集群列表页面。
2.  单击上方的数据开发页签，进入项目列表页面。
3.  单击对应项目右侧的**工作流设计**，进入作业编辑页面。
4.  在页面左侧，在需要操作的文件夹上单击右键，选择**新建作业**。
5.  填写作业名称，作业描述。
6.  选择 Shell 作业类型，表示创建的作业是一个 Bash Shell 作业。
7.  单击**确定**。

    **说明：** 您还可以通过在文件夹上单击右键，进行创建子文件夹、重命名文件夹和删除文件夹操作。

8.  在**作业内容**输入框中填入 Shell 命令后续的参数。
    -   -c 选项

        -c 选项可以直接设置要运行的 Shell 脚本，在作业**作业内容**输入框中直接输入，如下所示：

        ```
        -c "echo 1; sleep 2; echo 2; sleep 4; echo 3; sleep 8; echo 4; sleep 16; echo 5; sleep 32; echo 6; sleep 64; echo 8; sleep 128; echo finished"
        ```

    -   -f 选项

        -f 选项可以直接运行 Shell 脚本文件。通过将 Shell 脚本文件上传到 OSS 上，在 job 参数里面可以直接制定 OSS 上的 Shell 脚本，比使用 -c 选项更加灵活，如下所示：

        ```
        -f ossref://mxbucket/sample/sample-shell-job.sh
        ```

9.  单击**保存**，Shell 作业即定义完成。

