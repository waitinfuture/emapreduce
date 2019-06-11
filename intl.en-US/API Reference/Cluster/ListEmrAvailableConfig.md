# ListEmrAvailableConfig {#concept_azg_zlx_dgb .concept}

You can call this operation to view available creation configurations of E-MapReduce clusters.

## Request parameters {#section_pdw_lv4_dgb .section}

|Parameter|Type|Required|Example|Description|
|---------|----|--------|-------|-----------|
|AccessKeyId|String|Yes|LTAI8ljWyu7y\*\*\*\*|The AccessKey ID.|
|RegionId|String|Yes|cn-hangzhou|The region ID.|

## Response parameters {#section_vdw_lv4_dgb .section}

|Parameter|Type|Example|Description|
|---------|----|-------|-----------|
|RequestId|String|BF4FBAC6-B03E-4BFB-B6DB-EB53C34F2E22|The ID of the request.|
|EmrMainVersionList| | |The list of EMR versions.|
|RegionId|String|cn-hangzhou|The region ID.|
|MainVersionName|String|EMR-3.15.1|The main version.|
|EcmVersion|Boolean|true|A reserved parameter.|
|ClusterTypeInfoList| | |The list of cluster types.|
|ClusterType|String|HADOOP|The type of the cluster.|
|ClusterServiceInfoList| | |The list of services.|
|ServiceName|String|HIVE|The name of the service.|
|ServiceDisplayName|String|HIVE|The display name of the service.|
|ServiceVersion|String|2.3.3|The version of the service.|
|Mandatory|Boolean|true|Indicates whether the service is required.|
|SecurityGroupList| | |The list of security groups.|
|SecurityGroupId|String|sg-bp1j1n0xcwfs19y98b7n|The ID of the security group.|
|Description|String|sgdesc|The description of the security group.|
|SecurityGroupName|String|ziguansg|The name of the security group.|
|CreationTime|String|2018-12-03T10:11:55Z|The creation time of the cluster.|
|AvailableInstanceAmount|Integer|10|The number of available instances.|
|EcsCount|Integer|10|The number of ECS instances.|
|VpcInfoList| | |The list of VPCs.|
|VpcId|String|vpc-bp1d618azoa9go6wowjl2|The ID of the VPC.|
|VpcName|String|zgtest|The name of the VPC.|
|CidrBlock|String|192.168.0.0/16|The CIDR block.|
|VRouterId|String|0|A reserved parameter.|
|Description|String|vpc\_desc|The description of the VPC.|
|VswitchInfoList| | |The information of the VSwitch.|
|VswitchId|String|vsw-bp18amcazibt1u0d8qqqa|The ID of the VSwitch.|
|VswitchName|String|hangzhou\_g|The name of the VSwitch.|
|ZoneId|String|cn-hangzhou-g|The zone ID.|
|CidrBlock|String|192.168.0.0/24|The CIDR block.|
|AvailableIpAddressCount|Long|200|The number of available IP addresses.|
|Description|String|switch desc|The description of the VSwitch.|
|CreationTime|String|2018-11-22T07:38:49Z|The creation time of the VSwitch.|
|KeyPairNameList|String|\["key\_pair"\]|The list of key pair names.|

## Examples {#section_ydw_lv4_dgb .section}

-   Sample requests

    ```
    /? RegionId=cn-hangzhou
    &AccessKeyId=LTAI8ljWyu7y****
    &<Common request parameters>
    ```

