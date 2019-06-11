# ListClusterHost {#concept_fgn_wmq_dgb .concept}

You can call this operation to view the information of cluster hosts including configurations of disks, vCPUs, and memory.

## Request parameters {#section_pdw_lv4_dgb .section}

|Parameter|Type|Required|Example|Description|
|---------|----|--------|-------|-----------|
|ClusterId|String|Yes|C-D7CA98AAA96A\*\*\*\*|The ID of the cluster.|
|RegionId|String|Yes|cn-hangzhou|The region ID.|
|AccessKeyId|String|Yes|LTAI8ljWyu7y\*\*\*\*|The AccessKey ID.|
|ComponentName|String|No|HiveServer2|The name of the component.|
|GroupType|String|No|MASTER|The type of the host group.|
|HostInstanceId|String|No|i-bp11vdyh3l6xvmnl\*\*\*\*|The ID of the ECS instance.|
|HostName|String|No|emr-header-1|The name of the instance.|
|PageNumber|Integer|No|1|The page number. The minimum value is 1.|
|PageSize|Integer|No|10|The number of records that are displayed on each page.|
|PrivateIp|String|No|192.168.25.220|The internal IP address of the host.|
|PublicIp|String|No|47.110.77.16|The public IP address of the host.|
|StatusList.N|RepeatList|No|\["NORMAL"\]|The status of the instances.|

## Response parameters {#section_vdw_lv4_dgb .section}

|Parameter|Type|Example|Description|
|---------|----|-------|-----------|
|RequestId|String|0A46EB54-0D98-4BFA-B712-1A474925E9D4|The ID of the request.|
|PageNumber|Integer|1|The page number. The minimum value is 1.|
|PageSize|Integer|10|The number of records that are displayed on each page.|
|Total|Integer|12|The total number of records.|
|HostList| | |The list of hosts.|
|HostName|String|emr-header-1|The name of the host.|
|PublicIp|String|47.110.77.16|The public IP address of the host.|
|PrivateIp|String|192.168.25.220|The internal IP address of the host.|
|Role|String|MASTER|The role of the host in the cluster.|
|InstanceType|String|ecs.mn4.xlarge|The instance type of the host.|
|Cpu|Integer|4|The number of vCPUs.|
|Memory|Integer|16|The memory size. Unit: gibibytes.|
|Status|String|NORMAL|The status of the host.|
|Type|String|VM|The type of the host.|
|HostInstanceId|String|i-bp1cfwf2cwgji7ds\*\*\*\*|The instance ID of the host.|
|SerialNumber|String|3c2a5078-778d-4c18-87e4-1a38fbcbc306|The serial number of the host.|
|ChargeType|String|PostPaid|The billing method.|
|ExpiredTime|Long|32493801600000|The expiration time of the host.|
|DiskList| | |The list of disks.|
|DiskId|String|d-bp1aq78lhbig6ielbur8|The ID of the disk.|
|Type|String|data|The role of the disk.|
|DiskType|String|CLOUD\_EFFICIENCY|The type of the disk.|
|DiskSize|Integer|80|The capacity of the disk.|

## Examples {#section_ydw_lv4_dgb .section}

-   Sample requests

    ``` {#codeblock_quf_suz_da3}
    /? ClusterId=C-D7CA98AAA96A7998
    &RegionId=cn-hangzhou
    &AccessKeyId=LTAI8ljWyu7y****
    &ComponentName=
    &GroupType=
    &HostInstanceId=
    &HostName=emr-header-1
    &PageNumber=1
    &PageSize=10
    &PrivateIp=192.168.25.220
    &PublicIp=47.110.77.16
    &StatusList. 1=["NORMAL"]
    &<Common request parameters>
    ```

