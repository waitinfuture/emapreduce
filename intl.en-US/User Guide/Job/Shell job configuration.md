# Shell job configuration {#concept_thc_rjp_y2b .concept}

In this tutorial, you will learn how to configure a Shell job.

**Note:** By default, Shell scripts are currently run by Hadoop. If it is required to use root user, sudo can be used. Use Shell script jobs with caution.

## Procedure {#section_eww_tjp_y2b .section}

1.  Log on to the [Alibaba Cloud E-MapReduce Console](https://emr.console.aliyun.com/?spm=5176.8250060.103.1.48466f55SEaqMe#/cn-hangzhou).
2.  At the top of the navigation bar, click **Data Platform**.
3.  In the **Actions** column, click **Design Workflow** of the specified project.
4.  On the left side of the Job Editing page, right-click on the folder you want to operate and select **New Job**.
5.  In the **New Job** dialog box, enter the job name and description.
6.  Select the Shell job type to create a Bash Shell job.
7.  Click **OK**.

    **Note:** You can also create subfolder, rename folder, and delete folder by right-clicking on the folder.

8.  Enter the parameters in the**Content** box with parameters subsequent to Shell commands.
    -   -c option

        -c option can be used to set Shell scripts to run by inputting it into the **Content** box of the job, for example:

        ```
        -c "echo 1; sleep 2; echo 2; sleep 4; echo 3; sleep 8; echo 4; sleep 16; echo 5; sleep 32; echo 6; sleep 64; echo 8; sleep 128; echo finished"
        ```

    -   -f option

        -f option can be used to run Shell script files. By uploading a Shell script file to OSS, Shell scripts on OSS can be directly defined in the job parameters. This is more flexible than the -c option, for example:

        ```
        -f ossref://mxbucket/sample/sample-shell-job.sh
        ```

9.  Click **Save** to complete Shell job configuration.

