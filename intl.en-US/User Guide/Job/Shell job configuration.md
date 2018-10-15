# Shell job configuration {#concept_thc_rjp_y2b .concept}

Shell job configuration

**Note:** By default, Shell scripts are currently run by Hadoop. If it is required to use root user, sudo can be used. Use Shell script jobs with caution.

## Procedure {#section_eww_tjp_y2b .section}

1.  Log on to the [Alibaba Cloud E-MapReduce Console Job List](https://emr.console.aliyun.com/?spm=5176.doc28084.2.1.gxpx8G#/job/region/cn-hangzhou).
2.  Click **Create a job** in the upper right corner to enter the job creation page.
3.  Enter the job name.
4.  Select the Shell job type to create a Bash Shell job.
5.  Enter the **Parameters** in the option box with parameters subsequent to Shell commands.
    -   -c option

        -c option can be used to set Shell scripts to run by inputting it into the **Parameters** box of the job, for example:

        ```
        -c "echo 1; sleep 2; echo 2; sleep 4; echo 3; sleep 8; echo 4; sleep 16; echo 5; sleep 32; echo 6; sleep 64; echo 8; sleep 128; echo finished"
        ```

    -   -f option

        -f option can be used to run Shell script files. By uploading a Shell script file to OSS, Shell scripts on OSS can be directly defined in the job parameters. This is more flexible than the -c option, for example:

        ```
        -f ossref://mxbucket/sample/sample-shell-job.sh
        ```

6.  Select the policy for failed operations.
7.  Click **OK** to complete Shell job configuration.

