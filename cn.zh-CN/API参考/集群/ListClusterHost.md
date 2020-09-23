# ListClusterHost

调用ListClusterHost接口查询集群主机列表，包括磁盘和CPU内存配置等信息。

## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=Emr&api=ListClusterHost&type=RPC&version=2016-04-08)

## 请求参数

|名称|类型|是否必选|示例值|描述|
|--|--|----|---|--|
|Action|String|是|ListClusterHost|系统规定参数。对于您自行拼凑HTTP或HTTPS URL发起的API请求，该参数为必选参数。取值：ListClusterHost。 |
|ClusterId|String|是|C-D7CA98AAA96A\*\*\*\*|集群ID。您可以调用[ListClusters](~~28147~~)接口查看集群的ID。 |
|RegionId|String|是|cn-hangzhou|地域ID。您可以通过[DescribeRegions](~~25609~~)接口查看最新的阿里云地域列表。 |
|HostInstanceId|String|否|i-bp11vdyh3l6xvmnl\*\*\*\*|ECS实例ID。 |
|HostGroupId|String|否|G-A5EA210E15FC\*\*\*\*|机器组ID。 |
|HostName|String|否|emr-header-1|主机名。 |
|PrivateIp|String|否|192.\*\*\*.\*\*\*.\*\*\*|主机内网IP。 |
|PublicIp|String|否|47.\*\*\*.\*\*\*.\*\*\*|主机公网IP。 |
|GroupType|String|否|MASTER|机器组类型：

 -   MASTER：主实例节点
-   CORE：核心实例节点
-   TASK：计算实例节点 |
|ComponentName|String|否|HiveServer2|安装了指定组件名称的主机。 |
|StatusList.N|RepeatList|否|\["NORMAL"\]|主机状态列表：

 -   NORMAL：正常
-   ABNORMAL：异常
-   RESIZING：配置中
-   INITIALIZING：初始化中
-   RELEASED：已释放 |
|PageNumber|Integer|否|1|分页页数，从1开始。 |
|PageSize|Integer|否|10|分页大小。 |

## 返回数据

|名称|类型|示例值|描述|
|--|--|---|--|
|HostList|Array of Host| |主机列表。 |
|Host| | | |
|ChargeType|String|PostPaid|付费类型：

 -   PostPaid：按量付费集群
-   PrePaid：包年包月集群 |
|Cpu|Integer|4|CPU核心数。 |
|CreateTime|String|1599635156000|创建时间。 |
|DiskList|Array of Disk| |磁盘列表。 |
|Disk| | | |
|Device|String|/dev/xvde|磁盘名称。 |
|DiskId|String|d-bp1aq78lhbig6iel\*\*\*\*|磁盘ID。 |
|DiskSize|Integer|80|磁盘容量。 |
|DiskType|String|CLOUD\_ESSD|磁盘类型：

 -   CLOUD\_ESSD：ESSD云盘
-   CLOUD\_SSD：SSD云盘
-   CLOUD：云盘
-   CLOUD\_EFFCIENCY：高效云盘 |
|Type|String|data|磁盘对应的角色类型。 |
|EmrExpiredTime|String|无|E-MapReduce超时时间。保留字段。 |
|ExpiredTime|Long|32493801600000|主机过期时间。 |
|HostGroupId|String|G-A5EA210E15FC\*\*\*\*|机器组ID。 |
|HostInstanceId|String|i-bp1cfwf2cwgji7ds\*\*\*\*|主机ECS实例ID。 |
|HostName|String|emr-header-1|主机名。 |
|InstanceStatus|String|NORMAL|主机状态。 |
|InstanceType|String|ecs.mn4.xlarge|主机的型号。 |
|Memory|Integer|16|主机的内存（G）。 |
|PrivateIp|String|192.\*\*\*.\*\*\*\*.\*\*\*|主机内网IP。 |
|PublicIp|String|47.\*\*\*.\*\*\*.\*\*\*|主机公网IP。 |
|Role|String|MASTER|主机在集群中的角色：

 -   MASTER：主实例节点
