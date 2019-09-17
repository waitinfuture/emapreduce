# ListEmrAvailableConfig {#doc_api_Emr_ListEmrAvailableConfig .reference}

调用 ListEmrAvailableConfig 接口，获取可用的 EMR 集群创建配置。

## 调试 {#api_explorer .section}

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=Emr&api=ListEmrAvailableConfig&type=RPC&version=2016-04-08)

## 请求参数 {#parameters .section}

|名称|类型|是否必选|示例值|描述|
|--|--|----|---|--|
|Action|String|是|ListEmrAvailableConfig|系统规定参数。取值：ListEmrAvailableConfig。

 |
|RegionId|String|是|cn-hangzhou|地域ID。

 |
|AccessKeyId|String|否|LTAI8ljWyu7y\*\*\*\*|阿里云AccessKey ID信息，用于标识访问者身份。

 |

## 返回数据 {#resultMapping .section}

|名称|类型|示例值|描述|
|--|--|---|--|
|EmrMainVersionList| | |EMR版本列表。

 |
|EmrMainVersion| | |EMR版本列表。

 |
|ClusterTypeInfoList| | |集群类型列表。

 |
|ClusterTypeInfo| | |集群类型列表。

 |
|ClusterServiceInfoList| | |集群服务列表。

 |
|ClusterServiceInfo| | |集群服务列表。

 |
|Mandatory|Boolean|true|是否必选。

 |
|ServiceDisplayName|String|Hive|服务显示名。

 |
|ServiceName|String|HIVE|服务名。

 |
|ServiceVersion|String|2.3.3|服务版本。

 |
|ClusterType|String|HADOOP|集群类型。

 |
|EcmVersion|Boolean|true|保留字段。

 |
|MainVersionName|String|EMR-3.15.1|主版本。

 |
|RegionId|String|cn-hangzhou|地域ID。

 |
|StackName|String|EMR|EMR软件栈名称。

 |
|StackVersion|String|EMR-3.15.1|EMR软件栈版本。

 |
|KeyPairNameList| |\["key\_pair"\]|密钥对列表。

 |
|KeyPairName| | |密钥对列表。

 |
|RequestId|String|BF4FBAC6-B03E-4BFB-B6DB-EB53C34F2E22|请求ID。

 |
|SecurityGroupList| | |安全组列表。

 |
|SecurityGroup| | |安全组列表。

 |
|AvailableInstanceAmount|Integer|10|可用实例数。

 |
|CreationTime|String|2018-12-03T10:11:55Z|创建时间。

 |
|Description|String|sgdesc|安全组描述。

 |
|EcsCount|Integer|10|ECS数量。

 |
|SecurityGroupId|String|sg-bp1j1n0xcwfs19y98b7n|安全组ID。

 |
|SecurityGroupName|String|ziguansg|安全组名。

 |
|VpcId|String|vpc-bp1d618azoa9go6wowjl2|VPC ID。

 |
|VpcInfoList| | |VPC列表。

 |
|VpcInfo| | |VPC列表。

 |
|CidrBlock|String|192.168.0.0/16|CIDR设置。

 |
|CreationTime|String|2018-11-22T07:38:44Z|创建时间。

 |
|Description|String|vpc\_desc|VPC描述。

 |
|VRouterId|String|0|保留字段。

 |
|VpcId|String|vpc-bp1d618azoa9go6wowjl2|VPC ID。

 |
|VpcName|String|zgtest|VPC名字。

 |
|VswitchInfoList| | |虚拟交换机信息。

 |
|VswitchInfo| | |虚拟交换机信息。

 |
|AvailableIpAddressCount|Long|200|可用IP地址数量

 |
|CidrBlock|String|192.168.0.0/24|CIDR 设置

 |
|CreationTime|String|2018-11-22T07:38:49Z|创建时间

 |
|Description|String|switch desc|描述

 |
|VpcId|String|vpc-bp1d618azoa9go6wowjl2|VPC ID

 |
|VswitchId|String|vsw-bp18amcazibt1u0d8qqqa|虚拟交换机 ID

 |
|VswitchName|String|hangzhou\_g|虚拟交换机名字

 |
|ZoneId|String|cn-hangzhou-g|区域 ID

 |

## 示例 {#demo .section}

请求示例

``` {#request_demo}

http(s)://[Endpoint]/?Action=ListEmrAvailableConfig
&RegionId=cn-hangzhou
&<公共请求参数>

```

正常返回示例

`XML` 格式

