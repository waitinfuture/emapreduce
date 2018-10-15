# Spark SQL job configuration {#concept_vck_cjp_y2b .concept}

Spark SQL job configuration

**Note:** By default, the Spark SQL mode to submit a job is Yarn mode.

## Procedure {#section_ws2_gjp_y2b .section}

1.  Log on to the [Alibaba Cloud E-MapReduce Console Job List](https://emr.console.aliyun.com/?spm=5176.doc28084.2.1.gxpx8G#/job/region/cn-hangzhou).
2.  Click **Create a job** in the upper right corner to enter the job creation page.
3.  Enter the job name.
4.  Select the Spark SQL job type to create a Spark SQL job. This type of job is submitted in the background by using the following process:

    ```
    spark-sql [options] [cli option]
    ```

5.  Enter the **Parameters** in the option box with parameters subsequent to Spark SQL commands.
    -   -e option

        Directly write running SQL for -e options by inputting it into the **Parameters** box of the job, for example:

        ```
        -e "show databases;"
        ```

    -   -f option

        -f options can be used to specify a Spark SQL script file. Loading well prepared Spark SQL script files on OSS can give more flexibility. We recommend that you use this operation mode, for example:

        ```
        -f ossref://your-bucket/your-spark-sql-script.sql
        ```

6.  Select the policy for failed operations.
7.  Click **OK** to complete Spark SQL job configuration.

