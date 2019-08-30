# ListClusterHostGroup {#doc_api_Emr_ListClusterHostGroup .reference}

调用ListClusterHostGroup接口查询集群机器组列表。

查询集群机器组列表

## 调试 {#api_explorer .section}

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=Emr&api=ListClusterHostGroup&type=RPC&version=2016-04-08)

## 请求参数 {#parameters .section}

|名称|类型|是否必选|示例值|描述|
|--|--|----|---|--|
|Action|String|是|ListClusterHostGroup|系统规定参数。取值：ListClusterHostGroup。

 |
|ClusterId|String|是|C-D7958B72E59B\*\*\*\*|集群ID。

 |
|RegionId|String|是|cn-hangzhou|区域ID。

 |
|AccessKeyId|String|否|LTAI8ljWyu7y\*\*\*\*|阿里云AccessKey ID，用于标识访问者身份。

 |
|HostGroupId|String|否|G-76D09CF110A3\*\*\*\*|机器组ID。

 |
|HostGroupName|String|否|core\_group|待查询的机器组名称。

 |
|HostGroupType|String|否|CORE|待查询的机器组类型，枚举值：**MASTER**、**CORE**和**TASK**。

 |
|PageNumber|Integer|否|1|分页查询的页码。

 |
|PageSize|Integer|否|10|分页查询的每页数据量。

 |
|StatusList.N|RepeatList|否|NORMAL|待查询的机器组状态列表，枚举值：**NORMAL**、**ABNORMAL**和**DELETED**。

 |

## 返回数据 {#resultMapping .section}

|名称|类型|示例值|描述|
|--|--|---|--|
|ClusterId|String|C-D7958B72E59B\*\*\*\*|集群ID。

 |
|HostGroupList| | |集群的机器组信息列表。

 |
|ChargeType|String|PostPaid|当前机器组的付费类型。

 |
|Comment|String|备注信息|机器组的备注信息。

 |
|Cpu|Integer|4|机器组中节点的CPU规格。

 |
|DataDiskCount|Integer|4|机器组中节点的数据盘块数。

 |
|DataDiskSize|Integer|100|机器组中节点的单块数据盘大小，单位GB。

 |
|DataDiskType|String|CLOUD\_SSD|机器组中节点的数据盘类型。

 |
|HostGroupChangeStatus|String|IN\_PROGRESS|升配任务的执行状态，枚举值：**IN\_PROGRESS**、**COMPLETED**和**FAILED**。

 |
|HostGroupChangeType|String|RESIZE\_DISK|机器组当前的操作类型，枚举值：**RESIZE\_DISK**、**MODIFY\_INSTANCE\_TYPE**和**RESTART\_HOST\_GROUP**。

 |
|HostGroupId|String|G-EBAA5D6566E76F55|机器组ID。

 |
|HostGroupName|String|Core\_Group|机器组名称。

 |
|HostGroupSubType|String|AutoScaling|机器组的子类型，例如**AutoScaling**。

 |
|HostGroupType|String|CORE|机器组类型，枚举值：**MASTER**、**CORE**和**TASK**。

 |
|InstanceType|String|ecs.c5.xlarge|机器组的节点规格。

 |
|LockReason|String|Your account balance is insufficient|锁定原因。

 |
|LockType|String|ACCOUNTARREARS|机器组的锁定类型，枚举值：**CLUSTER\_EXPIRED**（集群过期）和**ACCOUNT\_ARREARS**（账号欠费）。

 |
|Memory|Integer|8|机器组中节点的内存规格。

 |
|NodeCount|Integer|3|机器组中节点的数量。

 |
|PayType|String|PostPaid|机器组的付费类型信息。

 |
|SecurityGroupId|String|sg-xxx|安全组ID。

 |
|Status|String|NORMAL|机器组状态。

 |
|SystemDiskCount|Integer|1|机器组中节点的系统盘的块数。

 |
|SystemDiskSize|Integer|40|机器组中节点的单块系统盘的大小，单位GB。

 |
|SystemDiskType|String|CLOUD\_SSD|机器组中节点的系统盘类型。

 |
|VswitchId|String|vsw-bp161s00cryyb4bs1\*\*\*\*|交换机ID。

 |
|gmtCreate|String|1564472329000|创建时间。

 |
|gmtModified|String|1564472889000|更新时间。

 |
|PageNumber|Integer|1|分页查询的页数。

 |
|PageSize|Integer|10|分页查询的每页数据量。

 |
|RequestId|String|BF4FBAC6-B03E-4BFB-B6DB-EB53C34F2E22|请求ID。

 |
|Total|Integer|15|查询总数。

 |

## 示例 {#demo .section}

请求示例