``` {#xml_return_success_demo}
<ListEmrAvailableConfigResponse>
	  <data>
		    <EmrMainVersionList>
			      <EmrMainVersion>
				        <ClusterTypeInfoList>
					          <ClusterTypeInfo>
						            <ClusterServiceInfoList>
							              <ClusterServiceInfo>
								                <Mandatory>false</Mandatory>
								                <ServiceDisplayName>Livy</ServiceDisplayName>
								                <ServiceName>LIVY</ServiceName>
								                <ServiceVersion>0.5.0</ServiceVersion>
							              </ClusterServiceInfo>
							              <ClusterServiceInfo>
								                <Mandatory>false</Mandatory>
								                <ServiceDisplayName>Superset</ServiceDisplayName>
								                <ServiceName>SUPERSET</ServiceName>
								                <ServiceVersion>0.27.0</ServiceVersion>
							              </ClusterServiceInfo>
							              <ClusterServiceInfo>
								                <Mandatory>false</Mandatory>
								                <ServiceDisplayName>Ranger</ServiceDisplayName>
								                <ServiceName>RANGER</ServiceName>
								                <ServiceVersion>1.0.0</ServiceVersion>
							              </ClusterServiceInfo>
							              <ClusterServiceInfo>
								                <Mandatory>false</Mandatory>
								                <ServiceDisplayName>Flink</ServiceDisplayName>
								                <ServiceName>FLINK</ServiceName>
								                <ServiceVersion>1.4.0-1.0.1</ServiceVersion>
							              </ClusterServiceInfo>
							              <ClusterServiceInfo>
								                <Mandatory>true</Mandatory>
								                <ServiceDisplayName>Knox</ServiceDisplayName>
								                <ServiceName>KNOX</ServiceName>
								                <ServiceVersion>0.13.0-0.0.3</ServiceVersion>
							              </ClusterServiceInfo>
							              <ClusterServiceInfo>
								                <Mandatory>true</Mandatory>
								                <ServiceDisplayName>ApacheDS</ServiceDisplayName>
								                <ServiceName>APACHEDS</ServiceName>
								                <ServiceVersion>2.0.0</ServiceVersion>
							              </ClusterServiceInfo>
							              <ClusterServiceInfo>
								                <Mandatory>false</Mandatory>
								                <ServiceDisplayName>Storm</ServiceDisplayName>
								                <ServiceName>STORM</ServiceName>
								                <ServiceVersion>1.1.2</ServiceVersion>
							              </ClusterServiceInfo>
							              <ClusterServiceInfo>
								                <Mandatory>true</Mandatory>
								                <ServiceDisplayName>Zeppelin</ServiceDisplayName>
								                <ServiceName>ZEPPELIN</ServiceName>
								                <ServiceVersion>0.8.0</ServiceVersion>
							              </ClusterServiceInfo>
							              <ClusterServiceInfo>
								                <Mandatory>true</Mandatory>
								                <ServiceDisplayName>Hue</ServiceDisplayName>
								                <ServiceName>HUE</ServiceName>
								                <ServiceVersion>4.1.0</ServiceVersion>
							              </ClusterServiceInfo>
							              <ClusterServiceInfo>
								                <Mandatory>true</Mandatory>
								                <ServiceDisplayName>Tez</ServiceDisplayName>
								                <ServiceName>TEZ</ServiceName>
								                <ServiceVersion>0.9.1-1.0.2</ServiceVersion>
							              </ClusterServiceInfo>
							              <ClusterServiceInfo>
								                <Mandatory>true</Mandatory>
								                <ServiceDisplayName>Sqoop</ServiceDisplayName>
								                <ServiceName>SQOOP</ServiceName>
								                <ServiceVersion>1.4.7-1.0.0</ServiceVersion>
							              </ClusterServiceInfo>
							              <ClusterServiceInfo>
								                <Mandatory>false</Mandatory>
								                <ServiceDisplayName>Phoenix</ServiceDisplayName>
								                <ServiceName>PHOENIX</ServiceName>
								                <ServiceVersion>4.10.0-1.0.0</ServiceVersion>
							              </ClusterServiceInfo>
							              <ClusterServiceInfo>
								                <Mandatory>true</Mandatory>
								                <ServiceDisplayName>Pig</ServiceDisplayName>
								                <ServiceName>PIG</ServiceName>
								                <ServiceVersion>0.14.0</ServiceVersion>
							              </ClusterServiceInfo>
							              <ClusterServiceInfo>
								                <Mandatory>true</Mandatory>
								                <ServiceDisplayName>Spark</ServiceDisplayName>
								                <ServiceName>SPARK</ServiceName>
								                <ServiceVersion>2.3.2-1.0.0</ServiceVersion>
							              </ClusterServiceInfo>
							              <ClusterServiceInfo>
								                <Mandatory>false</Mandatory>
								                <ServiceDisplayName>HBase</ServiceDisplayName>
								                <ServiceName>HBASE</ServiceName>
								                <ServiceVersion>1.1.1-1.0.0</ServiceVersion>
							              </ClusterServiceInfo>
							              <ClusterServiceInfo>
								                <Mandatory>true</Mandatory>
								                <ServiceDisplayName>Hive</ServiceDisplayName>
								                <ServiceName>HIVE</ServiceName>
								                <ServiceVersion>2.3.3-1.0.2</ServiceVersion>
							              </ClusterServiceInfo>
							              <ClusterServiceInfo>
								                <Mandatory>true</Mandatory>
								                <ServiceDisplayName>YARN</ServiceDisplayName>
								                <ServiceName>YARN</ServiceName>
								                <ServiceVersion>2.7.2-1.3.1</ServiceVersion>
							              </ClusterServiceInfo>
							              <ClusterServiceInfo>
								                <Mandatory>true</Mandatory>
								                <ServiceDisplayName>HDFS</ServiceDisplayName>
								                <ServiceName>HDFS</ServiceName>
								                <ServiceVersion>2.7.2-1.3.1</ServiceVersion>
							              </ClusterServiceInfo>
							              <ClusterServiceInfo>
								                <Mandatory>false</Mandatory>
								                <ServiceDisplayName>ZooKeeper</ServiceDisplayName>
								                <ServiceName>ZOOKEEPER</ServiceName>
								                <ServiceVersion>3.4.13</ServiceVersion>
							              </ClusterServiceInfo>
							              <ClusterServiceInfo>
								                <Mandatory>false</Mandatory>
								                <ServiceDisplayName>Oozie</ServiceDisplayName>
								                <ServiceName>OOZIE</ServiceName>
								                <ServiceVersion>4.2.0</ServiceVersion>
							              </ClusterServiceInfo>
							              <ClusterServiceInfo>
								                <Mandatory>false</Mandatory>
								                <ServiceDisplayName>Presto</ServiceDisplayName>
								                <ServiceName>PRESTO</ServiceName>
								                <ServiceVersion>0.208</ServiceVersion>
							              </ClusterServiceInfo>
							              <ClusterServiceInfo>
								                <Mandatory>false</Mandatory>
								                <ServiceDisplayName>Impala</ServiceDisplayName>
								                <ServiceName>IMPALA</ServiceName>
								                <ServiceVersion>2.10.0-1.0.0</ServiceVersion>
							              </ClusterServiceInfo>
							              <ClusterServiceInfo>
								                <Mandatory>true</Mandatory>
								                <ServiceDisplayName>Ganglia</ServiceDisplayName>
								                <ServiceName>GANGLIA</ServiceName>
								                <ServiceVersion>3.7.2</ServiceVersion>
							              </ClusterServiceInfo>
						            </ClusterServiceInfoList>
						            <ClusterType>HADOOP</ClusterType>
					          </ClusterTypeInfo>
					          <ClusterTypeInfo>
						            <ClusterServiceInfoList>
							              <ClusterServiceInfo>
								                <Mandatory>false</Mandatory>
								                <ServiceDisplayName>Superset</ServiceDisplayName>
								                <ServiceName>SUPERSET</ServiceName>
								                <ServiceVersion>0.27.0</ServiceVersion>
							              </ClusterServiceInfo>
							              <ClusterServiceInfo>
								                <Mandatory>true</Mandatory>
								                <ServiceDisplayName>Druid</ServiceDisplayName>
								                <ServiceName>DRUID</ServiceName>
								                <ServiceVersion>0.12.3-1.0.0</ServiceVersion>
							              </ClusterServiceInfo>
							              <ClusterServiceInfo>
								                <Mandatory>true</Mandatory>
								                <ServiceDisplayName>ApacheDS</ServiceDisplayName>
								                <ServiceName>APACHEDS</ServiceName>
								                <ServiceVersion>2.0.0</ServiceVersion>
							              </ClusterServiceInfo>
							              <ClusterServiceInfo>
								                <Mandatory>false</Mandatory>
								                <ServiceDisplayName>YARN</ServiceDisplayName>
								                <ServiceName>YARN</ServiceName>
								                <ServiceVersion>2.7.2-1.3.1</ServiceVersion>
							              </ClusterServiceInfo>
							              <ClusterServiceInfo>
								                <Mandatory>true</Mandatory>
								                <ServiceDisplayName>HDFS</ServiceDisplayName>
								                <ServiceName>HDFS</ServiceName>
								                <ServiceVersion>2.7.2-1.3.1</ServiceVersion>
							              </ClusterServiceInfo>
							              <ClusterServiceInfo>
								                <Mandatory>true</Mandatory>
								                <ServiceDisplayName>ZooKeeper</ServiceDisplayName>
								                <ServiceName>ZOOKEEPER</ServiceName>
								                <ServiceVersion>3.4.13</ServiceVersion>
							              </ClusterServiceInfo>
							              <ClusterServiceInfo>
								                <Mandatory>true</Mandatory>
								                <ServiceDisplayName>Ganglia</ServiceDisplayName>
								                <ServiceName>GANGLIA</ServiceName>
								                <ServiceVersion>3.7.2</ServiceVersion>
							              </ClusterServiceInfo>
						            </ClusterServiceInfoList>
						            <ClusterType>DRUID</ClusterType>
					          </ClusterTypeInfo>
					          <ClusterTypeInfo>
						            <ClusterServiceInfoList>
							              <ClusterServiceInfo>
								                <Mandatory>true</Mandatory>
								                <ServiceDisplayName>Jupyter</ServiceDisplayName>
								                <ServiceName>JUPYTER</ServiceName>
								                <ServiceVersion>4.4.0</ServiceVersion>
							              </ClusterServiceInfo>
							              <ClusterServiceInfo>
								                <Mandatory>true</Mandatory>
								                <ServiceDisplayName>Analytics Zoo</ServiceDisplayName>
								                <ServiceName>ANALYTICS-ZOO</ServiceName>
								                <ServiceVersion>0.2.0</ServiceVersion>
							              </ClusterServiceInfo>
							              <ClusterServiceInfo>
								                <Mandatory>true</Mandatory>
								                <ServiceDisplayName>ApacheDS</ServiceDisplayName>
								                <ServiceName>APACHEDS</ServiceName>
								                <ServiceVersion>2.0.0</ServiceVersion>
							              </ClusterServiceInfo>
							              <ClusterServiceInfo>
								                <Mandatory>true</Mandatory>
								                <ServiceDisplayName>Tensorflow on YARN</ServiceDisplayName>
								                <ServiceName>TENSORFLOW-ON-YARN</ServiceName>
								                <ServiceVersion>1.0.0</ServiceVersion>
							              </ClusterServiceInfo>
							              <ClusterServiceInfo>
								                <Mandatory>false</Mandatory>
								                <ServiceDisplayName>TensorFlow</ServiceDisplayName>
								                <ServiceName>TENSORFLOW</ServiceName>
								                <ServiceVersion>1.8.0</ServiceVersion>
							              </ClusterServiceInfo>
							              <ClusterServiceInfo>
								                <Mandatory>true</Mandatory>
								                <ServiceDisplayName>Zeppelin</ServiceDisplayName>
								                <ServiceName>ZEPPELIN</ServiceName>
								                <ServiceVersion>0.8.0</ServiceVersion>
							              </ClusterServiceInfo>
							              <ClusterServiceInfo>
								                <Mandatory>false</Mandatory>
								                <ServiceDisplayName>Hue</ServiceDisplayName>
								                <ServiceName>HUE</ServiceName>
								                <ServiceVersion>4.1.0</ServiceVersion>
							              </ClusterServiceInfo>
							              <ClusterServiceInfo>
								                <Mandatory>true</Mandatory>
								                <ServiceDisplayName>Spark</ServiceDisplayName>
								                <ServiceName>SPARK</ServiceName>
								                <ServiceVersion>2.3.2-1.0.0</ServiceVersion>
							              </ClusterServiceInfo>
							              <ClusterServiceInfo>
								                <Mandatory>false</Mandatory>
								                <ServiceDisplayName>Hive</ServiceDisplayName>
								                <ServiceName>HIVE</ServiceName>
								                <ServiceVersion>2.3.3-1.0.2</ServiceVersion>
							              </ClusterServiceInfo>
							              <ClusterServiceInfo>
								                <Mandatory>true</Mandatory>
								                <ServiceDisplayName>YARN</ServiceDisplayName>
								                <ServiceName>YARN</ServiceName>
								                <ServiceVersion>2.7.2-1.3.1</ServiceVersion>
							              </ClusterServiceInfo>
							              <ClusterServiceInfo>
								                <Mandatory>true</Mandatory>
								                <ServiceDisplayName>HDFS</ServiceDisplayName>
								                <ServiceName>HDFS</ServiceName>
								                <ServiceVersion>2.7.2-1.3.1</ServiceVersion>
							              </ClusterServiceInfo>
							              <ClusterServiceInfo>
								                <Mandatory>true</Mandatory>
								                <ServiceDisplayName>ZooKeeper</ServiceDisplayName>
								                <ServiceName>ZOOKEEPER</ServiceName>
								                <ServiceVersion>3.4.13</ServiceVersion>
							              </ClusterServiceInfo>
							              <ClusterServiceInfo>
								                <Mandatory>true</Mandatory>
								                <ServiceDisplayName>Ganglia</ServiceDisplayName>
								                <ServiceName>GANGLIA</ServiceName>
								                <ServiceVersion>3.7.2</ServiceVersion>
							              </ClusterServiceInfo>
						            </ClusterServiceInfoList>
						            <ClusterType>DATA_SCIENCE</ClusterType>
					          </ClusterTypeInfo>
					          <ClusterTypeInfo>
						            <ClusterServiceInfoList>
							              <ClusterServiceInfo>
								                <Mandatory>false</Mandatory>
								                <ServiceDisplayName>Ranger</ServiceDisplayName>
								                <ServiceName>RANGER</ServiceName>
								                <ServiceVersion>1.0.0</ServiceVersion>
							              </ClusterServiceInfo>
							              <ClusterServiceInfo>
								                <Mandatory>true</Mandatory>
								                <ServiceDisplayName>Kafka-Manager</ServiceDisplayName>
								                <ServiceName>KAFKA-MANAGER</ServiceName>
								                <ServiceVersion>1.3.3.16-1.1.0</ServiceVersion>
							              </ClusterServiceInfo>
							              <ClusterServiceInfo>
								                <Mandatory>true</Mandatory>
								                <ServiceDisplayName>Kafka</ServiceDisplayName>
								                <ServiceName>KAFKA</ServiceName>
								                <ServiceVersion>2.11-1.1.0-1.0.0</ServiceVersion>
							              </ClusterServiceInfo>
							              <ClusterServiceInfo>
								                <Mandatory>true</Mandatory>
								                <ServiceDisplayName>ZooKeeper</ServiceDisplayName>
								                <ServiceName>ZOOKEEPER</ServiceName>
								                <ServiceVersion>3.4.13</ServiceVersion>
							              </ClusterServiceInfo>
							              <ClusterServiceInfo>
								                <Mandatory>true</Mandatory>
								                <ServiceDisplayName>Ganglia</ServiceDisplayName>
								                <ServiceName>GANGLIA</ServiceName>
								                <ServiceVersion>3.7.2</ServiceVersion>
							              </ClusterServiceInfo>
						            </ClusterServiceInfoList>
						            <ClusterType>KAFKA</ClusterType>
					          </ClusterTypeInfo>
					          <ClusterTypeInfo>
						            <ClusterServiceInfoList>
							              <ClusterServiceInfo>
								                <Mandatory>true</Mandatory>
								                <ServiceDisplayName>ZooKeeper</ServiceDisplayName>
								                <ServiceName>ZOOKEEPER</ServiceName>
								                <ServiceVersion>3.4.13</ServiceVersion>
							              </ClusterServiceInfo>
							              <ClusterServiceInfo>
								                <Mandatory>true</Mandatory>
								                <ServiceDisplayName>Ganglia</ServiceDisplayName>
								                <ServiceName>GANGLIA</ServiceName>
								                <ServiceVersion>3.7.2</ServiceVersion>
							              </ClusterServiceInfo>
						            </ClusterServiceInfoList>
						            <ClusterType>ZOOKEEPER</ClusterType>
					          </ClusterTypeInfo>
				        </ClusterTypeInfoList>
				        <EcmVersion>true</EcmVersion>
				        <MainVersionName>EMR-3.15.1</MainVersionName>
				        <RegionId>cn-hangzhou</RegionId>
			      </EmrMainVersion>
		    </EmrMainVersionList>
		    <KeyPairNameList>
			      <KeyPairName>lr_test</KeyPairName>
			      <KeyPairName>zx</KeyPairName>
		    </KeyPairNameList>
		    <RequestId>BFA129D8-2D93-4B20-9883-FBD981483585</RequestId>
		    <SecurityGroupList>
			      <SecurityGroup>
				        <CreationTime>2018-12-03T10:11:55Z</CreationTime>
				        <Description></Description>
				        <SecurityGroupId>sg-bp1j1n0xcwfs19y9****</SecurityGroupId>
				        <SecurityGroupName>ziguansg</SecurityGroupName>
				        <VpcId>vpc-bp1d618azoa9go6wo****</VpcId>
			      </SecurityGroup>
		    </SecurityGroupList>
		    <VpcInfoList>
			      <VpcInfo>
				        <CidrBlock>192.168.0.0/16</CidrBlock>
				        <CreationTime>2018-11-22T07:38:44Z</CreationTime>
				        <Description></Description>
				        <VpcId>vpc-bp1d618azoa9go6wo****</VpcId>
				        <VpcName>zgtest</VpcName>
				        <VswitchInfoList>
					          <VswitchInfo>
						            <AvailableIpAddressCount>200</AvailableIpAddressCount>
						            <CidrBlock>192.168.0.0/24</CidrBlock>
						            <CreationTime>2018-11-22T07:38:49Z</CreationTime>
						            <Description></Description>
						            <VpcId>vpc-bp1d618azoa9go6wo****</VpcId>
						            <VswitchId>vsw-bp18amcazibt1u0d8****</VswitchId>
						            <VswitchName>hangzhou_g</VswitchName>
						            <ZoneId>cn-hangzhou-g</ZoneId>
					          </VswitchInfo>
				        </VswitchInfoList>
			      </VpcInfo>
			      <VpcInfo>
				        <CidrBlock>192.168.0.0/16</CidrBlock>
				        <CreationTime>2018-04-02T07:46:16Z</CreationTime>
				        <Description>ecm-admin-case-专用</Description>
				        <VpcId>vpc-bp1t3gj27698ijqq7****</VpcId>
				        <VpcName>ecm-admin-case-专用</VpcName>
				        <VswitchInfoList>
					          <VswitchInfo>
						            <AvailableIpAddressCount>234</AvailableIpAddressCount>
						            <CidrBlock>192.168.100.0/24</CidrBlock>
						            <CreationTime>2018-06-04T11:51:48Z</CreationTime>
						            <Description></Description>
						            <VpcId>vpc-bp1t3gj27698ijqq7****</VpcId>
						            <VswitchId>vsw-bp1eph7e1p67a4pru****</VswitchId>
						            <VswitchName>xxx</VswitchName>
						            <ZoneId>cn-hangzhou-f</ZoneId>
					          </VswitchInfo>
					          <VswitchInfo>
						            <AvailableIpAddressCount>236</AvailableIpAddressCount>
						            <CidrBlock>192.168.0.0/24</CidrBlock>
						            <CreationTime>2018-04-02T07:46:21Z</CreationTime>
						            <Description>ecm-admin-case-专用</Description>
						            <VpcId>vpc-bp1t3gj27698ijqq7****</VpcId>
						            <VswitchId>vsw-bp1iyxwwoxqlyr6ue****</VswitchId>
						            <VswitchName>ecm-admin-case-专用</VswitchName>
						            <ZoneId>cn-hangzhou-b</ZoneId>
					          </VswitchInfo>
				        </VswitchInfoList>
			      </VpcInfo>
		    </VpcInfoList>
	  </data>
	  <requestId>BFA129D8-2D93-4B20-9883-FBD981483585</requestId>
</ListEmrAvailableConfigResponse>
```

