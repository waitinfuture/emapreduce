# Release a cluster {#concept_vrx_3pn_y2b .concept}

On the **Cluster Management** page, you can release a cluster.

Only Pay-As-You-Go clusters in the following statuses can be released:

-   Creating

-   Running

-   Idle


## Common release {#section_dm4_kpn_y2b .section}

You are prompted to confirm a release before releasing a cluster. Once the release is confirmed, the following operations will happen:

-   All jobs in the cluster are forcibly terminated.
-   If you have selected to save the log to OSS, all current job logs are saved to OSS. It takes several minutes to uplaod logs to OSS.
-   Clusters are released in seconds, up to five minutes. This process depends on the size of the cluster, the smaller the cluster will be faster. ECS clusters to be released are billed before they are released.

    Warning:

    If you want to save money, make sure that a cluster is released before an exact hour.


## Forcible release {#section_fm4_kpn_y2b .section}

If you no longer need logs, and want to immediately terminate running clusters, activate the forcible release function. The process of log collection is skipped and clusters are released directly.

## Cluster release failure {#section_gm4_kpn_y2b .section}

Due to system error or other causes, cluster releases may fail after confirmation. If a cluster release fails, E-MapReduce starts background protection to automatically release the cluster again until it is released successfully.

