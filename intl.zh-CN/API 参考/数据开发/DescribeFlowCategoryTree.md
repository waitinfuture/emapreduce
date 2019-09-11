# DescribeFlowCategoryTree {#doc_api_Emr_DescribeFlowCategoryTree .reference}

调用 DescribeFlowCategoryTree 接口，获取目录树

## 调试 {#api_explorer .section}

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=Emr&api=DescribeFlowCategoryTree&type=RPC&version=2016-04-08)

## 请求参数 {#parameters .section}

|名称|类型|是否必选|示例值|描述|
|--|--|----|---|--|
|Action|String|是|DescribeFlowCategoryTree|系统规定参数。取值：DescribeFlowCategoryTree。

 |
|ProjectId|String|是|FP-ABD24A6163D3\*\*\*\*|项目ID。

 |
|RegionId|String|是|cn-hangzhou|地域ID。

 |
|Type|String|是|FLOW|目录类型：

 -   FLOW
-   JOB
-   ADHOC

 |

## 返回数据 {#resultMapping .section}

|名称|类型|示例值|描述|
|--|--|---|--|
|Data|String|\{"node":\{"categoryType":"FOLDER","gmtModified":1540344706000,"name":"FLOW","id":"FC-6B5B5BDAD3EFAB67","gmtCreate":1540344706000,"type":"FLOW","projectId":"FP-7A1018ADE9179EE1","parentId":"root\_parent"\},"children":\[\{"node":\{"categoryType":"FILE","gmtModified":1542855766000,"name":"flow2","id":"FC-D30AC9A7795F03A1","gmtCreate":1542855766000,"type":"FLOW","projectId":"FP-7A1018ADE9179EE1","parentId":"FC-6B5B5BDAD3EFAB67","objectId":"F-E9DC5533695C989B","objectType":"FLOW"\},"children":\[\],"childrenMap":\{\}\},\{"node":\{"categoryType":"FILE","gmtModified":1540796206000,"name":"flow-hive","id":"FC-296E3BB9491E39F2","gmtCreate":1540796206000,"type":"FLOW","projectId":"FP-7A1018ADE9179EE1","parentId":"FC-6B5B5BDAD3EFAB67","objectId":"F-35683D0E45734E34","objectType":"FLOW"\},"children":\[\],"childrenMap":\{\}\}\]\}|结果， 由**node**和**children**构成的树状结构。

 |
|RequestId|String|5C5E4A6F-5140-4627-AB81-F3E0D06C5C36|请求ID。

 |

## 示例 {#demo .section}

请求示例

``` {#request_demo}

http(s)://[Endpoint]/?Action=DescribeFlowCategoryTree
&ProjectId=FP-ABD24A6163D3****
&RegionId=cn-hangzhou
&Type=FLOW
&<公共请求参数>

```

正常返回示例

`XML` 格式

