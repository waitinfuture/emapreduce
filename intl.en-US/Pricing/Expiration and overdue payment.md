# Expiration and overdue payment {#concept_kr4_5sn_y2b .concept}

## Expiration description {#section_lgv_btn_y2b .section}

Expiration does not apply to Pay-As-You-Go clusters.

The expiration of subscribed clusters is divided into two categories:

If any ECS instance in the cluster expires, a notification of their expiration is displayed. Expired ECS instances in the cluster will run for extra 15 days. After 15 days, if your subscription is not renewed, the expired ECS instances will stop, and data will be retained for additional 15 days. If your subscription is not renewed at the end of 30 days, the instances are released and data is permanently deleted.

When E-MapReduce services expire, you are unable to implement any operations to the cluster from the console or call the API, and the automatic operation and maintenance \(monitoring, alarm, and other related tasks\) are also stopped.

If you want to continue using E-MapReduce services and ECS instances, you must renew your subscription. The E-MapReduce service and ECS instances can be renewed simultaneously through the E-MapReduce Product console. After renewal, both data and services are restored automatically.

If you want to release a cluster, you can wait for the cluster to stop, and ECS instances are released automatically, without the need for renewal.

## Overdue payment description {#section_ngv_btn_y2b .section}

Overdue payment does not apply to subscription clusters.

When any Pay-As-You-Go cluster you purchased is overdue for payment, that is, the balance in your current account cannot be paid for the expenses incurred in the previous hour, the E-MapReduce service will be stopped.

ECS instances in the cluster will continue to run for 15 days, after which the ECS instances stop running. Data is retained for extra 15 days, after which the instances are released and data is removed permanently. The E-MapReduce service stops until clusters in arrearage are restored or released.

## Others {#section_ogv_btn_y2b .section}

Different clusters do not affect one another. For example, if you have a subscription cluster and a Pay-As-You-Go cluster, if the latter is in the overdue payment state, the former works normally without being affected.

