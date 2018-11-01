# Job operations {#concept_ctn_vkp_y2b .concept}

You can create, clone, modify, and delete jobs.

## Job creation {#section_zr2_wkp_y2b .section}

A new job can be created at any time. The job created can currently only be used in the region where it is created.

## Job clone {#section_as2_wkp_y2b .section}

To completely clone configurations in which jobs already exist. It is also restricted to the same region.

## Job modification {#section_bs2_wkp_y2b .section}

If it is necessary to add a job into an execution plan, then it is required to ensure that this execution plan is not under operation and its periodic scheduling is not in progress before the job can be modified.

If it is necessary to add this job into several execution plans, the modification shall be made until the executing and periodic scheduling of all these plans. Since job modification may cause changes to all execution plans using this job and also errors to execution plans under executing and periodic scheduling.

If it is necessary to conduct debugging, clone is recommended, and after debugging, original jobs in the execution plan will be replaced.

## Job deletion {#section_cs2_wkp_y2b .section}

As with modification, jobs can be deleted only when the execution plan in which jobs are added in is not under operation and periodic scheduling is not in progress.

