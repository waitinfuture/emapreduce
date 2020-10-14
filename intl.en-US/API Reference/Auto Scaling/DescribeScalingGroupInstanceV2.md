# DescribeScalingGroupInstanceV2

Call DescribeScalingGroupInstanceV2 to query the details of a running scaling group.

## Debugging

[OpenAPI Explorer automatically calculates the signature value. For your convenience, we recommend that you call this operation in OpenAPI Explorer. OpenAPI Explorer dynamically generates the sample code of the operation for different SDKs.](https://api.aliyun.com/#product=Emr&api=DescribeScalingGroupInstanceV2&type=RPC&version=2016-04-08)

## Request parameters

|Parameter|Type|Required|Example|Description|
|---------|----|--------|-------|-----------|
|Action|String|Required|DescribeScalingGroupInstanceV2|The operation that you want to perform. For API requests using the HTTP or HTTPS URL, this parameter is required. Value: DescribeScalingGroupInstanceV2. |
|RegionId|String|Required|cn-hangzhou|The region ID of the security group to be queried. You can call the [DescribeRegions](~~25609~~) operation to query the most recent region list. |
|ResourceGroupId|String|No|rg-acfmv6jutt6\*\*\*\*|The ID of the resource group. You can call the [ListResourceGroups](~~158855~~) to view the resource group ID. |
|ScalingGroupBizId|String|No|SGB-AA1234567\*\*\*\*|The ID of the scaling group. You can call [ListScalingGroupV2](~~184367~~) to view the scaling group ID. |
|HostGroupBizId|String|No|G-1234567890A\*\*\*\*|The ID of the host group. You can call the [ListClusterHostGroup](~~128432~~)view Machine Group ID. |

## Response parameters

|Parameter|Type|Example|Description|
|---------|----|-------|-----------|
|ActiveRuleCategory|String|ByLoad|The scaling policy. Valid values:

-   ByLoad: scales by load
-   Scheduled |
|DefaultCooldown|Integer|300|The cooldown time. Unit: seconds. |
|HostGroupId|String|G-5011FB3E4928\*\*\*\*|The ID of the host group. |
|MaxSize|Integer|10|Machine Group maximum number of nodes. |
|MinSize|Integer|0|The minimum number of nodes in the Machine Group. |
|MultiAvailablePolicy|String|PRIORITY|Optimization policy. Valid values:

-   PRIORITY: ECS instances are scaled based on the VSwitchIds.N parameter.
-   COST\_OPTIMIZED: ECS instances are created based on the unit price of vCPUs, from low to high. Preemptible instances are preferentially created when preemptible instance types are specified for the scaling configuration.

**Note:** COST\_OPTIMIZED takes effect only when multiple instance types are specified or at least one preemptible instance type is specified. |
|MultiAvailablePolicyParam|String|\{"onDemandBaseCapacity": 1, "onDemandPercentageAboveBaseCapacity": 10, "spotInstanceRemedy": false, "spotInstancePools": 2\}|Optimize the parameters. The following configurations are required only when the MultiAvailablePolicy is COST\_OPTIMIZED:

-   onDemandBaseCapacity: nodes by volume
-   onDemandPercentageAboveBaseCapacity: pay-as-you-go node ratio
-   spotInstanceRemedy: whether to enable pay-as-you-go compensation.
-   spotInstancePools: the number of preemptible node specifications. |
|RequestId|String|6C96FD2C-95A0-4C03-8A19-7D84A4BAAA1E|The ID of the request. |
|ScalingConfig|Struct| |The scaling configuration. |
|DataDiskCategory|String|ssd|The type of the data disk. Values:

-   cloud: basic disk.
-   cloud\_efficiency: the ultra disk
-   cloud\_ssd: local SSD
-   ephemeral\_ssd: local SSD.
-   cloud\_essd: ESSD |
|DataDiskCount|Integer|4|The number of data disks. |
|DataDiskSize|Integer|40|The size of a data disk. Unit: GB. |
|InstanceTypeList|List|ecs.c5.xlarge|The type of the Message Queue for Apache RocketMQ instance. |
|PayType|String|PostPaid|The billing method of the host. Valid values:

-   PostPaid: pay-as-you-go clusters.
-   PrePaid: Subscription |
|SpotPriceLimits|Array of SpotPriceLimit| |Preemptible instance type and price parameters |
|SpotPriceLimit| | | |
|InstanceType|String|ecs.c5.xlarge|The preemptible instance type. |
|PriceLimit|Float|0.1|The unit price of the preemptible instance. Unit: RMB /hour. |
|SpotStrategy|String|NoSpot|The bidding strategy. Valid values:

-   NoSpot: Do not use preemptible instances
-   SpotWithPriceLimit: the use of preemptible instances with price restrictions
-   SpotAsPriceGo: to use preemptible instances at the preemptible price. |
|SysDiskCategory|String|ssd|The type of data disks. Valid values:

-   cloud: basic disk.
-   cloud\_efficiency: the ultra disk
-   cloud\_ssd: standard SSD |
|SysDiskSize|Integer|40|The size of the system disk. Unit: GB. |
|ScalingRuleList|Array of ScalingRule| |The list of scaling rules. |
|ScalingRule| | | |
|AdjustmentType|String|QuantityChangeInCapacity|The incremental mode. Set the value to QuantityChangeInCapacity. |
|AdjustmentValue|Integer|-1|The number of nodes to be adjusted each time. A value greater than 0 indicates scale-out, whereas a value less than 0 indicates scale-in. |
|CloudWatchTrigger|Struct| |Trigger rule parameters by load. |
|ComparisonOperator|String|\>|Comparison operator:

-   \>
-   ≥
-   <
-   ≤
-   = |
|EvaluationCount|String|1|Number of computations. |
|MetricDisplayName|String|YARN.PendingVCores|The display name of the measure. |
|MetricName|String|YarnRootPendingVCores|The name of the measure. |
|Period|Integer|5|The statistical time, in minutes. |
|Statistics|String|Average|The statistical method. |
|Threshold|String|0|The threshold. |
|Unit|String|None|Deprecated field. |
|Cooldown|Integer|600|The cooldown time. Unit: seconds. |
|EssScalingRuleId|String|sgr-34234eed234242|The ID of the associated ESS scaling rule. |
|LaunchExpirationTime|Integer|600|The time period during which the failed scheduled task is retried. Unit: seconds. Valid values: 0 to 21600. |
|LaunchTime|String|2020-07-22T03:09Z|The time at which the scheduled task is triggered. UTC time is used and complies with the ISO 8601 standard. The time must be in UTC. You cannot enter a time point later than 90 days from the scheduled task creation.

If the RecurrenceType parameter is specified, the task is executed each day at the time specified by LaunchTime.

If the RecurrenceType parameter is not specified, the task is executed only once at the time specified by LaunchTime. |
|RecurrenceEndTime|String|2020-07-22T03:09Z|The end time of the scheduled task to be repeated. Specify the time in the ISO 8601 standard in the YYYY-MM-DDThh:mmZ format. |
|RecurrenceType|String|Daily|The interval that a scheduled task is repeated at. Valid values:

-   Daily: The scheduled task is executed once every specified number of days.
-   Weekly: The scheduled task is executed on each specified day of a week.
-   Monthly: The scheduled task is executed on each specified day of a month.

You must specify both RecurrenceType and RecurrenceValue. |
|RecurrenceValue|String|1|The recurrence value of the scheduled task to be repeated.

-   Daily: If you set RecurrenceType to Daily, you can specify only one value. Valid values: 1 to 31.
-   Weekly: If you set RecurrenceType to Weekly, you can specify multiple values and separate them with commas \(,\). For example, the values of Sunday, Monday, Tuesday, Wednesday, Thursday, Friday, and Saturday are 0, 1, 2, 3, 4, 5, and 6.
-   Monthly: If you set RecurrenceType to Monthly, you can specify two values in the A-B format. Valid values: 1 to 31. B must be greater than or equal to A.

You must specify both RecurrenceType and RecurrenceValue. |
|RuleCategory|String|ByLoad|Rule type:

-   ByLoad: scales by load
-   Scheduled |
|RuleName|String|Rule -1|The name of the rule. |
|ScalingGroupId|Long|122|The instance ID of the scaling group. |
|SchedulerTrigger|Struct| |Periodic scheduling rule details |
|LaunchExpirationTime|Integer|600|The time period during which the failed scheduled task is retried. Unit: seconds. Valid values: 0 to 21600. |
|LaunchTime|Long|1434321345621|The time when the scheduled task was triggered. |
|RecurrenceEndTime|Long|1434321345621|The end time after which the scheduled task is no longer repeated. |
|RecurrenceType|String|Daily|The interval that a scheduled task is repeated at. Valid values:

-   Daily: The scheduled task is executed once every specified number of days.
-   Weekly: The scheduled task is executed on each specified day of a week.
-   Monthly: The scheduled task is executed on each specified day of a month.

You must specify both RecurrenceType and RecurrenceValue. |
|RecurrenceValue|String|1|The recurrence value of the scheduled task to be repeated.

-   Daily: If you set RecurrenceType to Daily, you can specify only one value. Valid values: 1 to 31.
-   Weekly: If you set RecurrenceType to Weekly, you can specify multiple values and separate them with commas \(,\). For example, the values of Sunday, Monday, Tuesday, Wednesday, Thursday, Friday, and Saturday are 0, 1, 2, 3, 4, 5, and 6.
-   Monthly: If you set RecurrenceType to Monthly, you can specify two values in the A-B format. Valid values: 1 to 31. B must be greater than or equal to A.

You must specify both RecurrenceType and RecurrenceValue. |
|Status|String|ACTIVE|The running Status. Valid values:

-   ACTIVE: The scaling group is activated.
-   INACTIVE: the scaling group is INACTIVE. |
|TimeoutWithGrace|Long|1000|The timeout period for graceful disconnection. Unit: milliseconds. A reserved field. |
|WithGrace|Boolean|true|Indicates whether to enable graceful disconnection.

-   true
-   false |
|ScalingGroupId|String|SGB-12324546568\*\*\*\*|The ID of the scaling group. |
|TimeoutWithGrace|Long|1000|The timeout period for graceful disconnection. Unit: milliseconds. |
|WithGrace|Boolean|true|Indicates whether to enable graceful disconnection.

-   true
-   false |

## Examples

Sample requests

```
http(s)://[Endpoint]/? Action=DescribeScalingGroupInstanceV2
&RegionId=cn-hangzhou
&<Common request parameters>
```

Sample success responses

`XML` format

```
<ScalingConfig>
    <SpotPriceLimits>
        <SpotPriceLimit>
            <PriceLimit>0.1</PriceLimit>
            <InstanceType>ecs.c5.xlarge</InstanceType>
        </SpotPriceLimit>
    </SpotPriceLimits>
    <DataDiskCount>4</DataDiskCount>
    <PayType>PostPaid</PayType>
    <DataDiskSize>40</DataDiskSize>
    <SysDiskSize>40</SysDiskSize>
    <DataDiskCategory>ssd</DataDiskCategory>
    <SpotStrategy>NoSpot</SpotStrategy>
    <SysDiskCategory>ssd</SysDiskCategory>
    <InstanceTypeList>
        <InstanceType>ecs.c5.xlarge</InstanceType>
    </InstanceTypeList>
</ScalingConfig>
<RequestId>6C96FD2C-95A0-4C03-8A19-7D84A4BAAA1E</RequestId>
<ActiveRuleCategory>ByLoad</ActiveRuleCategory>
<TimeoutWithGrace>1000</TimeoutWithGrace>
<ScalingGroupId>SGB-123245465686****</ScalingGroupId>
<MaxSize>10</MaxSize>
<MultiAvailablePolicyParam>{"onDemandBaseCapacity": 1, "onDemandPercentageAboveBaseCapacity": 10, "spotInstanceRemedy": false, "spotInstancePools": 2}</MultiAvailablePolicyParam>
<MinSize>0</MinSize>
<DefaultCooldown>300</DefaultCooldown>
<HostGroupId>G-5011FB3E49288C19</HostGroupId>
<WithGrace>true</WithGrace>
<MultiAvailablePolicy>PRIORITY</MultiAvailablePolicy>
<ScalingRuleList>
    <ScalingRule>
        <Status>ACTIVE</Status>
        <EssScalingRuleId>sgr-34234eed234242</EssScalingRuleId>
        <LaunchTime>2020-07-22T03:09Z</LaunchTime>
        <TimeoutWithGrace>1000</TimeoutWithGrace>
        <ScalingGroupId>122</ScalingGroupId>
        <Cooldown>600</Cooldown>
        <RecurrenceType>Daily</RecurrenceType>
        <LaunchExpirationTime>600</LaunchExpirationTime>
        <AdjustmentType>QuantityChangeInCapacity</AdjustmentType>
        <AdjustmentValue>-1</AdjustmentValue>
        <WithGrace>true</WithGrace>
        <RecurrenceValue>1</RecurrenceValue>
        <RecurrenceEndTime>2020-07-22T03:09Z</RecurrenceEndTime>
        <RuleName> rule 1 </RuleName>
        <RuleCategory>ByLoad</RuleCategory>
    </ScalingRule>
    <ScalingRule>
        <SchedulerTrigger>
            <ComparisonOperator>></ComparisonOperator>
            <LaunchTime>1434321345621</LaunchTime>
            <RecurrenceType>Daily</RecurrenceType>
            <MetricDisplayName>YARN.PendingVCores</MetricDisplayName>
            <Period>5</Period>
            <EvaluationCount>1</EvaluationCount>
            <Unit> </Unit>
            <Statistics>Average</Statistics>
            <LaunchExpirationTime>600</LaunchExpirationTime>
            <MetricName>YarnRootPendingVCores</MetricName>
            <RecurrenceValue>1</RecurrenceValue>
            <RecurrenceEndTime>1434321345621</RecurrenceEndTime>
            <Threshold>0</Threshold>
        </SchedulerTrigger>
        <CloudWatchTrigger>
            <ComparisonOperator>></ComparisonOperator>
            <LaunchTime>1434321345621</LaunchTime>
            <RecurrenceType>Daily</RecurrenceType>
            <MetricDisplayName>YARN.PendingVCores</MetricDisplayName>
            <Period>5</Period>
            <EvaluationCount>1</EvaluationCount>
            <Unit> </Unit>
            <Statistics>Average</Statistics>
            <LaunchExpirationTime>600</LaunchExpirationTime>
            <MetricName>YarnRootPendingVCores</MetricName>
            <RecurrenceValue>1</RecurrenceValue>
            <RecurrenceEndTime>1434321345621</RecurrenceEndTime>
            <Threshold>0</Threshold>
        </CloudWatchTrigger>
    </ScalingRule>
</ScalingRuleList>
```

`JSON` format

```
{
    "ScalingConfig":
    {
        "SpotPriceLimits":
        {
            "SpotPriceLimit":
            [
                {
                    "PriceLimit":"0.1",
                    "InstanceType":"ecs.c5.xlarge"
                    }
                    ]
                    },
                    "DataDiskCount":"4",
                    "PayType":"PostPaid",
                    "DataDiskSize":"40",
                    "SysDiskSize":"40",
                    "DataDiskCategory":"ssd",
                    "SpotStrategy":"NoSpot",
                    "SysDiskCategory":"ssd",
                    "InstanceTypeList":
                    {
                        "InstanceType":"ecs.c5.xlarge"
                        }
                        },
                        "RequestId":"6C96FD2C-95A0-4C03-8A19-7D84A4BAAA1E",
                        "ActiveRuleCategory":"ByLoad",
                        "TimeoutWithGrace":"1000",
                        "ScalingGroupId":"SGB-123245465686****",
                        "MaxSize":"10",
                        "MultiAvailablePolicyParam":"{\"onDemandBaseCapacity\": 1, \"onDemandPercentageAboveBaseCapacity\": 10, \"spotInstanceRemedy\": false, \"spotInstancePools\": 2}","MinSize":"0","DefaultCooldown":"300","HostGroupId":"G-5011FB3E49288C19",
                        "WithGrace":"true",
                        "MultiAvailablePolicy":"PRIORITY",
                        "ScalingRuleList":
                        {
                            "ScalingRule":
                            [
                                {
                                    "Status":"ACTIVE",
                                    "EssScalingRuleId":"sgr-34234eed234242",
                                    "LaunchTime":"2020-07-22T03:09Z",
                                    "TimeoutWithGrace":"1000",
                                    "ScalingGroupId":"122",
                                    "Cooldown":"600",
                                    "RecurrenceType":"Daily",
                                    "LaunchExpirationTime":"600",
                                    "AdjustmentType":"QuantityChangeInCapacity",
                                    "AdjustmentValue":"-1",
                                    "WithGrace":"true",
                                    "RecurrenceValue":"1",
                                    "RecurrenceEndTime":"2020-07-22T03:09Z",
                                    "RuleName":"Rule 1",
                                    "RuleCategory":"ByLoad"
                                    },
                                    {
                                        "SchedulerTrigger":    {
                                         "ComparisonOperator":">",
                                         "LaunchTime":"1434321345621",
                                         "RecurrenceType":"Daily",
                                         "MetricDisplayName":"YARN.PendingVCores",
                                         "Period":"5",
                                         "EvaluationCount":"1",
                                         "Unit":" ",
                                         "Statistics":"Average",
                                         "LaunchExpirationTime":"600",
                                         "MetricName":"YarnRootPendingVCores",
                                         "RecurrenceValue":"1",
                                         "RecurrenceEndTime":"1434321345621",
                                         "Threshold":"0"
                                         },
                                         "CloudWatchTrigger":
                                         {
                                             "ComparisonOperator":">",
                                             "LaunchTime":"1434321345621",
                                             "RecurrenceType":"Daily",                                            
                                             "MetricDisplayName":"YARN.PendingVCores",
                                             "Period":"5",
                                             "EvaluationCount":"1",
                                             "Unit":" ",
                                             "Statistics":"Average",
                                             "LaunchExpirationTime":"600",
                                             "MetricName":"YarnRootPendingVCores",
                                             "RecurrenceValue":"1",
                                             "RecurrenceEndTime":"1434321345621",
                                             "Threshold":"0"
                                          }
                           }
                       ]
             }
}
```

## Error code

|HttpCode|Error code|Error message|Description|
|--------|----------|-------------|-----------|
|403|Params.Illegal|The specified parameters are wrongly formed..|The error message returned because the format of the specified parameters is invalid.|
|500|InternalError|The request processing has failed due to some unknown error.|The error message returned because the request processing has failed due to an internal error. Submit a ticket.|

For a list of error codes, visit the [API Error Center](https://error-center.alibabacloud.com/status/product/Emr).

