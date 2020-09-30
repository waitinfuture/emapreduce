# Create a gateway cluster

A gateway cluster is composed of Elastic Compute Service \(ECS\) instances that reside in the same internal network as the E-MapReduce \(EMR\) cluster associated with the gateway cluster. You can use a gateway cluster to achieve load balancing and security isolation and to submit jobs to the EMR cluster.

An EMR gateway cluster supports only Hadoop and Kafka clusters. Before you create a gateway cluster, make sure that you have created an EMR Hadoop or Kafka cluster.

## Procedure

1.  Log on to the [EMR console](https://emr.console.aliyun.com/).

2.  In the top navigation bar, select the region where your cluster resides. Select the resource group as required. By default, all resources of the account appear.

3.  Click the **Cluster Management** tab.

4.  In the upper-right corner of the **Cluster Management** page, click **CreateGateway**.

5.  On the **Create Gateway** page, configure parameters in the following table.

    |Section|Parameter|Description|
    |-------|---------|-----------|
    |Basic Information|**Cluster Name**|The name of the gateway cluster. The name must be 1 to 64 characters in length and can contain only letters, digits, hyphens \(-\), and underscores \(\_\).|
    |**Assign Public IP Address**|Specifies whether an EIP address is associated with a gateway cluster.|
    |**Password** and **Key Pair**|    -   **Password**: the password used to log on to the gateway cluster. The password must be 8 to 30 characters in length and must contain uppercase letters, lowercase letters, digits, and special characters.

The following special characters are supported:

!@\#$%^&\*

    -   **Key Pair**: the name of the key pair used to log on to the gateway cluster. If no key pair is created, click **Create Key Pair** next to this field to go to the SSH Key Pairs page of the ECS console and create a key pair.

Keep the .pem private key file safe. After a gateway cluster is created, the public key is automatically bound to the ECS instance. When you log on to the gateway cluster by using SSH, you must enter the private key in the private key file. |
    |**Billing Method**|    -   **Subscription**: The system charges you for a cluster only once per subscription period. The unit price of a subscription cluster is lower than that of a pay-as-you-go cluster of the same specification. Subscription clusters are offered discounts, and longer subscription periods offer larger discounts.
    -   **Pay-As-You-Go**: The system charges you for a cluster based on the time the cluster is actually used in hours. |
    |Cluster Configuration|**Associated Cluster**|The cluster associated with the gateway cluster. The gateway cluster submits jobs to this cluster.|
    |**Zone**|The zone where the associated cluster resides.|
    |**Network Type**|The network type of the associated cluster.|
    |**VPC**|The Virtual Private Cloud \(VPC\) to which the associated cluster belongs.|
    |**VSwitch**|The VSwitch you want the gateway cluster to use. Select the VSwitch that corresponds to the zone and VPC.|
    |**Security Group Name**|The name of the security group to which the associated cluster belongs.|
    |Advanced Settings|**Permission Settings**|The RAM roles that allow applications running in a cluster to access other Alibaba Cloud services. You can use the default RAM roles.    -   **EMR Role**: The default value is AliyunEMRDefaultRole and cannot be changed. This RAM role authorizes a cluster to access other Alibaba Cloud services, such as ECS and OSS.
    -   **ECS Role**: You can also assign an application role to a cluster. Then, EMR applies for a temporary AccessKey pair when applications running on the compute nodes of that cluster access other Alibaba Cloud services, such as OSS. This way, you do not need to manually enter an AccessKey pair. You can grant the access permissions on specific Alibaba Cloud services to the application role as required. |
    |**Bootstrap Actions**|Optional. You can configure bootstrap actions to run custom scripts before a cluster starts. For more information, see [Bootstrap actions](/intl.en-US/Cluster Management/Third-party software/Bootstrap actions.md).|
    |Instance|**Gateway Instance**|The available instance types in the current zone. For more information, see [Instance families](/intl.en-US/Instance/Instance families.md).     -   **System Disk Type**: the type of the system disk you want the gateway cluster to use. System disks are classified into ultra disks, standard SSDs, and ESSDs. The system disk types that are available for you to create a gateway cluster depend on the selected region and instance type. By default, the system disks are released after the relevant cluster is released.
    -   **Disk Size**: the size of the system disk. Unit: GB. Valid values: 40 to 2048. Default value: 300.
    -   **Data Disk Type**: the type of data disks you want the gateway cluster to use. Data disks are classified into ultra disks, standard SSDs, and ESSDs. The data disk types that are available for you to create a gateway cluster depend on the selected region and instance type. By default, the data disks are released after the relevant cluster is released.
    -   **Disk Size**: the size of a data disk. Unit: GB. Valid values: 200 to 4000. Default value: 300.
    -   **Count**: the number of data disks. Valid values: 1 to 10. |

6.  Read and select E-MapReduce Service Terms and click **Create**.

    The created gateway cluster appears in the cluster list and its state changes from **Initializing** to **Idle** a few minutes later.


