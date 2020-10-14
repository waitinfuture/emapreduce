# DescribeScalingGroupInstanceV2

调用DescribeScalingGroupInstanceV2接口，获取一个正在运行中的伸缩组实例详情。

## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=Emr&api=DescribeScalingGroupInstanceV2&type=RPC&version=2016-04-08)

## 请求参数

|名称|类型|是否必选|示例值|描述|
|--|--|----|---|--|
|Action|String|是|DescribeScalingGroupInstanceV2|系统规定参数。对于您自行拼凑HTTP或HTTPS URL发起的API请求，该参数为必选参数。取值：DescribeScalingGroupInstanceV2。 |
|RegionId|String|是|cn-hangzhou|地域ID。您可以调用[DescribeRegions](~~25609~~)查看最新的阿里云地域列表。 |
|ResourceGroupId|String|否|rg-acfmv6jutt6\*\*\*\*|资源组ID。你可以调用[ListResourceGroups](~~158855~~)查看资源组ID。 |
|ScalingGroupBizId|String|否|SGB-AA1234567\*\*\*\*|伸缩组ID。你可以调用[ListScalingGroupV2](~~184367~~)查看伸缩组ID。 |
|HostGroupBizId|String|否|G-1234567890A\*\*\*\*|机器组ID。你可以调用[ListClusterHostGroup](~~128432~~)查看机器组ID。 |

## 返回数据

|名称|类型|示例值|描述|
|--|--|---|--|
|ActiveRuleCategory|String|ByLoad|伸缩策略。取值如下：

 -   ByLoad：按负载伸缩
-   Scheduled：定时伸缩 |
|DefaultCooldown|Integer|300|冷却时间，单位秒。 |
|HostGroupId|String|G-5011FB3E4928\*\*\*\*|机器组ID。 |
|MaxSize|Integer|10|机器组最大节点数。 |
|MinSize|Integer|0|机器组最小节点数。 |
|MultiAvailablePolicy|String|PRIORITY|优化策略。取值如下：

 -   PRIORITY：根据您定义的虚拟交换机（VSwitchIds.N）扩缩容。
-   COST\_OPTIMIZED：按vCPU单价从低到高进行尝试创建。当伸缩配置设置了抢占式计费方式的多实例规格时，优先创建对应抢占式实例。

 **说明：** COST\_OPTIMIZED仅在伸缩配置设置了多实例规格或者选用了抢占式实例的情况下生效。 |
|MultiAvailablePolicyParam|String|\{"onDemandBaseCapacity": 1, "onDemandPercentageAboveBaseCapacity": 10, "spotInstanceRemedy": false, "spotInstancePools": 2\}|优化参数。仅当MultiAvailablePolicy为COST\_OPTIMIZED时需要设置如下：

 -   onDemandBaseCapacity:：按量节点数
-   onDemandPercentageAboveBaseCapacity：按量节点比例
-   spotInstanceRemedy：是否开启按量补偿
-   spotInstancePools：抢占节点规格类型个数 |
|RequestId|String|6C96FD2C-95A0-4C03-8A19-7D84A4BAAA1E|请求ID。 |
|ScalingConfig|Struct| |伸缩配置。 |
|DataDiskCategory|String|ssd|数据盘类型，取值如下：

 -   cloud：普通云盘
-   cloud\_efficiency：高效云盘
-   cloud\_ssd：本地SSD盘
-   ephemeral\_ssd：本地SSD盘
-   cloud\_essd：ESSD云盘 |
|DataDiskCount|Integer|4|数据盘个数。 |
|DataDiskSize|Integer|40|数据盘大小，单位GB。 |
|InstanceTypeList|List|ecs.c5.xlarge|实例类型。 |
|PayType|String|PostPaid|付费类型。取值如下：

 -   PostPaid：按量付费集群
-   PrePaid：包年包月集群 |
|SpotPriceLimits|Array of SpotPriceLimit| |抢占式实例规格和价格参数 |
|SpotPriceLimit| | | |
|InstanceType|String|ecs.c5.xlarge|抢占实例规格类型。 |
|PriceLimit|Float|0.1|抢占式实例单价，单位元/小时。 |
|SpotStrategy|String|NoSpot|竞价策略。取值如下：

 -   NoSpot：不使用抢占式实例
-   SpotWithPriceLimit：有价格限制的使用抢占式实例
-   SpotAsPriceGo：按价格使用抢占式实例 |
|SysDiskCategory|String|ssd|系统盘类型。取值如下：

 -   cloud：普通云盘
-   cloud\_efficiency：高效云盘
-   cloud\_ssd：SSD云盘 |
|SysDiskSize|Integer|40|系统盘大小，单位GB。 |
|ScalingRuleList|Array of ScalingRule| |伸缩规则列表。 |
|ScalingRule| | | |
|AdjustmentType|String|QuantityChangeInCapacity|增量模式，固定为QuantityChangeInCapacity。 |
|AdjustmentValue|Integer|-1|每次调整的节点个数，大于0为扩容，小于0为缩容。 |
|CloudWatchTrigger|Struct| |按负载触发规则参数。 |
|ComparisonOperator|String|\>|比较符：

 -   ＞
