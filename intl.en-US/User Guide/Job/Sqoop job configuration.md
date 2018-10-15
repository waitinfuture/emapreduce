# Sqoop job configuration {#concept_dlq_ckp_y2b .concept}

Sqoop job configuration

**Note:** Only E-MapReduce products version V1.3.0 and higher support the Sqoop job type. Running a Sqoop job on lower cluster versions will fail, and errlog will report “Not supported” errors. For parameter details, refer to [Data Transmission Sqoop](https://www.alibabacloud.com/help/doc-detail/28133.htm?spm=a2c63.p38356.a3.3.246051barsyWuU#concept-p22-qkp-y2b).

## Procedure {#section_kzj_3kp_y2b .section}

1.  Log on to the [Alibaba Cloud E-MapReduce Console Job List](https://emr.console.aliyun.com/).
2.  Click **Create a job** in the upper right corner to enter the job creation page.
3.  Input the job name.
4.  Select the Sqoop job type to create a Sqoop job. In E-MapReduce back-end, Sqoop job will submit through the following process:

    ```
    sqoop [args]
    ```

5.  Complete the **Parameters** option box with parameters subsequent to Sqoop commands.
6.  Select the policy for failed operations.
7.  Click **OK** to complete Sqoop job definition.