-   Successful response examples

    JSON format

    ``` {#codeblock_lmb_kty_3ah}
    {
        "hostList":[
            {
                "chargeType":"PostPaid",
                "cpu":4,
                "diskList":[
                    {
                        "diskId":"d-bp1d2duao4slg805dmg8",
                        "diskSize":80,
                        "diskType":"CLOUD_EFFICIENCY",
                        "type":"data"
                    },
                    {
                        "diskId":"d-bp1974rijbh940n5ih4q",
                        "diskSize":120,
                        "diskType":"CLOUD_EFFICIENCY",
                        "type":"system"
                    }
                ],
                "expiredTime":32493801600000,
                "hostInstanceId":"i-bp1cfwf2cwgji7ds****",
                "hostName":"emr-header-1",
                "instanceType":"ecs.mn4.xlarge",
                "memory":16,
                "privateIp":"192.168.25.220",
                "publicIp":"47.110.77.16",
                "role":"MASTER",
                "serialNumber":"3c2a5078-778d-4c18-87e4-1a38fbcbc306",
                "status":"NORMAL"
            },
            {
                "chargeType":"PostPaid",
                "cpu":4,
                "diskList":[
                    {
                        "diskId":"d-bp1aq78lhbig6ielbur8",
                        "diskSize":80,
                        "diskType":"CLOUD_EFFICIENCY",
                        "type":"data"
                    },
                    {
                        "diskId":"d-bp1iply35zhdge3nbqxf",
                        "diskSize":80,
                        "diskType":"CLOUD_EFFICIENCY",
                        "type":"data"
                    },
                    {
                        "diskId":"d-bp19gcyyon6nx2lahx9x",
                        "diskSize":80,
                        "diskType":"CLOUD_EFFICIENCY",
                        "type":"data"
                    },
                    {
                        "diskId":"d-bp1974rijbh940n5ih4r",
                        "diskSize":80,
                        "diskType":"CLOUD_EFFICIENCY",
                        "type":"data"
                    },
                    {
                        "diskId":"d-bp1558hztv9c3q3gl7nn",
                        "diskSize":80,
                        "diskType":"CLOUD_EFFICIENCY",
                        "type":"system"
                    }
                ],
                "expiredTime":32493801600000,
                "hostInstanceId":"i-bp18sb2w8oj9p6yf****",
                "hostName":"emr-worker-1",
                "instanceType":"ecs.n4.xlarge",
                "memory":8,
                "privateIp":"192.168.25.221",
                "publicIp":"",
                "role":"CORE",
                "serialNumber":"d4dbd488-8684-4742-b178-5565ef38c6f9",
                "status":"NORMAL"
            },
            {
                "chargeType":"PostPaid",
                "cpu":4,
                "diskList":[
                    {
                        "diskId":"d-bp19gcyyon6nx2lahx9y",
                        "diskSize":80,
                        "diskType":"CLOUD_EFFICIENCY",
                        "type":"data"
                    },
                    {
                        "diskId":"d-bp1314ka4c7qmhpadwhf",
                        "diskSize":80,
                        "diskType":"CLOUD_EFFICIENCY",
                        "type":"data"
                    },
                    {
                        "diskId":"d-bp1558hztv9c3q3gl7no",
                        "diskSize":80,
                        "diskType":"CLOUD_EFFICIENCY",
                        "type":"data"
                    },
                    {
                        "diskId":"d-bp1e82xlhob2n2e7q5rd",
                        "diskSize":80,
                        "diskType":"CLOUD_EFFICIENCY",
                        "type":"data"
                    },
                    {
                        "diskId":"d-bp1gpdc9x4o051udcmd3",
                        "diskSize":80,
                        "diskType":"CLOUD_EFFICIENCY",
                        "type":"system"
                    }
                ],
                "expiredTime":32493801600000,
                "hostInstanceId":"i-bp14rtl1gqupopgm****",
                "hostName":"emr-worker-2",
                "instanceType":"ecs.n4.xlarge",
                "memory":8,
                "privateIp":"192.168.25.222",
                "publicIp":"",
                "role":"CORE",
                "serialNumber":"e8d25d43-2df6-4054-9861-53d1c12d08cb",
                "status":"NORMAL"
            }
        ],
        "pageNumber":1,
        "pageSize":10,
        "requestId":"0A46EB54-0D98-4BFA-B712-1A474925E9D4",
        "total":3
    }
    ```

-   Error response examples

    JSON format

    ``` {#codeblock_td7_s2d_6s6}
    {
        "code":"RAM.Permission.NotAllow",
        "message":"It is not allow to execute this operation,please use RAM to authorize!",
        "requestId":"9AEDC439-1F63-491D-B8C6-9737C372BF3A",
        "successResponse":false
    }
    ```


## Error codes {#section_a2w_lv4_dgb .section}

