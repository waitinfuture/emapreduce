# Expand a cluster {#concept_jl4_d4n_y2b .concept}

If your cluster resources \(computing and storage resources\) are insufficient, the cluster can be expanded horizontally. Only the core node can be expanded. The hardware configuration defaults to be consistent with the ECS purchased previously.

## Expansion entry {#section_yjf_h4n_y2b .section}

Select the clusters to be expanded on the cluster list page. Click **Adjust scale** to enter the cluster expansion page. You can also click **View details** to enter the cluster details page, and click **Adjust cluster scale** at the core node information.

## Expansion interface {#section_zjf_h4n_y2b .section}

**Note:** Only expansion is supported. Reduction is not supported.

-   Quantity of current master nodes: By default, the quantity is 1 and cannot be adjusted.

-   Quantity of current cores: Displays the quantity of all your current core nodes.

-   Increase the quantity of cores: Enter the quantity that you want to add. The total cost of the expanded cluster is displayed on the right side. Click Confirm to expand. For safety reasons, we strongly recommend that you expand small batches consisting of one or two units, rather than expanding a large quantity each time.


## Expansion status {#section_ckf_h4n_y2b .section}

To view the cluster expansion status, go to the core node information on the details page. The node being expanded is displayed as **Expanding**. When the status of this ECS changes to **Normal**, the ECS has been added into the cluster and can provide services normally.

