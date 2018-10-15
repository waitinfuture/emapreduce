# Cluster renewal {#concept_ums_15n_y2b .concept}

When your Subscription cluster service renewal is due to expire, you must renew the cluster to continue E-MapReduce cluster services. The cluster renewal includes the renewal of the E-MapReduce service charge and ECS fees.

## Renewal entrance {#section_jlq_nxn_y2b .section}

1.  Log on to [Alibaba Cloud E-MapReduce Cluster List Page](https://emr.console.aliyun.com/?spm=5176.doc35223.2.1.GOk4g8#/cluster/region/cn-hangzhou).
2.  Specify the cluster to be renewed.
3.  Click **Renewal** under the corresponding cluster to enter the cluster renewal page.

## Renewal page {#section_mvf_mxn_y2b .section}

-   **Renewal**: Select the machine to be renewed.

-   **ECS instance ID**: The machine ECS instance ID in the cluster.

-   **Current ECS expiration time**: The ECS expiration date.

-   **Current E-MapReduce expiration time**: The E-MapReduce product expiration date.

-   **ECS renewal time**: The renewal duration for ECS \(1-9 months and 1-3 years are supported\).

-   **E-MapReduce renewal time**: The renewal duration of the E-MapReduce service at the node. We recommend that you keep it consistent with ECS.

-   **ECS renewal price**: Corresponding renewal price of the ECS nodes.

-   **E-MapReduce renewal price**: The renewal price of corresponding nodes of the E-MapReduce service.


## Pay for the order {#section_qvf_mxn_y2b .section}

**Note:** The fees for cluster renewal is the sum of ECS renewal price and E-MapReduce service product price. If there are unpaid orders in the cluster list, you cannot expand or renew any clusters.

1.  Click **Confirm** to view the prompt box for successful order placement \(be patient, as the prompt information may be delayed for a while\).
2.  Click **Pay for the order** to go to the order payment page. The payment page displays the total amount payable and order details. One is the order of E-MapReduce product fees, and others are the ECS orders of cluster renewal.
3.  Click **Confirm payment**to complete the payment.
4.  After you make the payment, click **Completed** to return to the cluster list page.

The expiration time of successfully renewed clusters displayed on the cluster list page is updated to the time after renewal. For corresponding ECS, the expiration time after the renewal is usually updated after about three to five minutes.

If you confirm the renewal order, but don’t pay for it, you can find the cluster entry on the cluster list page. **Pay** and **Cancel** are displayed in the operation column on the right side. You can click **Pay**to complete the corresponding order payment and cluster expansion processes, or click **Cancel** to cancel the renewal.