`JSON` 格式

``` {#json_return_success_demo}
{
	"requestId":"BFA129D8-2D93-4B20-9883-FBD981483585",
	"data":{
		"VpcInfoList":{
			"VpcInfo":[
				{
					"CreationTime":"2018-11-22T07:38:44Z",
					"VpcName":"zgtest",
					"CidrBlock":"192.168.0.0/16",
					"Description":"",
					"VswitchInfoList":{
						"VswitchInfo":[
							{
								"CreationTime":"2018-11-22T07:38:49Z",
								"VswitchName":"hangzhou_g",
								"CidrBlock":"192.168.0.0/24",
								"Description":"",
								"AvailableIpAddressCount":200,
								"VswitchId":"vsw-bp18amcazibt1u0d8****",
								"ZoneId":"cn-hangzhou-g",
								"VpcId":"vpc-bp1d618azoa9go6wo****"
							}
						]
					},
					"VpcId":"vpc-bp1d618azoa9go6wo****"
				},
				{
					"CreationTime":"2018-04-02T07:46:16Z",
					"VpcName":"ecm-admin-case-专用",
					"CidrBlock":"192.168.0.0/16",
					"Description":"ecm-admin-case-专用",
					"VswitchInfoList":{
						"VswitchInfo":[
							{
								"CreationTime":"2018-06-04T11:51:48Z",
								"VswitchName":"xxx",
								"CidrBlock":"192.168.100.0/24",
								"Description":"",
								"AvailableIpAddressCount":234,
								"VswitchId":"vsw-bp1eph7e1p67a4pru****",
								"ZoneId":"cn-hangzhou-f",
								"VpcId":"vpc-bp1t3gj27698ijqq7****"
							},
							{
								"CreationTime":"2018-04-02T07:46:21Z",
								"VswitchName":"ecm-admin-case-专用",
								"CidrBlock":"192.168.0.0/24",
								"Description":"ecm-admin-case-专用",
								"AvailableIpAddressCount":236,
								"VswitchId":"vsw-bp1iyxwwoxqlyr6ue****",
								"ZoneId":"cn-hangzhou-b",
								"VpcId":"vpc-bp1t3gj27698ijqq7****"
							}
						]
					},
					"VpcId":"vpc-bp1t3gj27698ijqq7****"
				}
			]
		},
		"SecurityGroupList":{
			"SecurityGroup":[
				{
					"CreationTime":"2018-12-03T10:11:55Z",
					"SecurityGroupId":"sg-bp1j1n0xcwfs19y9****",
					"SecurityGroupName":"ziguansg",
					"Description":"",
					"VpcId":"vpc-bp1d618azoa9go6wo****"
				}
			]
		},
		"RequestId":"BFA129D8-2D93-4B20-9883-FBD981483585",
		"EmrMainVersionList":{
			"EmrMainVersion":[
				{
					"MainVersionName":"EMR-3.15.1",
					"RegionId":"cn-hangzhou",
					"ClusterTypeInfoList":{
						"ClusterTypeInfo":[
							{
								"ClusterServiceInfoList":{
									"ClusterServiceInfo":[
										{
											"ServiceName":"LIVY",
											"Mandatory":false,
											"ServiceVersion":"0.5.0",
											"ServiceDisplayName":"Livy"
										},
										{
											"ServiceName":"SUPERSET",
											"Mandatory":false,
											"ServiceVersion":"0.27.0",
											"ServiceDisplayName":"Superset"
										},
										{
											"ServiceName":"RANGER",
											"Mandatory":false,
											"ServiceVersion":"1.0.0",
											"ServiceDisplayName":"Ranger"
										},
										{
											"ServiceName":"FLINK",
											"Mandatory":false,
											"ServiceVersion":"1.4.0-1.0.1",
											"ServiceDisplayName":"Flink"
										},
										{
											"ServiceName":"KNOX",
											"Mandatory":true,
											"ServiceVersion":"0.13.0-0.0.3",
											"ServiceDisplayName":"Knox"
										},
										{
											"ServiceName":"APACHEDS",
											"Mandatory":true,
											"ServiceVersion":"2.0.0",
											"ServiceDisplayName":"ApacheDS"
										},
										{
											"ServiceName":"STORM",
											"Mandatory":false,
											"ServiceVersion":"1.1.2",
											"ServiceDisplayName":"Storm"
										},
										{
											"ServiceName":"ZEPPELIN",
											"Mandatory":true,
											"ServiceVersion":"0.8.0",
											"ServiceDisplayName":"Zeppelin"
										},
										{
											"ServiceName":"HUE",
											"Mandatory":true,
											"ServiceVersion":"4.1.0",
											"ServiceDisplayName":"Hue"
										},
										{
											"ServiceName":"TEZ",
											"Mandatory":true,
											"ServiceVersion":"0.9.1-1.0.2",
											"ServiceDisplayName":"Tez"
										},
										{
											"ServiceName":"SQOOP",
											"Mandatory":true,
											"ServiceVersion":"1.4.7-1.0.0",
											"ServiceDisplayName":"Sqoop"
										},
										{
											"ServiceName":"PHOENIX",
											"Mandatory":false,
											"ServiceVersion":"4.10.0-1.0.0",
											"ServiceDisplayName":"Phoenix"
										},
										{
											"ServiceName":"PIG",
											"Mandatory":true,
											"ServiceVersion":"0.14.0",
											"ServiceDisplayName":"Pig"
										},
										{
											"ServiceName":"SPARK",
											"Mandatory":true,
											"ServiceVersion":"2.3.2-1.0.0",
											"ServiceDisplayName":"Spark"
										},
										{
											"ServiceName":"HBASE",
											"Mandatory":false,
											"ServiceVersion":"1.1.1-1.0.0",
											"ServiceDisplayName":"HBase"
										},
										{
											"ServiceName":"HIVE",
											"Mandatory":true,
											"ServiceVersion":"2.3.3-1.0.2",
											"ServiceDisplayName":"Hive"
										},
										{
											"ServiceName":"YARN",
											"Mandatory":true,
											"ServiceVersion":"2.7.2-1.3.1",
											"ServiceDisplayName":"YARN"
										},
										{
											"ServiceName":"HDFS",
											"Mandatory":true,
											"ServiceVersion":"2.7.2-1.3.1",
											"ServiceDisplayName":"HDFS"
										},
										{
											"ServiceName":"ZOOKEEPER",
											"Mandatory":false,
											"ServiceVersion":"3.4.13",
											"ServiceDisplayName":"ZooKeeper"
										},
										{
											"ServiceName":"OOZIE",
											"Mandatory":false,
											"ServiceVersion":"4.2.0",
											"ServiceDisplayName":"Oozie"
										},
										{
											"ServiceName":"PRESTO",
											"Mandatory":false,
											"ServiceVersion":"0.208",
											"ServiceDisplayName":"Presto"
										},
										{
											"ServiceName":"IMPALA",
											"Mandatory":false,
											"ServiceVersion":"2.10.0-1.0.0",
											"ServiceDisplayName":"Impala"
										},
										{
											"ServiceName":"GANGLIA",
											"Mandatory":true,
											"ServiceVersion":"3.7.2",
											"ServiceDisplayName":"Ganglia"
										}
									]
								},
								"ClusterType":"HADOOP"
							},
							{
								"ClusterServiceInfoList":{
									"ClusterServiceInfo":[
										{
											"ServiceName":"SUPERSET",
											"Mandatory":false,
											"ServiceVersion":"0.27.0",
											"ServiceDisplayName":"Superset"
										},
										{
											"ServiceName":"DRUID",
											"Mandatory":true,
											"ServiceVersion":"0.12.3-1.0.0",
											"ServiceDisplayName":"Druid"
										},
										{
											"ServiceName":"APACHEDS",
											"Mandatory":true,
											"ServiceVersion":"2.0.0",
											"ServiceDisplayName":"ApacheDS"
										},
										{
											"ServiceName":"YARN",
											"Mandatory":false,
											"ServiceVersion":"2.7.2-1.3.1",
											"ServiceDisplayName":"YARN"
										},
										{
											"ServiceName":"HDFS",
											"Mandatory":true,
											"ServiceVersion":"2.7.2-1.3.1",
											"ServiceDisplayName":"HDFS"
										},
										{
											"ServiceName":"ZOOKEEPER",
											"Mandatory":true,
											"ServiceVersion":"3.4.13",
											"ServiceDisplayName":"ZooKeeper"
										},
										{
											"ServiceName":"GANGLIA",
											"Mandatory":true,
											"ServiceVersion":"3.7.2",
											"ServiceDisplayName":"Ganglia"
										}
									]
								},
								"ClusterType":"DRUID"
							},
							{
								"ClusterServiceInfoList":{
									"ClusterServiceInfo":[
										{
											"ServiceName":"JUPYTER",
											"Mandatory":true,
											"ServiceVersion":"4.4.0",
											"ServiceDisplayName":"Jupyter"
										},
										{
											"ServiceName":"ANALYTICS-ZOO",
											"Mandatory":true,
											"ServiceVersion":"0.2.0",
											"ServiceDisplayName":"Analytics Zoo"
										},
										{
											"ServiceName":"APACHEDS",
											"Mandatory":true,
											"ServiceVersion":"2.0.0",
											"ServiceDisplayName":"ApacheDS"
										},
										{
											"ServiceName":"TENSORFLOW-ON-YARN",
											"Mandatory":true,
											"ServiceVersion":"1.0.0",
											"ServiceDisplayName":"Tensorflow on YARN"
										},
										{
											"ServiceName":"TENSORFLOW",
											"Mandatory":false,
											"ServiceVersion":"1.8.0",
											"ServiceDisplayName":"TensorFlow"
										},
										{
											"ServiceName":"ZEPPELIN",
											"Mandatory":true,
											"ServiceVersion":"0.8.0",
											"ServiceDisplayName":"Zeppelin"
										},
										{
											"ServiceName":"HUE",
											"Mandatory":false,
											"ServiceVersion":"4.1.0",
											"ServiceDisplayName":"Hue"
										},
										{
											"ServiceName":"SPARK",
											"Mandatory":true,
											"ServiceVersion":"2.3.2-1.0.0",
											"ServiceDisplayName":"Spark"
										},
										{
											"ServiceName":"HIVE",
											"Mandatory":false,
											"ServiceVersion":"2.3.3-1.0.2",
											"ServiceDisplayName":"Hive"
										},
										{
											"ServiceName":"YARN",
											"Mandatory":true,
											"ServiceVersion":"2.7.2-1.3.1",
											"ServiceDisplayName":"YARN"
										},
										{
											"ServiceName":"HDFS",
											"Mandatory":true,
											"ServiceVersion":"2.7.2-1.3.1",
											"ServiceDisplayName":"HDFS"
										},
										{
											"ServiceName":"ZOOKEEPER",
											"Mandatory":true,
											"ServiceVersion":"3.4.13",
											"ServiceDisplayName":"ZooKeeper"
										},
										{
											"ServiceName":"GANGLIA",
											"Mandatory":true,
											"ServiceVersion":"3.7.2",
											"ServiceDisplayName":"Ganglia"
										}
									]
								},
								"ClusterType":"DATA_SCIENCE"
							},
							{
								"ClusterServiceInfoList":{
									"ClusterServiceInfo":[
										{
											"ServiceName":"RANGER",
											"Mandatory":false,
											"ServiceVersion":"1.0.0",
											"ServiceDisplayName":"Ranger"
										},
										{
											"ServiceName":"KAFKA-MANAGER",
											"Mandatory":true,
											"ServiceVersion":"1.3.3.16-1.1.0",
											"ServiceDisplayName":"Kafka-Manager"
										},
										{
											"ServiceName":"KAFKA",
											"Mandatory":true,
											"ServiceVersion":"2.11-1.1.0-1.0.0",
											"ServiceDisplayName":"Kafka"
										},
										{
											"ServiceName":"ZOOKEEPER",
											"Mandatory":true,
											"ServiceVersion":"3.4.13",
											"ServiceDisplayName":"ZooKeeper"
										},
										{
											"ServiceName":"GANGLIA",
											"Mandatory":true,
											"ServiceVersion":"3.7.2",
											"ServiceDisplayName":"Ganglia"
										}
									]
								},
								"ClusterType":"KAFKA"
							},
							{
								"ClusterServiceInfoList":{
									"ClusterServiceInfo":[
										{
											"ServiceName":"ZOOKEEPER",
											"Mandatory":true,
											"ServiceVersion":"3.4.13",
											"ServiceDisplayName":"ZooKeeper"
										},
										{
											"ServiceName":"GANGLIA",
											"Mandatory":true,
											"ServiceVersion":"3.7.2",
											"ServiceDisplayName":"Ganglia"
										}
									]
								},
								"ClusterType":"ZOOKEEPER"
							}
						]
					},
					"EcmVersion":true
				}
			]
		},
		"KeyPairNameList":{
			"KeyPairName":[
				"lr_test",
				"zx"
			]
		}
	}
}
```

## 错误码 { .section}

访问[错误中心](https://error-center.alibabacloud.com/status/product/Emr)查看更多错误码。

