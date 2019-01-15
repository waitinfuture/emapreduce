# ClusterInfo {#concept_xw1_3qb_kfb .concept}

|Name|Type|Description|
|----|----|-----------|
|Id|String|The ID of the cluster.|
|RegionId|String|The region in which the cluster has been deployed.|
|Name|String |The name of the cluster.|
|CreateType|String|ON-DEMAND: A cluster that is created on demand using the execution plan. MANUAL: A cluster that is created manually.|
|ChargeType|String|The billing method for the cluster. PREPAID: Subscription. POSTPAID: Pay-As-You-Go.|
|StartTime|Long|The creation time of the cluster in TIMESTAMP format. For example, 1459236255438.|
|StopTime|Long|The stop time of the cluster in TIMESTAMP format. For example, 1459236255438.|
|LogEnable|Boolean|Specifies whether to enable storing the logs.|
|LogPath|String |The path of the stored log.|
|Status|String |[The status of the cluster.](../DNemapreduce1851503/EN-US_TP_18065.dita#concept_zkb_3cc_pfb)|
|HighAvailabilityEnable|Boolean|Enables or disables high availability.|
|RunningTime|Integer|Running time. Unit: seconds.|
|MasterNodeTotal|Integer|The total number of master nodes.|
|CoreNodeTotal|Integer|The total number of core nodes.|
|FailReason|[FailReason](EN-US_TP_18038.dita#concept_gct_ttb_kfb)|The cause of the failure.|
|SoftwareInfo|Array<[SoftwareInfo](EN-US_TP_18037.dita#concept_rpl_qtb_kfb)\>|The information about the software.|
|ImageId|String|The ID of the used image.|
|SecurityGroupId|String|The ID of the security group.|
|SecurityGroupName|String |The name of the security group.|
|EcsInstances|Array<[ECSInstance](EN-US_TP_18029.dita#concept_kfv_mqb_kfb)\>|The information of the ECS instances.|
|BootstrapActions|List<[BootstrapAction](EN-US_TP_18031.dita#concept_v4l_ksb_kfb)\>|The list of bootstrap actions. The maximum number is 16. If the maximum number has been exceeded, only the first 16 lists are retained.|
|Configurations|String|The OSS file path. For the contents of this file, see the user manual.|

