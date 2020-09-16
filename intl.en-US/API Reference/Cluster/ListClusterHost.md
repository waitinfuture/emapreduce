# ListClusterHost

You can call this operation to ListClusterHost a cluster, including disks, CPU, and memory configurations.

## Debugging

[OpenAPI Explorer automatically calculates the signature value. For your convenience, we recommend that you call this operation in OpenAPI Explorer. OpenAPI Explorer dynamically generates the sample code of the operation for different SDKs.](https://api.aliyun.com/#product=Emr&api=ListClusterHost&type=RPC&version=2016-04-08)

## Request parameters

|Parameter|Type|Required|Example|Description|
|---------|----|--------|-------|-----------|
|Action|String|Yes|ListClusterHost|The operation that you want to perform. For API requests using the HTTP or HTTPS URL, this parameter is required. Set the value to ListClusterHost. |
|ClusterId|String|Yes|C-D7CA98AAA96A\*\*\*\*|The ID of the ApsaraDB for POLARDB cluster. |
|RegionId|String|Yes|cn-hangzhou|The ID of the region. |
|HostInstanceId|String|No|i-bp11vdyh3l6xvmnl\*\*\*\*|The ID of the Elastic Compute Service \(ECS\) instance. |
|HostGroupId|String|No|G-A5EA210E15FC\*\*\*\*|The ID of the host group. |
|HostName|String|No|emr-header-1|The hostname of the created ECS instances. |
|PrivateIp|String|No|192.\*\*\*. \*\*\*. \*\*\*|The internal IP address of the host. |
|PublicIp|String|No|47.\*\*\*. \*\*\*. \*\*\*|The public IP address of the host. |
|GroupType|String|No|MASTER|Machine group type:

-   MASTER
-   CORE |
|ComponentName|String|No|HiveServer2|The name of the component. |
|StatusList.N|RepeatList|No|\["NORMAL"\]|Host status list:

-   NORMAL
-   ABNORMAL
-   RESIZING
-   INITIALIZING
-   RELEASED |
|PageNumber|Integer|No|1|The number of the page to return. Pages start from page 1. |
|PageSize|Integer|No|10|The number of entries returned per page. |

## Response parameters

|Parameter|Type|Example|Description|
|---------|----|-------|-----------|
|HostList|Array of Host| |The information about the hosts. |
|Host| | | |
|ChargeType|String|PostPaid|The billing method of the host. |
|Cpu|Integer|4|The number of vCPUs. |
|CreateTime|String|1599635156000|The time when the disk was created. |
|DiskList|Array of Disk| |The list of disks. |
|Disk| | | |
|Device|String|/dev/xvde|The name of the disk. |
|DiskId|String|d-bp1aq78lhbig6iel\*\*\*\*|The ID of the disk. |
|DiskSize|Integer|80|The disk capacity. |
|DiskType|String|CLOUD\_ESSD|The type of disks. Valid values:

-   CLOUD\_ESSD
-   CLOUD\_SSD
-   CLOUD
-   CLOUD\_EFFCIENCY |
|Type|String|data|Specifies whether the disk is a data disk or system disk. |
|EmrExpiredTime|String|N/A|The time when the EMR cluster expires. A reserved field. |
|ExpiredTime|Long|32493801600000|The time when the host expires. |
|HostGroupId|String|G-A5EA210E15FC\*\*\*\*|The ID of the host group. |
|HostInstanceId|String|i-bp1cfwf2cwgji7ds\*\*\*\*|The ID of the ECS instance. |
|HostName|String|emr-header-1|The hostname of the created ECS instances. |
|InstanceStatus|String|NORMAL|The status of the ECS instance. |
|InstanceType|String|ecs.mn4.xlarge|The instance type of the host. |
|Memory|Integer|16|The memory size of the host. Unit: GB. |
|PrivateIp|String|192.\*\*\*. \*\*\*\*. \*\*\*|The internal IP address of the host. |
|PublicIp|String|47.\*\*\*. \*\*\*. \*\*\*|The public IP address of the host. |
|Role|String|MASTER|The role of the host in the cluster.

-   MASTER
-   CORE |
|SerialNumber|String|3c2a5078-778d-4c18-87e4-1a38fbcb\*\*\*\*|The serial number of the host. |
|Status|String|NORMAL|The status of the ECS instance. |
|SupportIpV6|Boolean|false|Specifies whether the IPv6 protocol is supported. |
|Type|String|VM|The type of the host. |
|ZoneId|String|cn-hangzhou-i|The ID of the region where your project resides. |
|PageNumber|Integer|1|The page number of the returned page. |
|PageSize|Integer|10|The maximum number of exceptions on each page. |
|RequestId|String|50F7151C-915D-4576-A291-833E8D193853|The ID of the instance. |
|Total|Integer|12|The total number of hosts that you have queried. |

## Examples

Sample requests

```
http(s)://[Endpoint]/? Action=ListClusterHost
&ClusterId=C-D7CA98AAA96A****
&RegionId=cn-hangzhou
&<Common request parameters>
```

Sample success responses

`XML` format

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
        <PrivateIp>192. **. **. **</PrivateIp>
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
        <PublicIp>118. **. **. **</PublicIp>
        <Memory>16</Memory>
        <CreateTime>1599635156000</CreateTime>
        <Cpu>4</Cpu>
        <HostInstanceId>i-bp19n1ipsgm5s2sa****</HostInstanceId>
        <SerialNumber>1b513c42-520f-4f2c-b72d-5c3aa90e5707</SerialNumber>
        <PrivateIp>192. **. **. **</PrivateIp>
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
        <PrivateIp>192. **. **. **</PrivateIp>
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

`JSON` format

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
                "PrivateIp": "192. **. **. **",
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
                "PublicIp": "118. **. **. **",
                "Memory": 16,
                "CreateTime": 1599635156000,
                "Cpu": 4,
                "HostInstanceId": "i-bp19n1ipsgm5s2sa****",
                "SerialNumber": "1b513c42-520f-4f2c-b72d-5c3aa90e5707",
                "PrivateIp": "192. **. **. **",
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
                "PrivateIp": "192. **. **. **",
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

## Error codes

|HttpCode|Error codes|Error message|Description|
|--------|-----------|-------------|-----------|
|403|Params.Illegal|The specified parameters are wrongly formed..|The error message returned because the format of the specified parameters is invalid.|
|403|User.OtherUserResource.NotAllow|It is not allowed to operate other user's resource|The error message returned because you are not authorized to manage the resources of other users.|
|403|Invalid.Cluster.Status|Invalid cluster status %s in status list|The error message returned because the specified cluster status is invalid.|
|403|Invalid.Cluster.Type|Invalid cluster type %s in cluster type list|The error message returned because the type of the specified cluster is invalid.|
|500|InternalError|The request processing has failed due to some unknown error.|The error message returned because the request processing has failed due to an internal error. Submit a ticket.|

For a list of error codes, visit the [API Error Center](https://error-center.alibabacloud.com/status/product/Emr).

