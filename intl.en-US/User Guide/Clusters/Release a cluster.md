# Release a cluster {#concept_vrx_3pn_y2b .concept}

Pay-As-You-Go clusters can be released on the **Cluster Management** page. Only those with the following statuses can be released:

-   Creating

-   Running

-   Idle


## Common release {#section_dm4_kpn_y2b .section}

Before releasing a cluster, you are prompted to confirm the operation. Once the release is confirmed, the following occurs:

-   All jobs in the cluster are forcibly terminated.
-   If you have selected to save logs to OSS, all current job logs are saved to OSS. This may take several minutes.
-   Clusters are released. Depending on their size, this may take seconds or minutes. ECS clusters are billed before they are released.

    Warning:

    To save money, make sure to release a cluster before a new billable hour starts.


## Forcible release {#section_fm4_kpn_y2b .section}

If you no longer need logs and want to immediately terminate running clusters, perform a forcible release. Logs are not collected and clusters are released directly.

## Cluster release failure {#section_gm4_kpn_y2b .section}

Due to system errors or other causes, clusters may fail to be released. If a failure occurs, E-MapReduce starts background protection until the cluster is finally released.

