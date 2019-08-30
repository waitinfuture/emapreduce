# ModifyFlowProjectClusterSetting {#doc_api_Emr_ModifyFlowProjectClusterSetting .reference}

调用ModifyFlowProjectClusterSetting接口修改项目集群设置。

## 调试 {#api_explorer .section}

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=Emr&api=ModifyFlowProjectClusterSetting&type=RPC&version=2016-04-08)

## 请求参数 {#parameters .section}

|名称|类型|是否必选|示例值|描述|
|--|--|----|---|--|
|Action|String|是|ModifyFlowProjectClusterSetting|系统规定参数。取值：ModifyFlowProjectClusterSetting。

 |
|ClusterId|String|是|C-FDB726F71863\*\*\*\*|关联集群ID。

 |
|ProjectId|String|是|FP-179332E88F52\*\*\*\*|所属项目ID。

 |
|RegionId|String|是|cn-hangzhou|地域ID。

 |
|DefaultQueue|String|否|default|默认提交队列。

 |
|DefaultUser|String|否|hadoop|默认提交用户。

 |
|HostList.N|RepeatList|否|emr-header-1|关联主机列表。

 |
|QueueList.N|RepeatList|否|queue1|队列列表。

 |
|UserList.N|RepeatList|否|user1|用户列表。

 |

## 返回数据 {#resultMapping .section}

|名称|类型|示例值|描述|
|--|--|---|--|
|Data|Boolean|true|修改结果，**true**表示修改成功，**false**表示修改失败。

 |
|RequestId|String|5ECD6EA1-838E-4BDF-96C8-AEAA40F04F48|请求ID。

 |

## 示例 {#demo .section}

请求示例

``` {#request_demo}

http(s)://[Endpoint]/?Action=ModifyFlowProjectClusterSetting
&ClusterId=C-FDB726F71863****
&ProjectId=FP-179332E88F52****
&RegionId=cn-hangzhou
&<公共请求参数>

```

正常返回示例

`XML` 格式

``` {#xml_return_success_demo}
<ModifyFlowProjectClusterSettingResponse>
  <code>200</code>
	  <data>
		    <RequestId>5ECD6EA1-838E-4BDF-96C8-AEAA40F04F48</RequestId>
		    <Data>true</Data>
	  </data>
	  <requestId>5ECD6EA1-838E-4BDF-96C8-AEAA40F04F48</requestId>
	  <successResponse>true</successResponse>
</ModifyFlowProjectClusterSettingResponse>
```

`JSON` 格式

``` {#json_return_success_demo}
{
	"successResponse":true,
	"requestId":"5ECD6EA1-838E-4BDF-96C8-AEAA40F04F48",
	"data":{
		"Data":true,
		"RequestId":"5ECD6EA1-838E-4BDF-96C8-AEAA40F04F48"
	},
	"code":"200"
}
```

## 错误码 { .section}

访问[错误中心](https://error-center.alibabacloud.com/status/product/Emr)查看更多错误码。

