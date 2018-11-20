# ModifyExecutionPlan {#concept_fbc_dlb_kfb .concept}

You can call this operation to modify en execution plan.

## Request parameters {#section_cr5_hlb_kfb .section}

|Field|Type|Required|Default value|Description|
|-----|----|--------|-------------|-----------|
|RegionId|String|Yes|None|The ID of the region.|
|Id|String|Yes|None|The ID of the execution plan.|
|Name|String|Yes|None|The name of the execution plan.|
|Strategy|String|Yes|None|The running policy of the execution plan. RUN\_MANUALLY: Manual execution. The execution plan runs only when triggered by users. SCHEDULE: Periodic scheduling. The execution plan runs based on the set interval.|
|StartTime|Long|No. It is required if Strategy==SCHEDULE.|None|The time when periodic scheduling takes effect.|
|TimeUnit|String|No. It is required if Strategy==SCHEDULE.|None|The time interval for periodic scheduling. DAY: The unit is the day. HOUR: The unit is the hour.|
|TimeInterval|Integer|No. It is required if Strategy==SCHEDULE.|None|The time interval. If the unit is the day, set the value to 1. If the unit is the hour, set the value to a number in the range from 1 to 23.|
|JobIdList|Array|Yes|None|The array of the job IDs. For example, \[“J-24H770B821D3C56A”,”J-8A37Y0B821D3C90R”\]|
|CreateClusterOnDemand|Boolean|No|false|Specifies whether to create a cluster on demand dynamically.|
|ClusterId|String|No. It is required if CreateClusterOnDemand==false.|None|The ID of the existing cluster.|
|When you set CreateClusterOnDemand to true, the cluster is created on demand according to the settings of the following parameters.| | | | |
|ClusterName|Boolean|No|None|The name of the newly created cluster.|
|ZoneId|String|No|None|The zone location ID. For example, cn-hangzhou-b.|
|LogEnable|Boolean|Yes|None|Specifies whether to enable or disable storing logs. Make sure you have activated [OSS](https://www.aliyun.com/product/oss/)before using this function.|
|LogPath|String|Only required when LogEnable==true|None|The location of the log that is stored in OSS. The format is: oss://bucketname/dir|
|SecurityGroupId|String|Yes|None|The ID of a security group. You can create a security group in the ECS console and use it. Note: If you are using an existing security group, the default security group policy is applied to this security group. Only port 22 is open at the inbound and all ports are open at the outbound.|
|IsOpenPublicIp|Boolean|No|true|Specifies whether to enable a public network IP address. The default bandwidth is 8 MB.|
|EmrVer|String|Yes|None|The product version of E-MapReduce. For example: EMR-2.4.1 or EMR-3.0.1|
|ClusterType|String|Yes|None|The type of the cluster. HADOOP is supported. HBASE is not supported.|
|HighAvailabilityEnable|Boolean|No|false|Specifies whether to enable or disable high availability. If high availability is enabled, a minimum of two master nodes is required.|
|EcsOrder|[EcsOrder](EN-US_TP_18030.dita#concept_x1c_csb_kfb)|Yes|None|The machine information of the ECS instances contained by the cluster is in JSON format. For example, \[\{“nodeCount”:3, “nodeType”:”MASTER”, “instanceType”:”ecs.n1.large”, “diskType”:”CLOUD\_EFFICIENCY”, “diskCapacity”:80,diskCount”:1\}\]|
|BootstrapActions|List [BootstrapAction](EN-US_TP_18031.dita#concept_v4l_ksb_kfb)|No|None|The lists of bootstrap actions. The maximum number is 16. If the maximum is exceeded, only the first 16 lists are retained.|
|Configurations|String|No|None|The OSS file path. For the contents of this file, see the user manual.|
|VpcId|String|No|None|VPC ID|
|VSwitchId|String|No|None|The ID of the VSwitch.|
|NetType|String|No|None|Valid values: classic and vpc. Default value: classic.|
|IoOptimized|Boolean|No|true|Indicates whether to enable IO optimization.|
|InstanceGeneration|String|No|No|The generation of ECS instances. Set the value to ecs-1 or ecs-2.|

## Response parameters {#section_ogy_jlb_kfb .section}

Common response parameters

## Examples {#section_z4h_klb_kfb .section}

-   Sample requests

    ```
    https://emr.aliyuncs.com/?Action=ModifyExecutionPlan
    &Id=WF-13A570B821D4BAB3
    &CreateClusterOnDemand=false
    &ClusterId=C-358770B821D4B293
    &JobIdList=%5B%22J-24H770B821D3C56A%22%2C%22J-8A37Y0B821D3C90R%22%5D
    &Name=%E9%A2%84%E5%8F%91%E6%B5%8B%E8%AF%95%E7%94%A8%E4%BE%8B_SSD%E7%9B%98HDFS%E5%86%99
    &StartTime=1
    &TimeInterval=1
    &Strategy=RUN_MANUALLY
    &RegionId=cn-hangzhou
    ```

-   Sample responses

    JSON format

    ```
    {
        "RequestId": "34B08619-2636-49F9-AB4E-CD8D347B1E07"
    }
    ```


