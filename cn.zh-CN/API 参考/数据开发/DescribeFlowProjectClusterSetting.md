# DescribeFlowProjectClusterSetting {#doc_api_Emr_DescribeFlowProjectClusterSetting .reference}

调用 DescribeFlowProjectClusterSetting 接口，查询项目集群设置详情

## 调试 {#api_explorer .section}

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=Emr&api=DescribeFlowProjectClusterSetting&type=RPC&version=2016-04-08)

## 请求参数 {#parameters .section}

|名称|类型|是否必选|示例值|描述|
|--|--|----|---|--|
|Action|String|是|DescribeFlowProjectClusterSetting|系统规定参数。取值：DescribeFlowProjectClusterSetting。

 |
|ClusterId|String|是|C-DCEE11B49C8F\*\*\*\*|集群ID。

 |
|ProjectId|String|是|FP-ED2F3E844FE3\*\*\*\*|项目ID。

 |
|RegionId|String|是|cn-hangzhou|地域ID。

 |

## 返回数据 {#resultMapping .section}

|名称|类型|示例值|描述|
|--|--|---|--|
|ClusterId|String|C-DCEE11B49C8F\*\*\*\*|集群ID。

 |
|DefaultQueue|String|default|默认提交YARN队列。

 |
|DefaultUser|String|hadoop|默认提交的Linux用户。

 |
|GmtCreate|Long|1541561123000|创建时间。

 |
|GmtModified|Long|1541561123000|修改时间。

 |
|HostList| |"Host":\["emr-header-1.cluster-500159692"|客户端白名单列表。

 |
|Host| | |客户端白名单列表。

 |
|ProjectId|String|FP-3535FE0BE522\*\*\*\*|项目ID。

 |
|QueueList| |"Queue": \["default"\]|YARN队列白名单列表。

 |
|Queue| | |YARN队列白名单列表。

 |
|RequestId|String|F2168FB7-8E60-44EE-A946-DA887297\*\*\*\*|请求ID。

 |
|UserList| |"User": \["hadoop"\]|Linux提交用户白名单列表。

 |
|User| | |Linux提交用户白名单列表。

 |

## 示例 {#demo .section}

请求示例

``` {#request_demo}

http(s)://[Endpoint]/?Action=DescribeFlowProjectClusterSetting
&ClusterId=C-DCEE11B49C8F****
&ProjectId=FP-ED2F3E844FE3****
&RegionId=cn-hangzhou
&<公共请求参数>

```

正常返回示例

`XML` 格式

``` {#xml_return_success_demo}
<DescribeFlowProjectClusterSettingResponse>
	  <data>
		    <DefaultUser>hadoop</DefaultUser>
		    <GmtCreate>1541561123000</GmtCreate>
		    <RequestId>80F270E8-27BD-4F24-BB2A-CD3FBCC450DA</RequestId>
		    <DefaultQueue>undefined</DefaultQueue>
		    <ClusterId>C-F32FB31D8295****</ClusterId>
		    <ProjectId>FP-3535FE0BE5224A47</ProjectId>
		    <GmtModified>1542948908000</GmtModified>
		    <HostList>
			      <Host>emr-header-1.cluster-500159692</Host>
		    </HostList>
		    <UserList></UserList>
		    <QueueList></QueueList>
	  </data>
	  <requestId>80F270E8-27BD-4F24-BB2A-CD3FBCC450DA</requestId>
</DescribeFlowProjectClusterSettingResponse>
```

`JSON` 格式

``` {#json_return_success_demo}
{
	"requestId":"80F270E8-27BD-4F24-BB2A-CD3FBCC450DA",
	"data":{
		"DefaultUser":"hadoop",
		"UserList":{
			"User":[]
		},
		"ClusterId":"C-F32FB31D8295****",
		"RequestId":"80F270E8-27BD-4F24-BB2A-CD3FBCC450DA",
		"DefaultQueue":"undefined",
		"QueueList":{
			"Queue":[]
		},
		"GmtCreate":1541561123000,
		"HostList":{
			"Host":[
				"emr-header-1.cluster-500159692"
			]
		},
		"GmtModified":1542948908000,
		"ProjectId":"FP-3535FE0BE5224A47"
	}
}
```

## 错误码 { .section}

访问[错误中心](https://error-center.aliyun.com/status/product/Emr)查看更多错误码。