-   Successful response examples

    JSON format

    ```
    {
    	"code":"200",
    	"data":{
    		"EmrMainVersionList":{
    			"EmrMainVersion":[{
    				"ClusterTypeInfoList":{
    					"ClusterTypeInfo":[
    						{
    							"ClusterServiceInfoList":{
    								"ClusterServiceInfo":[
    									{
    										"Mandatory":false,
    										"ServiceDisplayName":"Livy",
    										"ServiceName":"LIVY",
    										"ServiceVersion":"0.5.0"
    									},
    									{
    										"Mandatory":false,
    										"ServiceDisplayName":"Superset",
    										"ServiceName":"SUPERSET",
    										"ServiceVersion":"0.27.0"
    									},
    									{
    										"Mandatory":false,
    										"ServiceDisplayName":"Ranger",
    										"ServiceName":"RANGER",
    										"ServiceVersion":"1.0.0"
    									},
    									{
    										"Mandatory":false,
    										"ServiceDisplayName":"Flink",
    										"ServiceName":"FLINK",
    										"ServiceVersion":"1.4.0-1.0.1"
    									},
    									{
    										"Mandatory":true,
    										"ServiceDisplayName":"Knox",
    										"ServiceName":"KNOX",
    										"ServiceVersion":"0.13.0-0.0.3"
    									},
    									{
    										"Mandatory":true,
    										"ServiceDisplayName":"ApacheDS",
    										"ServiceName":"APACHEDS",
    										"ServiceVersion":"2.0.0"
    									},
    									{
    										"Mandatory":false,
    										"ServiceDisplayName":"Storm",
    										"ServiceName":"STORM",
    										"ServiceVersion":"1.1.2"
    									},
    									{
    										"Mandatory":true,
    										"ServiceDisplayName":"Zeppelin",
    										"ServiceName":"ZEPPELIN",
    										"ServiceVersion":"0.8.0"
    									},
    									{
    										"Mandatory":true,
    										"ServiceDisplayName":"Hue",
    										"ServiceName":"HUE",
    										"ServiceVersion":"4.1.0"
    									},
    									{
    										"Mandatory":true,
    										"ServiceDisplayName":"Tez",
    										"ServiceName":"TEZ",
    										"ServiceVersion":"0.9.1-1.0.2"
    									},
    									{
    										"Mandatory":true,
    										"ServiceDisplayName":"Sqoop",
    										"ServiceName":"SQOOP",
    										"ServiceVersion":"1.4.7-1.0.0"
    									},
    									{
    										"Mandatory":false,
    										"ServiceDisplayName":"Phoenix",
    										"ServiceName":"PHOENIX",
    										"ServiceVersion":"4.10.0-1.0.0"
    									},
    									{
    										"Mandatory":true,
    										"ServiceDisplayName":"Pig",
    										"ServiceName":"PIG",
    										"ServiceVersion":"0.14.0"
    									},
    									{
    										"Mandatory":true,
    										"ServiceDisplayName":"Spark",
    										"ServiceName":"SPARK",
    										"ServiceVersion":"2.3.2-1.0.0"
    									},
    									{
    										"Mandatory":false,
    										"ServiceDisplayName":"HBase",
    										"ServiceName":"HBASE",
    										"ServiceVersion":"1.1.1-1.0.0"
    									},
    									{
    										"Mandatory":true,
    										"ServiceDisplayName":"Hive",
    										"ServiceName":"HIVE",
    										"ServiceVersion":"2.3.3-1.0.2"
    									},
    									{
    										"Mandatory":true,
    										"ServiceDisplayName":"YARN",
    										"ServiceName":"YARN",
    										"ServiceVersion":"2.7.2-1.3.1"
    									},
    									{
    										"Mandatory":true,
    										"ServiceDisplayName":"HDFS",
    										"ServiceName":"HDFS",
    										"ServiceVersion":"2.7.2-1.3.1"
    									},
    									{
    										"Mandatory":false,
    										"ServiceDisplayName":"ZooKeeper",
    										"ServiceName":"ZOOKEEPER",
    										"ServiceVersion":"3.4.13"
    									},
    									{
    										"Mandatory":false,
    										"ServiceDisplayName":"Oozie",
    										"ServiceName":"OOZIE",
    										"ServiceVersion":"4.2.0"
    									},
    									{
    										"Mandatory":false,
    										"ServiceDisplayName":"Presto",
    										"ServiceName":"PRESTO",
    										"ServiceVersion":"0.208"
    									},
    									{
    										"Mandatory":false,
    										"ServiceDisplayName":"Impala",
    										"ServiceName":"IMPALA",
    										"ServiceVersion":"2.10.0-1.0.0"
    									},
    									{
    										"Mandatory":true,
    										"ServiceDisplayName":"Ganglia",
    										"ServiceName":"GANGLIA",
    										"ServiceVersion":"3.7.2"
    									}
    								]
    							},
    							"ClusterType":"HADOOP"
    						},
    						{
    							"ClusterServiceInfoList":{
    								"ClusterServiceInfo":[
    									{
    										"Mandatory":false,
    										"ServiceDisplayName":"Superset",
    										"ServiceName":"SUPERSET",
    										"ServiceVersion":"0.27.0"
    									},
    									{
    										"Mandatory":true,
    										"ServiceDisplayName":"Druid",
    										"ServiceName":"DRUID",
    										"ServiceVersion":"0.12.3-1.0.0"
    									},
    									{
    										"Mandatory":true,
    										"ServiceDisplayName":"ApacheDS",
    										"ServiceName":"APACHEDS",
    										"ServiceVersion":"2.0.0"
    									},
    									{
    										"Mandatory":false,
    										"ServiceDisplayName":"YARN",
    										"ServiceName":"YARN",
    										"ServiceVersion":"2.7.2-1.3.1"
    									},
    									{
    										"Mandatory":true,
    										"ServiceDisplayName":"HDFS",
    										"ServiceName":"HDFS",
    										"ServiceVersion":"2.7.2-1.3.1"
    									},
    									{
    										"Mandatory":true,
    										"ServiceDisplayName":"ZooKeeper",
    										"ServiceName":"ZOOKEEPER",
    										"ServiceVersion":"3.4.13"
    									},
    									{
    										"Mandatory":true,
    										"ServiceDisplayName":"Ganglia",
    										"ServiceName":"GANGLIA",
    										"ServiceVersion":"3.7.2"
    									}
    								]
    							},
    							"ClusterType":"DRUID"
    						},
    						{
    							"ClusterServiceInfoList":{
    								"ClusterServiceInfo":[
    									{
    										"Mandatory":true,
    										"ServiceDisplayName":"Jupyter",
    										"ServiceName":"JUPYTER",
    										"ServiceVersion":"4.4.0"
    									},
    									{
    										"Mandatory":true,
    										"ServiceDisplayName":"Analytics Zoo",
    										"ServiceName":"ANALYTICS-ZOO",
    										"ServiceVersion":"0.2.0"
    									},
    									{
    										"Mandatory":true,
    										"ServiceDisplayName":"ApacheDS",
    										"ServiceName":"APACHEDS",
    										"ServiceVersion":"2.0.0"
    									},
    									{
    										"Mandatory":true,
    										"ServiceDisplayName":"Tensorflow on YARN",
    										"ServiceName":"TENSORFLOW-ON-YARN",
    										"ServiceVersion":"1.0.0"
    									},
    									{
    										"Mandatory":false,
    										"ServiceDisplayName":"TensorFlow",
    										"ServiceName":"TENSORFLOW",
    										"ServiceVersion":"1.8.0"
    									},
    									{
    										"Mandatory":true,
    										"ServiceDisplayName":"Zeppelin",
    										"ServiceName":"ZEPPELIN",
    										"ServiceVersion":"0.8.0"
    									},
    									{
    										"Mandatory":false,
    										"ServiceDisplayName":"Hue",
    										"ServiceName":"HUE",
    										"ServiceVersion":"4.1.0"
    									},
    									{
    										"Mandatory":true,
    										"ServiceDisplayName":"Spark",
    										"ServiceName":"SPARK",
    										"ServiceVersion":"2.3.2-1.0.0"
    									},
    									{
    										"Mandatory":false,
    										"ServiceDisplayName":"Hive",
    										"ServiceName":"HIVE",
    										"ServiceVersion":"2.3.3-1.0.2"
    									},
    									{
    										"Mandatory":true,
    										"ServiceDisplayName":"YARN",
    										"ServiceName":"YARN",
    										"ServiceVersion":"2.7.2-1.3.1"
    									},
    									{
    										"Mandatory":true,
    										"ServiceDisplayName":"HDFS",
    										"ServiceName":"HDFS",
    										"ServiceVersion":"2.7.2-1.3.1"
    									},
    									{
    										"Mandatory":true,
    										"ServiceDisplayName":"ZooKeeper",
    										"ServiceName":"ZOOKEEPER",
    										"ServiceVersion":"3.4.13"
    									},
    									{
    										"Mandatory":true,
    										"ServiceDisplayName":"Ganglia",
    										"ServiceName":"GANGLIA",
    										"ServiceVersion":"3.7.2"
    									}
    								]
    							},
    							"ClusterType":"DATA_SCIENCE"
    						},
    						{
    							"ClusterServiceInfoList":{
    								"ClusterServiceInfo":[
    									{
    										"Mandatory":false,
    										"ServiceDisplayName":"Ranger",
    										"ServiceName":"RANGER",
    										"ServiceVersion":"1.0.0"
    									},
    									{
    										"Mandatory":true,
    										"ServiceDisplayName":"Kafka-Manager",
    										"ServiceName":"KAFKA-MANAGER",
    										"ServiceVersion":"1.3.3.16-1.1.0"
    									},
    									{
    										"Mandatory":true,
    										"ServiceDisplayName":"Kafka",
    										"ServiceName":"KAFKA",
    										"ServiceVersion":"2.11-1.1.0-1.0.0"
    									},
    									{
    										"Mandatory":true,
    										"ServiceDisplayName":"ZooKeeper",
    										"ServiceName":"ZOOKEEPER",
    										"ServiceVersion":"3.4.13"
    									},
    									{
    										"Mandatory":true,
    										"ServiceDisplayName":"Ganglia",
    										"ServiceName":"GANGLIA",
    										"ServiceVersion":"3.7.2"
    									}
    								]
    							},
    							"ClusterType":"KAFKA"
    						},
    						{
    							"ClusterServiceInfoList":{
    								"ClusterServiceInfo":[
    									{
    										"Mandatory":true,
    										"ServiceDisplayName":"ZooKeeper",
    										"ServiceName":"ZOOKEEPER",
    										"ServiceVersion":"3.4.13"
    									},
    									{
    										"Mandatory":true,
    										"ServiceDisplayName":"Ganglia",
    										"ServiceName":"GANGLIA",
    										"ServiceVersion":"3.7.2"
    									}
    								]
    							},
    							"ClusterType":"ZOOKEEPER"
    						}
    					]
    				},
    				"EcmVersion":true,
    				"MainVersionName":"EMR-3.15.1",
    				"RegionId":"cn-hangzhou"
    			}]
    		},
    		"KeyPairNameList":{
    			"KeyPairName":[
    				"lr_test",
    				"zx"
    			]
    		},
    		"RequestId":"BFA129D8-2D93-4B20-9883-FBD981483585",
    		"SecurityGroupList":{
    			"SecurityGroup":[{
    				"CreationTime":"2018-12-03T10:11:55Z",
    				"Description":"",
    				"SecurityGroupId":"sg-bp1j1n0xcwfs19y98b7n",
    				"SecurityGroupName":"ziguansg",
    				"VpcId":"vpc-bp1d618azoa9go6wowjl2"
    			}]
    		},
    		"VpcInfoList":{
    			"VpcInfo":[
    				{
    					"CidrBlock":"192.168.0.0/16",
    					"CreationTime":"2018-11-22T07:38:44Z",
    					"Description":"",
    					"VpcId":"vpc-bp1d618azoa9go6wowjl2",
    					"VpcName":"zgtest",
    					"VswitchInfoList ":{
    						"VswitchInfo":[{
    							"AvailableIpAddressCount":200,
    							"CidrBlock":"192.168.0.0/24",
    							"CreationTime":"2018-11-22T07:38:49Z",
    							"Description":"",
    							"VpcId":"vpc-bp1d618azoa9go6wowjl2",
    							"VswitchId":"vsw-bp18amcazibt1u0d8qqqa",
    							"VswitchName":"hangzhou_g",
    							"ZoneId":"cn-hangzhou-g"
    						}]
    					}
    				},
    				{
    					"CidrBlock":"192.168.0.0/16",
    					"CreationTime":"2018-04-02T07:46:16Z",
    					"Description":"ecm-admin-case-dedicated",
    					"VpcId":"vpc-bp1t3gj27698ijqq7xqpg",
    					"VpcName":"ecm-admin-case-dedicated",
    					"VswitchInfoList":{
    						"VswitchInfo":[
    							{
    								"AvailableIpAddressCount":234,
    								"CidrBlock":"192.168.100.0/24",
    								"CreationTime":"2018-06-04T11:51:48Z",
    								"Description":"",
    								"VpcId":"vpc-bp1t3gj27698ijqq7xqpg",
    								"VswitchId":"vsw-bp1eph7e1p67a4pru99rw",
    								"VswitchName":"xxx",
    								"ZoneId":"cn-hangzhou-f"
    							},
    							{
    								"AvailableIpAddressCount":236,
    								"CidrBlock":"192.168.0.0/24",
    								"CreationTime":"2018-04-02T07:46:21Z",
    								"Description":"ecm-admin-case-dedicated",
    								"VpcId":"vpc-bp1t3gj27698ijqq7xqpg",
    								"VswitchId":"vsw-bp1iyxwwoxqlyr6uebo1b",
    								"VswitchName":"ecm-admin-case-dedicated",
    								"ZoneId": "cn-hangzhou-b"
    							}
    						]
    					}
    				}
    			]
    		}
    	},
    	"requestId":"BFA129D8-2D93-4B20-9883-FBD981483585",
    	"successResponse":true
    }
    ```

-   Error response examples

    JSON format

    ```
    {
    	"code":"RAM.Permission.NotAllow",
    	"message":"It is not allow to execute this operation,please use RAM to authorize!",
    	"requestId":"9AEDC439-1F63-491D-B8C6-9737C372BF3A",
    	"successResponse":false
    }
    ```


## Error codes {#section_a2w_lv4_dgb .section}

