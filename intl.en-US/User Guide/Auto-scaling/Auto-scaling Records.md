# Auto-scaling Records {#concept_gcd_3lj_z2b .concept}

After the auto-scaling operation is complete, you can click the **Auto-scaling Records** tab on the top of the **Auto-scaling** page to see the records of auto-scaling operation and the nodes number after auto-scaling operation is completed.

The execution status of auto-scaling includes the following four types:

-   **Running**: Auto-scaling operation is being implemented.
-   **Success**: All specified nodes involved in the scaling rule are added or removed from the cluster.
-   **Partial success**: According to scaling rules, some nodes were successfully added or removed from the cluster, but some were failed due to disk quota or ECS inventory.
-   **Failure**: According to scaling rules, no node is added or removed from the cluster.

