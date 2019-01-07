# Execution plan authentication {#concept_x2b_q3v_1fb .concept}

E-MapReduce clusters support execution plan authentication. You can authorize Alibaba Cloud Resource Access Management \(RAM\) user accounts to access execution plans using the master account.

## Master account access {#section_syl_t3v_1fb .section}

After logging on to the E-MapReduce console with the master account, you can run execution plans on the Execution plan page. Submit jobs to the security cluster for execution and access the related open source services involved in the jobs using the Hadoop user name.

## RAM user account access {#section_vng_w3v_1fb .section}

After logging on to the E-MapReduce console with the RAM user account, you can run execution plans on the Execution plan page. Submit jobs to the security cluster for execution and access the related open-source component services involved in the jobs using the user name of the RAM user account.

## Examples {#section_izl_x3v_1fb .section}

-   The master account administrator can create multiple RAM user accounts as required and grant them permissions from the RAM console. The RAM users can then log on to the E-MapReduce console and use the related functions.
-   The master account administrator provides RAM user accounts to developers.
-   After creating jobs and execution plans, developers start running them to submit jobs to the cluster. They can then access the relevant component services in the cluster using the user names that correspond to the RAM user accounts.

    **Note:** Periodic execution plans are currently uniformly executed using the Hadoop account.

-   Relevant permission control for component services, such as whether account A is permitted to access a file in HDFS, is performed using the user name of a RAM user.