-   CORE：核心实例节点
-   TASK：计算实例节点 |
|SerialNumber|String|3c2a5078-778d-4c18-87e4-1a38fbcb\*\*\*\*|主机序列号。 |
|Status|String|NORMAL|主机状态：

 -   NORMAL：正常
-   ABNORMAL：异常
-   RESIZING：配置中
-   INITIALIZING：初始化中
-   RELEASED：已释放 |
|SupportIpV6|Boolean|false|是否支持IPv6。 |
|Type|String|VM|主机类型。 |
|ZoneId|String|cn-hangzhou-i|区域ID。 |
|PageNumber|Integer|1|分页页数。 |
|PageSize|Integer|10|分页大小。 |
|RequestId|String|50F7151C-915D-4576-A291-833E8D193853|请求ID。 |
|Total|Integer|12|查询总数。 |

## 示例

请求示例

```
http(s)://[Endpoint]/?Action=ListClusterHost
&ClusterId=C-D7CA98AAA96A****
&RegionId=cn-hangzhou
&<公共请求参数>
```

正常返回示例

`XML` 格式

```
<RequestId>50F7151C-915D-4576-A291-833E8D193853</RequestId>
<PageSize>10</PageSize>
<PageNumber>1</PageNumber>
<Total>3</Total>
<HostList>
    <Host>
        <Status>NORMAL</Status>
        <ZoneId>cn-hangzhou-i</ZoneId>
        <PublicIp/>
        <Memory>16</Memory>
        <CreateTime>1599635156000</CreateTime>
        <Cpu>4</Cpu>
        <HostInstanceId>i-bp1bqta9j2d7x7tc****</HostInstanceId>
        <Role>CORE</Role>
        <SerialNumber>bfad582f-a6bc-40b7-868e-60940b0be9ae</SerialNumber>
        <PrivateIp>192.**.**.**</PrivateIp>
        <ChargeType>PostPaid</ChargeType>
        <ExpiredTime>32493801600000</ExpiredTime>
        <DiskList>
            <Disk>
                <DiskType>CLOUD_ESSD</DiskType>
                <Type>data</Type>
                <Device>/dev/xvde</Device>
                <DiskSize>80</DiskSize>
                <DiskId>d-bp17scdtdsb4r5qx****</DiskId>
            </Disk>
            <Disk>
                <DiskType>CLOUD_ESSD</DiskType>
                <Type>data</Type>
                <Device>/dev/xvdd</Device>
                <DiskSize>80</DiskSize>
                <DiskId>d-bp1620snh1vzd8bo****</DiskId>
            </Disk>
            <Disk>
                <DiskType>CLOUD_ESSD</DiskType>
                <Type>data</Type>
                <Device>/dev/xvdc</Device>
                <DiskSize>80</DiskSize>
                <DiskId>d-bp1i3puwiaha37ru****</DiskId>
            </Disk>
            <Disk>
                <DiskType>CLOUD_ESSD</DiskType>
                <Type>data</Type>
                <Device>/dev/xvdb</Device>
                <DiskSize>80</DiskSize>
                <DiskId>d-bp14h6m9spidesdt****</DiskId>
            </Disk>
            <Disk>
                <DiskType>CLOUD_ESSD</DiskType>
                <Type>system</Type>
                <Device>/dev/xvda</Device>
                <DiskSize>120</DiskSize>
                <DiskId>d-bp1hwhh5o7ctrr4w****</DiskId>
            </Disk>
        </DiskList>
        <SupportIpV6>false</SupportIpV6>
        <InstanceType>ecs.g6.xlarge</InstanceType>
        <HostName>emr-worker-2</HostName>
    </Host>
    <Host>
        <Status>RELEASED</Status>
        <ZoneId>cn-hangzhou-i</ZoneId>
        <PublicIp>118.**.**.**</PublicIp>
        <Memory>16</Memory>
        <CreateTime>1599635156000</CreateTime>
        <Cpu>4</Cpu>
        <HostInstanceId>i-bp19n1ipsgm5s2sa****</HostInstanceId>
        <SerialNumber>1b513c42-520f-4f2c-b72d-5c3aa90e5707</SerialNumber>
        <PrivateIp>192.**.**.**</PrivateIp>
        <ChargeType>PostPaid</ChargeType>
        <ExpiredTime>1600229040000</ExpiredTime>
        <DiskList>
            <Disk>
                <DiskType>CLOUD_ESSD</DiskType>
                <Type>data</Type>
                <Device>/dev/xvdb</Device>
                <DiskSize>80</DiskSize>
                <DiskId>d-bp1f2uxmm5grkjut****</DiskId>
            </Disk>
            <Disk>
                <DiskType>CLOUD_ESSD</DiskType>
                <Type>system</Type>
                <Device>/dev/xvda</Device>
                <DiskSize>120</DiskSize>
                <DiskId>d-bp17ku663b9lp7ew****</DiskId>
            </Disk>
        </DiskList>
        <SupportIpV6>false</SupportIpV6>
        <InstanceType>ecs.g6.xlarge</InstanceType>
        <HostName>emr-header-1</HostName>
    </Host>
    <Host>
        <Status>NORMAL</Status>
        <ZoneId>cn-hangzhou-i</ZoneId>
        <PublicIp/>
        <Memory>16</Memory>
        <CreateTime>1599635155000</CreateTime>
        <Cpu>4</Cpu>
        <HostInstanceId>i-bp1c0gr9i4an9xyr****</HostInstanceId>
        <Role>CORE</Role>
        <SerialNumber>ff2215b2-f518-4f1d-aa52-e99759f67778</SerialNumber>
        <PrivateIp>192.**.**.**</PrivateIp>
        <ChargeType>PostPaid</ChargeType>
        <ExpiredTime>32493801600000</ExpiredTime>
        <DiskList>
            <Disk>
                <DiskType>CLOUD_ESSD</DiskType>
                <Type>data</Type>
                <Device>/dev/xvde</Device>
                <DiskSize>80</DiskSize>
                <DiskId>d-bp1df0bh75y58zrk****</DiskId>
            </Disk>
            <Disk>
                <DiskType>CLOUD_ESSD</DiskType>
                <Type>data</Type>
                <Device>/dev/xvdd</Device>
                <DiskSize>80</DiskSize>
                <DiskId>d-bp11vuvirj8qq9nm****</DiskId>
            </Disk>
            <Disk>
                <DiskType>CLOUD_ESSD</DiskType>
                <Type>data</Type>
                <Device>/dev/xvdc</Device>
                <DiskSize>80</DiskSize>
                <DiskId>d-bp172aj8y4g8ni9f****</DiskId>
            </Disk>
            <Disk>
                <DiskType>CLOUD_ESSD</DiskType>
                <Type>data</Type>
                <Device>/dev/xvdb</Device>
                <DiskSize>80</DiskSize>
                <DiskId>d-bp18zvy13uv36umu****</DiskId>
            </Disk>
            <Disk>
                <DiskType>CLOUD_ESSD</DiskType>
                <Type>system</Type>
                <Device>/dev/xvda</Device>
                <DiskSize>120</DiskSize>
                <DiskId>d-bp14wvtlcfpswp4t****</DiskId>
            </Disk>
        </DiskList>
        <SupportIpV6>false</SupportIpV6>
        <InstanceType>ecs.g6.xlarge</InstanceType>
        <HostName>emr-worker-1</HostName>
    </Host>
</HostList>
```

