# Job operations {#concept_ctn_vkp_y2b .concept}

You can create, clone, modify, and delete jobs.

## Job creation {#section_zr2_wkp_y2b .section}

A new job can be created at any time. Currently, a job can only be used in the region where it is created.

## Job cloning {#section_as2_wkp_y2b .section}

Configurations that already exist for a job can be cloned. A cloned job can also only be used in the region where it is created.

## Job modification {#section_bs2_wkp_y2b .section}

Before you can modify a job that needs to be added to an execution plan, you must first ensure that the execution plan is not running and that its periodic scheduling is not in progress.

Before you can modify a job that needs to be added to several execution plans, you must first ensure that none of the execution plans are running and that none of their periodic scheduling is in progress. Modifying a job may result in changes to all of the execution plans that use this job.

If you need to debug, we recommend that you perform cloning instead. After you debug, the original jobs in the execution plan are replaced.

## Job deletion {#section_cs2_wkp_y2b .section}

As with modification, a job can only be deleted when the execution plan where the job is located is not running and its periodic scheduling is not in progress.

