# CreateCluster {#concept_ahy_wy1_kfb .concept}

You can this operation to create a cluster.

## Request parameters {#section_wbp_dz1_kfb .section}

|Field|Type|Required|Default value|Description|
|-----|----|--------|-------------|-----------|
|Name|String|Yes|None|The name of the cluster. Length constraints: Minimum length of 1 character. Maximum length of 64 characters. Only Chinese characters, letters, numbers, hyphens \(-\), and underscores \(\_\) are allowed.|
|RegionId|String|Yes|None|Currently, the available regions are China East 1 \(Hangzhou\), China East 2 \(Shanghai\), China South 1 \(Shenzhen\), China North 2 \(Beijing\), China North 3 \(Zhangjiakou\), US West 1 \(Silicon Valley\), Asia Pacific SE 1 \(Singapore\), and Germany \(Frankfurt\).|
|ZoneId|String|No|None|The zone location ID of the supported available zone in the China East 1 \(Hangzhou\) region: cn-hangzhou-b. The zone location IDs of the supported available zones in the China East 2 \(Shanghai\) region: cn-beijing-a, cn-beijing-b, and cn-beijing-c.|
|LogEnable|Boolean|Yes|None|Specifies whether to enable or disable storing logs. Make sure you have activated [OSS](https://www.aliyun.com/product/oss/) to use this function.|
|LogPath|String|No. It is required if LogEnable==true.|None|The location of the log that is stored in OSS. The format is: oss://bucketname/dir|
|SecurityGroupId|String|No|None|The ID of a security group. You can create a security group in the ECS console and use it. Note: If you are using an existing security group, the default security group policy is applied to this security group. Only port 22 is open at the inbound and all ports are open at the outbound.|
|SecurityGroupName|String|No|None|If the ID of the security group is not specified, this name is used to create a new security group. After the cluster has been created, you can see the ID of the created security group in the detailed information of the cluster. The default security group policy is applied to this security group. Only port 22 is open at the inbound and all ports are open at the outbound.|
|IsOpenPublicIp|Boolean|No|true|Specifies whether to set the public IP address. The default bandwidth is 8 MB.|
|EmrVer|String|Yes|None|The product version of E-MapReduce. For example, emr-2.4.1, emr-3.0.1.|
|ClusterType|String|Yes|None|The type of the cluster. Currently, only the Hadoop type is supported.|
|HighAvailabilityEnable|Boolean|No|false|Specifies whether to enable or disable high availability. If high availability is enabled, a minimum of two master nodes is required.|
|MasterPwdEnable|Boolean|No|None|Indicates whether to set the root passwords for master nodes.|
|MasterPwd|String|No. It is required if MasterPwdEnable==true.|None|The password must meet the following requirements. Length constraints: Minimum length of 8 characters. Maximum length of 30 characters. It must contain three types \(uppercase letters, lowercase letters, numbers, and special symbols\) of characters.|
|EcsOrder|String|Yes|None|The machine information of ESC instances contained by clusters. It is in JSON format. For example，”\[\{‘NodeCount’:1, ‘NodeType’:’MASTER’,’InstanceType’:’ecs.n1.large’,’DiskType’:’CLOUD\_EFFICIENCY’,’DiskCapacity’:80,’DiskCount’:1,’Index’:1\},\{‘NodeCount’:2, ‘NodeType’:’CORE’,’InstanceType’:’ecs.n1.large’,’DiskType’:’CLOUD\_EFFICIENCY’,’DiskCapacity’:40,’DiskCount’:4,’Index’:2\}\]“ [EcsOrder](EN-US_TP_18030.dita#concept_x1c_csb_kfb)|
|ChargeType|String|Yes|None|The billing method. PostPaid: Pay-As-You-Go. PrePaid: Subscription.|
|Period|Integer|Only required when ChargeType==PrePaid|None|The length of the subscription. Unit: months. Valid values: 1, 2, 3, 4, 5, 6, 7, 8, 9, 12, 24, and 36.|
|BootstrapActions|List [BootstrapAction](EN-US_TP_18031.dita#concept_v4l_ksb_kfb)|No|None|The lists of bootstrap actions. The maximum number is 16. If the maximum is exceeded, only the first 16 lists are retained.|
|Configurations|String|No|None|Provides the path of an OSS file. See the user guide for the file content.|
|VpcId|String|No|None|VPC ID|
|VSwitchId|String|No|None|The ID of VSwitch|
|NetType|String|No|classic|Valid values: classic and vpc. Default value: classic.|
|IoOptimized|Boolean|No|true|Specifies whether to enable or disable IO optimization. Default value: true.|
|instanceGeneration|String|No|None|The generation of the ECS instance. Set the value to ecs-1 or ecs-2.|

## Response parameters {#section_xxl_hz1_kfb .section}

|Field|Type|Description|
|-----|----|-----------|
|ClusterId|String|The ID of the cluster.|
|EmrOrderId|String |The ID of the EMR service order.|
|MasterOrderId|String |The ID of the master node service order.|
|CoreOrderId|String|The ID of the core node service order.|

## Examples {#section_yr3_jz1_kfb .section}

-   Sample requests

    ```
    https://emr.aliyuncs.com/?Action=CreateCluster
    &Name=smokeTestCreateCluster1
    &ClusterType=HADOOP
    &HighAvailabilityEnable=false
    &SecurityGroupId=sg-234r6xoqe
    &LogEnable=false
    &EmrVer=EMR+1.1.0
    &ZoneId=cn-hangzhou-b
    &IsOpenPublicIp=true
    &RegionId=cn-hangzhou
    &MasterPwdEnable=false
    &VpcId=vpc-239kkz237
    &VSwitch=vsw-234iqq7ae
    &NetType=vpc
    &EcsOrder=[{"nodeCount":1,"nodeType":"master","instanceType":"ecs.n1.large","diskType":"CLOUD_EFFICIENCY","diskCapacity":80}]
    &Common request parameters
    ```

-   Sample responses

    JSON format

    ```
    {
        "RequestId": "34B08619-2636-49F9-AB4E-CD8D347B1E07",
        "InstanceId": "C-13A570B821D4BAB3"
    }
    ```