``` {#request_demo}

http(s)://[Endpoint]/?Action=ListClusterHostGroup
&ClusterId=C-D7958B72E59B****
&RegionId=cn-hangzhou
&<公共请求参数>

```

正常返回示例

`XML` 格式

``` {#xml_return_success_demo}
<ListClusterHostGroupResponse>
	  <code>200</code>
	  <data>
		    <hostGroupList>
			      <chargeType>PostPaid</chargeType>
			      <cpu>4</cpu>
			      <dataDiskCount>4</dataDiskCount>
			      <dataDiskSize>100</dataDiskSize>
			      <dataDiskType>CLOUD_SSD</dataDiskType>
			      <gmtCreate>1564472329000</gmtCreate>
			      <gmtModified>1564472889000</gmtModified>
			      <hostGroupId>G-EBAA5D6566E7****</hostGroupId>
			      <hostGroupName>Core_Group</hostGroupName>
			      <hostGroupType>CORE</hostGroupType>
			      <instanceType>ecs.c5.xlarge</instanceType>
			      <memory>8</memory>
			      <nodeCount>3</nodeCount>
			      <payType>PostPaid</payType>
			      <status>NORMAL</status>
			      <systemDiskSize>200</systemDiskSize>
			      <systemDiskType>CLOUD_SSD</systemDiskType>
			      <vswitchId>vsw-bp161s00cryyb4bs1****</vswitchId>
		    </hostGroupList>
		    <hostGroupList>
			      <chargeType>PostPaid</chargeType>
			      <cpu>4</cpu>
			      <dataDiskCount>3</dataDiskCount>
			      <dataDiskSize>500</dataDiskSize>
			      <dataDiskType>CLOUD_SSD</dataDiskType>
			      <gmtCreate>1564472329000</gmtCreate>
			      <gmtModified>1564472889000</gmtModified>
			      <hostGroupId>G-3379C5DEBA14****</hostGroupId>
			      <hostGroupName>Master_Group</hostGroupName>
			      <hostGroupType>MASTER</hostGroupType>
			      <instanceType>ecs.g5.xlarge</instanceType>
			      <memory>16</memory>
			      <nodeCount>3</nodeCount>
			      <payType>PostPaid</payType>
			      <status>NORMAL</status>
			      <systemDiskSize>200</systemDiskSize>
			      <systemDiskType>CLOUD_SSD</systemDiskType>
			      <vswitchId>vsw-bp161s00cryyb4bs1****</vswitchId>
		    </hostGroupList>
		    <pageNumber>1</pageNumber>
		    <pageSize>500</pageSize>
		    <requestId>9E395024-CE21-42F7-A777-69DD7AFFBE1F</requestId>
	  </data>
	  <success>true</success>
</ListClusterHostGroupResponse>
```

`JSON` 格式

``` {#json_return_success_demo}
{
	"data":{
		"hostGroupList":[
			{
				"gmtModified":"1564472889000",
				"gmtCreate":"1564472329000",
				"status":"NORMAL",
				"dataDiskType":"CLOUD_SSD",
				"cpu":4,
				"dataDiskCount":4,
				"hostGroupId":"G-EBAA5D6566E7****",
				"hostGroupName":"Core_Group",
				"dataDiskSize":100,
				"memory":8,
				"instanceType":"ecs.c5.xlarge",
				"systemDiskSize":200,
				"systemDiskType":"CLOUD_SSD",
				"payType":"PostPaid",
				"vswitchId":"vsw-bp161s00cryyb4bs1****",
				"chargeType":"PostPaid",
				"nodeCount":3,
				"hostGroupType":"CORE"
			},
			{
				"gmtModified":"1564472889000",
				"gmtCreate":"1564472329000",
				"status":"NORMAL",
				"dataDiskType":"CLOUD_SSD",
				"cpu":4,
				"dataDiskCount":3,
				"hostGroupId":"G-3379C5DEBA14****",
				"hostGroupName":"Master_Group",
				"dataDiskSize":500,
				"memory":16,
				"instanceType":"ecs.g5.xlarge",
				"systemDiskSize":200,
				"systemDiskType":"CLOUD_SSD",
				"payType":"PostPaid",
				"vswitchId":"vsw-bp161s00cryyb4bs1****",
				"chargeType":"PostPaid",
				"nodeCount":3,
				"hostGroupType":"MASTER"
			}
		],
		"requestId":"9E395024-CE21-42F7-A777-69DD7AFFBE1F",
		"pageSize":500,
		"pageNumber":1
	},
	"code":"200",
	"success":true
}
```

## 错误码 { .section}

访问[错误中心](https://error-center.alibabacloud.com/status/product/Emr)查看更多错误码。

