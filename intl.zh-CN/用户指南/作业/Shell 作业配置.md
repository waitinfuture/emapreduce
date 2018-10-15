# Shell 作业配置 {#concept_thc_rjp_y2b .concept}

Shell 作业配置

**说明：** 目前 Shell 脚本默认是使用 Hadoop 用户执行的，如果需要使用 root 用户，可以使用 sudo。请谨慎使用 Shell 脚本作业。

## 操作步骤 {#section_eww_tjp_y2b .section}

1.  进入[阿里云 E-MapReduce 控制台作业列表](https://emr.console.aliyun.com/?spm=5176.doc28084.2.1.gxpx8G#/job/region/cn-hangzhou)。
2.  单击该页右上角的**创建作业**，进入创建作业页面。
3.  填写作业名称。
4.  选择 Shell 作业类型，表示创建的作业是一个 Bash Shell 作业。
5.  在**应用参数**选项框中填入 Shell 命令后续的参数。
    -   -c 选项

        -c 选项可以直接设置要运行的 Shell 脚本，在作业**应用参数**框中直接输入，如下所示：

        ```
        -c "echo 1; sleep 2; echo 2; sleep 4; echo 3; sleep 8; echo 4; sleep 16; echo 5; sleep 32; echo 6; sleep 64; echo 8; sleep 128; echo finished"
        ```

    -   -f 选项

        -f 选项可以直接运行 Shell 脚本文件。通过将 Shell 脚本文件上传到 OSS 上，在 job 参数里面可以直接制定 OSS 上的 Shell 脚本，比使用 -c 选项更加灵活，如下所示：

        ```
        -f ossref://mxbucket/sample/sample-shell-job.sh
        ```

6.  选择执行失败后策略。
7.  单击**确定**，Shell 作业即定义完成。

