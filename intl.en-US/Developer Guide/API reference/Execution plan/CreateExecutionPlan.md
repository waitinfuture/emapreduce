# CreateExecutionPlan {#concept_qwr_qjb_kfb .concept}

You can call this operation to create an execution plan.

## Request parameters {#section_vfq_zjb_kfb .section}

|Field|Type|Required|Default value|Description|
|-----|----|--------|-------------|-----------|
|RegionId|String|Yes|None|The ID of the region.|
|Name|String|Yes|None|The name of the execution plan.|
|Strategy|String|Yes|None|The running policy of the execution plan. RUN\_MANUALLY: Manual execution. Run the execution plan only when triggered by users. SCHEDULE: Periodic scheduling. Run the execution plan on the set interval.|
|StartTime|Long|No. It is required if Strategy==SCHEDULE.|None|The start time of the periodic scheduling. Unit: millisecond. The format is TIMESTAMP \(UTC\). For example, 1497516900000.|
|TimeUnit|String|No. It is required if Strategy==SCHEDULE.|None|The time interval for periodic scheduling: DAY: The unit is the day. HOUR: The unit is the hour.|
|TimeInterval|Integer|No. It is required if Strategy==SCHEDULE.|None|The time interval. If the unit is the day, set the value to 1. If the unit is the hour, set the value to a number in the range from 1 to 23.|
|JobIdList|Array|Yes|None|The array of the job IDs. For example, \[“J-90D5FC26DA8C7CA7”,”J-A5B1F503752DE01F”\].|
|CreateClusterOnDemand|Boolean|No|false|Specifies whether to create a cluster on demand dynamically.|
|ClusterId|String|No. It is required if CreateClusterOnDemand=false.|None|The ID of the existing cluster.|
|When you set CreateClusterOnDemand to true, the cluster is created on demand according to the settings of the following parameters.| | | | |
|ClusterName|Boolean|Yes|None|The name of the newly created cluster.|
|ZoneId|String|No|None|The zone location ID. For example, cn-hangzhou-b.|
|LogEnable|Boolean|Yes|None|Specifies whether to enable storing logs. Before using this function, make sure you have activated [OSS](https://www.alibabacloud.com/product/oss/).|
|LogPath|String |Required only when LogEnable==true|None|The location of the log that is stored in OSS. The format is: oss://bucketname/dir|
|SecurityGroupId|String|Yes|None|The ID of a security group. You can create a security group in the ECS console and use it. Note: If you are using an existing security group, the default security group policy is applied to this security group: Only port 22 is open at the inbound and all ports are open at the outbound.|
|IsOpenPublicIp|Boolean|No|true|Specifies whether to enable the public network IP address. The default bandwidth is 8 MB.|
|EmrVer|String|Yes|None|The product version of E-MapReduce. For example, EMR 1.0.0 and EMR 1.1.0.|
|ClusterType|String|Yes| |The type of the cluster. Hadoop is supported. Hbase is not supported.|
|HighAvailabilityEnable|Boolean|No|false|Specifies whether to enable high availability. If it is enabled, a minimum of two master nodes is required.|
|EcsOrder|[EcsOrder](EN-US_TP_18030.dita#concept_x1c_csb_kfb)|Yes|None|The machine information of ECS instances contained by the cluster. The format is JSON. For example, \[\{"nodeCount":3, "nodeType":"MASTER", "instanceType":"ecs.n1.large", "diskType":"CLOUD\_EFFICIENCY", "diskCapacity":80,diskCount":1\}\]|
|BootstrapActions|List [BootstrapAction](EN-US_TP_18031.dita#concept_v4l_ksb_kfb)|No|None|The lists of bootstrap actions. The maximum number is 16. If the maximum is exceeded, only the first 16 lists are retained.|
|Configurations|String|No|None|The OSS file path. For the contents of this file, see the user manual.|
|VpcId|String|No|None|The VPC ID.|
|VSwitchId|String|No|None|The ID of VSwitch.|
|NetType|String|No|classic|Valid values: classic and vpc. Default value: classic.|
|IoOptimized|Boolean|No|true|Specifies whether to enable IO optimization. The default value is true.|
|InstanceGeneration|String|No|None|The generation of the ECS instance. Set the value to ecs-1 or ecs-2.|

## Response parameters {#section_ccq_bkb_kfb .section}

|Field|Type|Description|
|-----|----|-----------|
|Id|String |The ID of the execution plan.|

## Examples {#section_ed1_dkb_kfb .section}

-   Sample requests

    ```
    https://emr.aliyuncs.com/?Action=CreateExecutionPlan
    &ClusterName=Test
    &ClusterType=HADOOP
    &LogEnable=false
    &SecurityGroupId=sg-234r6xoqe
    &IsOpenPublicIp=true
    &ZoneId=cn-hangzhou-b
    &EmrVer=EMR+1.1.0
    &EcsOrder=[{"nodeCount":3,"nodeType":"master","instanceType":"ecs.n1.large","diskType":"CLOUD_EFFICIENCY","diskCapacity":80}]
    &MasterIndex=1
    &Name=MyJobFlow
    &StartTime=1497516900000
    &TimeInterval=1
    &Strategy=RUN_MANUALLY
    &CreateClusterOnDemand=true
    &JobIdList=%5B%22J-90D5FC26DA8C7CA7%22%2C%22J-A5B1F503752DE01F%22%5D
    &RegionId=cn-hangzhou
    &Common request parameters
    ```

-   Sample responses

    JSON format

    ```
    {
        "RequestId": "34B08619-2636-49F9-AB4E-CD8D347B1E07",
        "Id":"WF-13A570B821D4BAB3"
    }
    ```


