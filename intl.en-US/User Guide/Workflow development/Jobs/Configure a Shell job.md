# Configure a Shell job {#concept_thc_rjp_y2b .concept}

In this tutorial, you will learn how to configure a Shell job.

**Note:** By default, Shell scripts are currently run by Hadoop. If you need to use the root user, sudo can be used. Use Shell script jobs with caution.

## Procedure {#section_eww_tjp_y2b .section}

1.  Log on to the [Alibaba Cloud E-MapReduce console](https://emr.console.aliyun.com/?spm=5176.8250060.103.1.48466f55SEaqMe#/cn-hangzhou).
2.  At the top of the navigation bar, click **Data Platform**.
3.  In the **Actions** column, click **Design Workflow** next to the specified project.
4.  On the left of the Job Editing page, right-click the folder you want to operate and select **New Job**.
5.  In the **New Job** dialog box, enter the job name and description.
6.  Select the Shell job type to create a Bash Shell job.
7.  Click **OK**.

    **Note:** You can also create subfolders, rename folders, and delete folders by right-clicking on them.

8.  Enter the parameters in the **Content** field after the Shell commands.
    -   -c option

        -c options can be used to set Shell scripts to run by inputting them into the **Content** field of the job. For example:

        ```
        -c "echo 1; sleep 2; echo 2; sleep 4; echo 3; sleep 8; echo 4; sleep 16; echo 5; sleep 32; echo 6; sleep 64; echo 8; sleep 128; echo finished"
        ```

    -   -f option

        -f options can be used to run Shell script files. By uploading a Shell script file to OSS, Shell scripts on OSS can be defined in the job parameters, making it more flexible than the -c option. For example:

        ```
        -f ossref://mxbucket/sample/sample-shell-job.sh
        ```

9.  Click **Save** to complete Shell job configurations.

