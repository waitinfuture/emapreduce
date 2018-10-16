# Execution plan authentication {#concept_x2b_q3v_1fb .concept}

E-MapReduce console - execution plan

## Master account access {#section_syl_t3v_1fb .section}

After logging on to E-MapReduce console with the master account, you can run the corresponding execution plan on the Execution plan page. Submit the jobs to the security cluster for execution and access the related open source component services involved in the jobs using the hadoop username.

## Subaccount access {#section_vng_w3v_1fb .section}

After logging on to E-MapReduce console with the RAM subaccount, you can run the corresponding execution plan on the Execution plan page. Submit the jobs to the security cluster for execution and access the related open source component services involved in the jobs using the corresponding username of the RAM subaccount.

## Examples {#section_izl_x3v_1fb .section}

-   The master account administrator can create multiple subaccounts \(such as A, B, and C\) as needed and grant the [Aliyun EMR Full Access](https://help.aliyun.com/document_detail/28639.html?spm=5176.doc28640.6.552.ZYJIXN) permissions to these subaccounts from the RAM console. Then, the subaccounts can log on to the E-MapReduce console and use the related functions.
-   The master account administrator may provide the subaccounts to developers.
-   After job creation and plan execution, the developers may start executing the execution plans to submit the jobs to the cluster, and then access the relevant component services in the cluster using usernames \(such as A, B, and C\) corresponding to their subaccounts.

    **Note:** Currently, the periodic execution plans are uniformly executed using the hadoop account.

-   Relevant permission control for component services, such as whether or not Account A has the permission to access a file in hdfs, is performed by using the username of a subaccount.

