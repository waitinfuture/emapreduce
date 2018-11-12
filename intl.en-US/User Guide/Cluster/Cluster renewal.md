# Cluster renewal {#concept_ums_15n_y2b .concept}

When your subscription cluster service renewal is about to expire, you must renew the cluster to continue E-MapReduce cluster services. The cluster renewal includes E-MapReduce services and ECS instances.

## Renewal entrance {#section_jlq_nxn_y2b .section}

1.  Log on to the[Alibaba Cloud E-MapReduce console](https://emr.console.aliyun.com/).
2.  At the top of the page, click Cluster Management.
3.  In the cluster list, target the cluster that you want to renew.
4.  On the right side of the corresponding cluster, click **Renew** to enter the cluster renewal page.

## Renewal page {#section_mvf_mxn_y2b .section}

The figure is shown as follows:

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17860/154201631010451_en-US.png)

-   **ECS Expiration Date**: The expiration date of an ECS instance.
-   **EMR Expiration Date**: The expiration date of E-MapReduce services.
-   **Quantity**: The number of machines for instance groups.
-   **ECS List**: The ECS instance ID of the machine in the cluster.
-   **ECS Subscription Duration**: The renewal duration for ECS \(one-nine months and one-three years are supported\).
-   **EMR Subscription Duration**: The renewal duration for E-MapReduce. We recommend that you keep it consistent with ECS.
-   **Price**: The renewal price of E-MapReduce services and ECS instances.

## Pay for orders {#section_qvf_mxn_y2b .section}

**Note:** The fees are the sum of ECS renewal price and E-MapReduce service product price. If there are unpaid orders in the cluster list, you cannot expand or renew any clusters.

1.  Click **OK** to view the prompt box for successful order placement.
2.  Click **Go to the payment page**. The payment page displays the total amount and order details of E-MapReduce services ECS intances.
3.  Click **Confirm payment**.
4.  After you make the payment, click **Payment completed** to return to the cluster list page.

The expiration time of successfully renewed clusters displayed on the cluster list page is updated to the time after renewal. For the corresponding ECS instance, the expiration time after the renewal is usually updated after about three to five minutes.

If you confirm the renewal order, but don’t pay for it, **Cancel order** and **Make the payment** are displayed on the right side of the cluster. Click **Make the payment**to complete the corresponding order payment , or click **Cancel** to cancel the renewal.