`JSON` 格式

```
{
	"RequestId": "50F7151C-915D-4576-A291-833E8D193853",
	"PageSize": 10,
	"PageNumber": 1,
	"Total": 3,
	"HostList": {
		"Host": [
			{
				"Status": "NORMAL",
				"ZoneId": "cn-hangzhou-i",
				"PublicIp": "",
				"Memory": 16,
				"CreateTime": 1599635156000,
				"Cpu": 4,
				"HostInstanceId": "i-bp1bqta9j2d7x7tc****",
				"Role": "CORE",
				"SerialNumber": "bfad582f-a6bc-40b7-868e-60940b0be9ae",
				"PrivateIp": "192.**.**.**",
				"ChargeType": "PostPaid",
				"ExpiredTime": 32493801600000,
				"DiskList": {
					"Disk": [
						{
							"DiskType": "CLOUD_ESSD",
							"Type": "data",
							"Device": "/dev/xvde",
							"DiskSize": 80,
							"DiskId": "d-bp17scdtdsb4r5qx****"
						},
						{
							"DiskType": "CLOUD_ESSD",
							"Type": "data",
							"Device": "/dev/xvdd",
							"DiskSize": 80,
							"DiskId": "d-bp1620snh1vzd8bo****"
						},
						{
							"DiskType": "CLOUD_ESSD",
							"Type": "data",
							"Device": "/dev/xvdc",
							"DiskSize": 80,
							"DiskId": "d-bp1i3puwiaha37ru****"
						},
						{
							"DiskType": "CLOUD_ESSD",
							"Type": "data",
							"Device": "/dev/xvdb",
							"DiskSize": 80,
							"DiskId": "d-bp14h6m9spidesdt****"
						},
						{
							"DiskType": "CLOUD_ESSD",
							"Type": "system",
							"Device": "/dev/xvda",
							"DiskSize": 120,
							"DiskId": "d-bp1hwhh5o7ctrr4w****"
						}
					]
				},
				"SupportIpV6": false,
				"InstanceType": "ecs.g6.xlarge",
				"HostName": "emr-worker-2"
			},
			{
				"Status": "RELEASED",
				"ZoneId": "cn-hangzhou-i",
				"PublicIp": "118.**.**.**",
				"Memory": 16,
				"CreateTime": 1599635156000,
				"Cpu": 4,
				"HostInstanceId": "i-bp19n1ipsgm5s2sa****",
				"SerialNumber": "1b513c42-520f-4f2c-b72d-5c3aa90e5707",
				"PrivateIp": "192.**.**.**",
				"ChargeType": "PostPaid",
				"ExpiredTime": 1600229040000,
				"DiskList": {
					"Disk": [
						{
							"DiskType": "CLOUD_ESSD",
							"Type": "data",
							"Device": "/dev/xvdb",
							"DiskSize": 80,
							"DiskId": "d-bp1f2uxmm5grkjut****"
						},
						{
							"DiskType": "CLOUD_ESSD",
							"Type": "system",
							"Device": "/dev/xvda",
							"DiskSize": 120,
							"DiskId": "d-bp17ku663b9lp7ew****"
						}
					]
				},
				"SupportIpV6": false,
				"InstanceType": "ecs.g6.xlarge",
				"HostName": "emr-header-1"
			},
			{
				"Status": "NORMAL",
				"ZoneId": "cn-hangzhou-i",
				"PublicIp": "",
				"Memory": 16,
				"CreateTime": 1599635155000,
				"Cpu": 4,
				"HostInstanceId": "i-bp1c0gr9i4an9xyr****",
				"Role": "CORE",
				"SerialNumber": "ff2215b2-f518-4f1d-aa52-e99759f67778",
				"PrivateIp": "192.**.**.**",
				"ChargeType": "PostPaid",
				"ExpiredTime": 32493801600000,
				"DiskList": {
					"Disk": [
						{
							"DiskType": "CLOUD_ESSD",
							"Type": "data",
							"Device": "/dev/xvde",
							"DiskSize": 80,
							"DiskId": "d-bp1df0bh75y58zrk****"
						},
						{
							"DiskType": "CLOUD_ESSD",
							"Type": "data",
							"Device": "/dev/xvdd",
							"DiskSize": 80,
							"DiskId": "d-bp11vuvirj8qq9nm****"
						},
						{
							"DiskType": "CLOUD_ESSD",
							"Type": "data",
							"Device": "/dev/xvdc",
							"DiskSize": 80,
							"DiskId": "d-bp172aj8y4g8ni9f****"
						},
						{
							"DiskType": "CLOUD_ESSD",
							"Type": "data",
							"Device": "/dev/xvdb",
							"DiskSize": 80,
							"DiskId": "d-bp18zvy13uv36umu****"
						},
						{
							"DiskType": "CLOUD_ESSD",
							"Type": "system",
							"Device": "/dev/xvda",
							"DiskSize": 120,
							"DiskId": "d-bp14wvtlcfpswp4t****"
						}
					]
				},
				"SupportIpV6": false,
				"InstanceType": "ecs.g6.xlarge",
				"HostName": "emr-worker-1"
			}
		]
	}
}
```