-   ≥
-   ＜
-   ≤
-   ＝ |
|EvaluationCount|String|1|计算次数。 |
|MetricDisplayName|String|YARN.PendingVCores|度量显示名称。 |
|MetricName|String|YarnRootPendingVCores|度量名称。 |
|Period|Integer|5|统计时间，单位分钟。 |
|Statistics|String|Average|统计方式。 |
|Threshold|String|0|阈值。 |
|Unit|String|None|废弃字段。 |
|Cooldown|Integer|600|冷却时间，单位秒。 |
|EssScalingRuleId|String|sgr-34234eed234242|关联的ESS伸缩规则ID。 |
|LaunchExpirationTime|Integer|600|定时任务触发操作失败后，在此时间内重试。单位为秒，取值范围：0~21600。 |
|LaunchTime|String|2020-07-22T03:09Z|定时任务触发的时间点。 按照ISO8601标准表示，并需要使用UTC时间。格式为：YYYY-MM-DDThh:mmZ。不能填写自创建当天起90日后的时间。

 如果指定了RecurrenceType，则此属性指定的时间点，默认为循环执行的时间点。

 如果未指定RecurrenceType，则按指定的日期和时间执行一次。 |
|RecurrenceEndTime|String|2020-07-22T03:09Z|重复执行定时任务的结束时间。按照ISO8601标准表示，并需要使用UTC时间。 |
|RecurrenceType|String|Daily|重复执行定时任务的类型，取值范围：

 -   Daily：每多少天重复执行一次定时任务。
-   Weekly：每周指定几天重复执行一次定时任务。
-   Monthly：每月内指定几天重复执行一次定时任务。

 您需要同时指定RecurrenceType和RecurrenceValue。 |
|RecurrenceValue|String|1|重复执行定时任务的数值。

 -   RecurrenceType取Daily时，只能填一个值，取值范围：1~31。
-   RecurrenceType取Weekly时，可以填入多个值，填多个值时使用英文逗号（,）分隔。比如，周日、周一、周二、周三、周四、周五、周六的值依次为：0,1,2,3,4,5,6。
-   RecurrenceType取Monthly时，格式为A-B。A、B的取值范围为1~31，并且B必须大于等于A。

 您需要同时指定RecurrenceType和RecurrenceValue。 |
|RuleCategory|String|ByLoad|规则类型:

 -   ByLoad：按负载伸缩
-   Scheduled：定时伸缩 |
|RuleName|String|规则1|规则名称。 |
|ScalingGroupId|Long|122|伸缩组实例ID。 |
|SchedulerTrigger|Struct| |周期调度规则详情 |
|LaunchExpirationTime|Integer|600|定时任务触发操作失败后，在此时间内重试。单位为秒，取值范围：0~21600。 |
|LaunchTime|Long|1434321345621|定时任务触发的时间点。 |
|RecurrenceEndTime|Long|1434321345621|重复执行定时任务的结束时间。 |
|RecurrenceType|String|Daily|重复执行定时任务的类型，取值范围：

 -   Daily：每多少天重复执行一次定时任务。
-   Weekly：每周指定几天重复执行一次定时任务。
-   Monthly：每月内指定几天重复执行一次定时任务。

 您需要同时指定RecurrenceType和RecurrenceValue。 |
|RecurrenceValue|String|1|重复执行定时任务的数值。

 -   RecurrenceType取Daily时，只能填一个值，取值范围：1~31。
-   RecurrenceType取Weekly时，可以填入多个值，填多个值时使用英文逗号（,）分隔。比如，周日、周一、周二、周三、周四、周五、周六的值依次为：0,1,2,3,4,5,6。
-   RecurrenceType取Monthly时，格式为A-B。A、B的取值范围为1~31，并且B必须大于等于A。

 您需要同时指定RecurrenceType和RecurrenceValue。 |
|Status|String|ACTIVE|运行状态，取值范围：

 -   ACTIVE：伸缩组已启动
-   INACTIVE：伸缩组未启动 |
|TimeoutWithGrace|Long|1000|优雅下线超时时间，单位毫秒。保留字段。 |
|WithGrace|Boolean|true|是否开启优雅下线：

 -   true：是
-   false：否 |
|ScalingGroupId|String|SGB-12324546568\*\*\*\*|伸缩组ID。 |
|TimeoutWithGrace|Long|1000|优雅下线超时时间，单位毫秒。 |
|WithGrace|Boolean|true|是否开启优雅下线：

 -   true：是
-   false：否 |

## 示例

请求示例

```
http(s)://[Endpoint]/?Action=DescribeScalingGroupInstanceV2
&RegionId=cn-hangzhou
&<公共请求参数>
```

正常返回示例

`XML` 格式

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
        <RuleName>规则1</RuleName>
        <RuleCategory>ByLoad</RuleCategory>
    </ScalingRule>
    <ScalingRule>
        <SchedulerTrigger>
            <ComparisonOperator>&gt;</ComparisonOperator>
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
            <ComparisonOperator>&gt;</ComparisonOperator>
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

`JSON` 格式

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
                                    "RuleName":"规则1",
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

## 错误码

|HttpCode|错误码|错误信息|描述|
|--------|---|----|--|
|403|Params.Illegal|The specified parameters are wrongly formed..|指定参数格式错误|
|500|InternalError|The request processing has failed due to some unknown error.|内部错误，请提工单|

访问[错误中心](https://error-center.alibabacloud.com/status/product/Emr)查看更多错误码。