``` {#xml_return_success_demo}
<DescribeFlowCategoryTreeResponse>
	  <data>
		    <node>
			      <categoryType>FOLDER</categoryType>
			      <gmtModified>1540344706000</gmtModified>
			      <name>FLOW</name>
			      <id>FC-6B5B5BDAD3EF****</id>
			      <gmtCreate>1540344706000</gmtCreate>
			      <type>FLOW</type>
			      <projectId>FP-7A1018ADE917****</projectId>
			      <parentId>root_parent</parentId>
		    </node>
		    <children>
			      <node>
				        <categoryType>FILE</categoryType>
				        <gmtModified>1542855766000</gmtModified>
				        <name>flow2</name>
				        <id>FC-D30AC9A7795F****</id>
				        <gmtCreate>1542855766000</gmtCreate>
				        <type>FLOW</type>
				        <projectId>FP-7A1018ADE917****</projectId>
				        <parentId>FC-6B5B5BDAD3EF****</parentId>
				        <objectId>F-E9DC5533695C****</objectId>
				        <objectType>FLOW</objectType>
			      </node>
			      <childrenMap></childrenMap>
		    </children>
		    <children>
			      <node>
				        <categoryType>FILE</categoryType>
				        <gmtModified>1540796206000</gmtModified>
				        <name>flow-hive</name>
				        <id>FC-296E3BB9491E****</id>
				        <gmtCreate>1540796206000</gmtCreate>
				        <type>FLOW</type>
				        <projectId>FP-7A1018ADE917****</projectId>
				        <parentId>FC-6B5B5BDAD3EF****</parentId>
				        <objectId>F-35683D0E4573****</objectId>
				        <objectType>FLOW</objectType>
			      </node>
			      <childrenMap></childrenMap>
		    </children>
		    <childrenMap>
			      <FC-D30AC9A7795F03A1>
				        <node>
					          <categoryType>FILE</categoryType>
					          <gmtModified>1542855766000</gmtModified>
					          <name>flow2</name>
					          <id>FC-D30AC9A7795F****</id>
					          <gmtCreate>1542855766000</gmtCreate>
					          <type>FLOW</type>
					          <projectId>FP-7A1018ADE917****</projectId>
					          <parentId>FC-6B5B5BDAD3EF****</parentId>
					          <objectId>F-E9DC5533695C****</objectId>
					          <objectType>FLOW</objectType>
				        </node>
				        <childrenMap></childrenMap>
			      </FC-D30AC9A7795F03A1>
			      <FC-296E3BB9491E39F2>
				        <node>
					          <categoryType>FILE</categoryType>
					          <gmtModified>1540796206000</gmtModified>
					          <name>flow-hive</name>
					          <id>FC-296E3BB9491E****</id>
					          <gmtCreate>1540796206000</gmtCreate>
					          <type>FLOW</type>
					          <projectId>FP-7A1018ADE917****</projectId>
					          <parentId>FC-6B5B5BDAD3EF****</parentId>
					          <objectId>F-35683D0E4573****</objectId>
					          <objectType>FLOW</objectType>
				        </node>
				        <childrenMap></childrenMap>
			      </FC-296E3BB9491E39F2>
		    </childrenMap>
	  </data>
	  <requestId>B12F5D42-52C2-47F2-8A2F-4D0D68E64E44</requestId>
</DescribeFlowCategoryTreeResponse>
```

`JSON` 格式

``` {#json_return_success_demo}
{
	"requestId":"B12F5D42-52C2-47F2-8A2F-4D0D68E64E44",
	"data":{
		"node":{
			"id":"FC-6B5B5BDAD3EF****",
			"gmtModified":1540344706000,
			"parentId":"root_parent",
			"gmtCreate":1540344706000,
			"name":"FLOW",
			"categoryType":"FOLDER",
			"projectId":"FP-7A1018ADE917****",
			"type":"FLOW"
		},
		"children":[
			{
				"node":{
					"id":"FC-D30AC9A7795F****",
					"gmtModified":1542855766000,
					"parentId":"FC-6B5B5BDAD3EF****",
					"gmtCreate":1542855766000,
					"objectId":"F-E9DC5533695C****",
					"name":"flow2",
					"categoryType":"FILE",
					"projectId":"FP-7A1018ADE917****",
					"type":"FLOW",
					"objectType":"FLOW"
				},
				"children":[],
				"childrenMap":{}
			},
			{
				"node":{
					"id":"FC-296E3BB9491E****",
					"gmtModified":1540796206000,
					"parentId":"FC-6B5B5BDAD3EF****",
					"gmtCreate":1540796206000,
					"objectId":"F-35683D0E4573****",
					"name":"flow-hive",
					"categoryType":"FILE",
					"projectId":"FP-7A1018ADE917****",
					"type":"FLOW",
					"objectType":"FLOW"
				},
				"children":[],
				"childrenMap":{}
			}
		],
		"childrenMap":{
			"FC-296E3BB9491E39F2":{
				"node":{
					"id":"FC-296E3BB9491E****",
					"gmtModified":1540796206000,
					"parentId":"FC-6B5B5BDAD3EF****",
					"gmtCreate":1540796206000,
					"objectId":"F-35683D0E4573****",
					"name":"flow-hive",
					"categoryType":"FILE",
					"projectId":"FP-7A1018ADE917****",
					"type":"FLOW",
					"objectType":"FLOW"
				},
				"children":[],
				"childrenMap":{}
			},
			"FC-D30AC9A7795F03A1":{
				"node":{
					"id":"FC-D30AC9A7795F****",
					"gmtModified":1542855766000,
					"parentId":"FC-6B5B5BDAD3EF****",
					"gmtCreate":1542855766000,
					"objectId":"F-E9DC5533695C****",
					"name":"flow2",
					"categoryType":"FILE",
					"projectId":"FP-7A1018ADE917****",
					"type":"FLOW",
					"objectType":"FLOW"
				},
				"children":[],
				"childrenMap":{}
			}
		}
	}
}
```

## 错误码 { .section}

访问[错误中心](https://error-center.alibabacloud.com/status/product/Emr)查看更多错误码。

