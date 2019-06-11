# ResizeClusterV2 {#concept_klf_nfb_kfb .concept}

You can call this operation to scale out a cluster based on the configuration.

## Request parameters {#section_p5v_ffb_kfb .section}

|Parameter|Type|Required|Example|Description |
|---------|----|--------|-------|------------|
|ClusterId|String|Yes|C-D7958B72E59BAB88|The ID of the cluster.|
|RegionId|String|Yes|cn-hangzhou|The region ID.|
|AccessKeyId|String|Yes|LTAI8ljWyu7y\*\*\*\*|The AccessKey ID.|
|HostGroup.N.AutoRenew|Boolean|No|false|Indicates whether the host group is automatically renewed.|
|HostGroup.N.ChargeType|String|No|PostPaid|The billing method for the host group.|
|HostGroup.N.ClusterId|String|No|C-D7958B72E59BAB88|The ID of the cluster to be scaled out.|
|HostGroup.N.Comment|String|No|0|A reserved parameter.|
|HostGroup.N.CreateType|String|No|0|A reserved parameter.|
|HostGroup.N.DiskCapacity|Integer|No|120|The data disk capacity of the host group.|
|HostGroup.N.DiskCount|Integer|No|4|The data disk number of the host group.|
|HostGroup.N.DiskType|String|No|CLOUD\_SSD|The type of the data disk.|
|HostGroup.N.HostGroupId|String|No|G-48E83B43E97111BE|The ID of the host group used to scale out the cluster.|
|HostGroup.N.HostGroupName|String|No|Task Instance Group|The name of the host group.|
|HostGroup.N.HostGroupType|String|No|TASK|The type of the host group.|
|HostGroup.N.HostKeyPairName|String|No|test-pair|The key pair name of the host group. Currently, only gateways are supported.|
|HostGroup.N.HostPassword|String|No|pwd|The password of the host. Currently, only gateways are supported.|
|HostGroup.N.InstanceType|String|No|ecs.mn4.2xlarge|The instance type of the host group.|
|HostGroup.N.NodeCount|Integer|No|1|The number of nodes in the host group.|
|HostGroup.N.Period|Integer|No|1|The length of the subscription. Unit: months. Valid values: 1, 2, 3, 4, 5, 6, 7, 8, 9, 12, 24, and 36.|
|HostGroup.N.SysDiskCapacity|Integer|No|120|The capacity of the system disk.|
|HostGroup.N.SysDiskType|String|No|SSD\_CLOUD|The type of the system disk.|
|HostGroup.N.VswitchId|Integer|No|0|The ID of the VSwitch in the host group.|
|VswitchId|String|No|vsw-bp10tvjyc77psy0z5h0ni|The ID of the VSwitch.|

## Response parameters {#section_vdw_lv4_dgb .section}

|Parameter|Type|Example|Description |
|---------|----|-------|------------|
|RequestId|String|BF4FBAC6-B03E-4BFB-B6DB-EB53C34F2E22|The ID of the request.|
|ClusterId|String|C-D7958B72E59BAB88|The ID of the cluster.|

## Examples {#section_ydw_lv4_dgb .section}

-   Sample requests

    ```
    /? ClusterId=C-D7958B72E59BAB88
    &RegionId=cn-hangzhou 
    &AccessKeyId=LTAI8ljWyu7y****
    &VswitchId=vsw-bp10tvjyc77psy0z5h0ni
    &HostGroup. 1.AutoRenew=
    &HostGroup. 1.ChargeType=PostPaid
    &HostGroup. 1.ClusterId=
    &HostGroup. 1.Comment=
    &HostGroup. 1.CreateType=
    &HostGroup. 1.DiskCapacity=120
    &HostGroup. 1.DiskCount=4
    &HostGroup. 1.DiskType=CLOUD_SSD
    &HostGroup. 1.HostGroupId=
    &HostGroup. 1.HostGroupName=Task instance group
    &HostGroup. 1.HostGroupType=TASK
    &HostGroup. 1.HostKeyPairName=
    &HostGroup. 1.HostPassword=
    &HostGroup. 1.InstanceType=ecs.mn4.2xlarge
    &HostGroup. 1.1odeCount=1
    &HostGroup. 1.Period=1
    &HostGroup. 1.SysDiskCapacity=
    &HostGroup. 1.SysDiskType=
    &HostGroup. 1.VswitchId=
    &<Common request parameters>
    ```

-   Successful response examples

    JSON format

    ```
    {
    	"ClusterId":"C-D7958B72E59BAB88",
    	"RequestId":"BF4FBAC6-B03E-4BFB-B6DB-EB53C34F2E22"
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

