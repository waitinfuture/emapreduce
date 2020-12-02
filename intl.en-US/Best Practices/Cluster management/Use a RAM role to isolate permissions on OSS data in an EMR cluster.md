# Use a RAM role to isolate permissions on OSS data in an EMR cluster

This topic describes how to create a RAM role and how to use the RAM role to isolate permissions on OSS data in an E-MapReduce \(EMR\) cluster.

Multiple AccessKey pairs are obtained when you access Alibaba Cloud resources by using MetaService. After you create a RAM role, you can use this role to obtain the specific AccessKey pair. Limits:

-   A specified cluster can access only specified OSS data directories.
-   A specified cluster can access specified external resources.

For more information about how to access Alibaba Cloud resources by using MetaService, see [MetaService](/intl.en-US/Cluster Management/Configure clusters/MetaService.md).

## Create a RAM role

1.  Log on to the [RAM console](https://ram.console.aliyun.com/) by using an Alibaba Cloud account.

2.  Create an authorization policy.

    1.  In the left-side navigation pane, choose **Permissions** \> **Policies**.

    2.  On the Policies page, click **Create Policy**.

    3.  On the Create Custom Policy page, specify **Policy Name**, select **Script** for Configuration Mode, and enter the policy content.

        ![Create an authorization policy](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/4240693061/p96074.png)

        Policy content in this example:

        ```
        {
            "Version": "1",
            "Statement": [
                {
                    "Action": [
                        "oss:GetObject",
                        "oss:ListObjects"
                    ],
                    "Resource": [
                        "acs:oss:*:*:bigdata001",
                        "acs:oss:*:*:bigdata001/*"
                    ],
                    "Effect": "Allow"
                }
            ]
        }
        ```

        **Note:**

        -   `Action` indicates the permissions granted for you to read and query OSS directories.
        -   `Resource` indicates the OSS bucket resources that you can access, such as `bigdata001` in this example.
    4.  Click **OK**.

3.  Create a RAM role.

    1.  In the left-side navigation pane, click **RAM Roles**.

    2.  On the RAM Roles page, click **Create RAM Role**.

    3.  In the Create RAM Role pane, select **Alibaba Cloud Service** and click **Next**.

    4.  Specify RAM Role Name and select **Elastic Compute Service** from the Select Trusted Service drop-down list.

        ![Create a RAM role](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/4240693061/p96079.png)

        **Note:** Select ECS as a trusted service. Change the trusted service to EMR in subsequent operations.

    5.  Click **OK**.

4.  Change the trusted service.

    1.  On the **RAM Roles** page, click the name of the created RAM role.

    2.  Click the **Trust Policy Management** tab.

    3.  Click **Edit Trust Policy**.

    4.  Change `ecs.aliyuncs.com` to `emr.aliyuncs.com`.

        ![Change the trusted service](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/4240693061/p96084.png)

    5.  Click **OK**.

5.  Add specific permissions.

    1.  Click the **Permissions** tab.

    2.  Click **Add Permissions**. In the Add Permissions pane, click the **Custom Policy** tab and add specific permissions.

    3.  Click **OK**.

    4.  Click **Complete**.


## Use a custom role

The EMR console does not support the configuration of custom roles. You can use EMR SDK to specify a custom role and create a cluster.

-   SDK for Java

    ```
    package main
    
    import (
        "fmt"
          "github.com/aliyun/alibaba-cloud-sdk-go/services/emr"
    
        "github.com/aliyun/alibaba-cloud-sdk-go/sdk/requests"
    
    )
    
    func main() {
        client, err := emr.NewClientWithAccessKey("cn-huhehaote", "<accessKeyId>", "<accessSecret>")
    
        request := emr.CreateCreateClusterV2Request()
        request.Scheme = "https"
    
      request.Name = "test"
      request.EmrVer = "EMR-3.22.0"
      request.ClusterType = "HADOOP"
      request.HostGroup = &[]emr.CreateClusterV2HostGroup{
        {
          HostGroupType: "MASTER",
          NodeCount: "1",
          InstanceType: "ecs.g5.2xlarge",
          DiskType: "CLOUD_SSD",
          DiskCapacity: "80",
          DiskCount: "4",
          SysDiskType: "CLOUD_SSD",
          SysDiskCapacity: "80",
        },
        {
          HostGroupType: "CORE",
          NodeCount: "3",
          InstanceType: "ecs.g5.2xlarge",
          DiskType: "CLOUD_SSD",
          DiskCapacity: "80",
          DiskCount: "4",
          SysDiskType: "CLOUD_SSD",
          SysDiskCapacity: "80",
        },
      }
      request.ZoneId = "cn-huhehaote-a"
      request.SecurityGroupId = "sg-hp39gm8n17f3u0i****"
      request.IsOpenPublicIp = requests.NewBoolean(true)
      request.ChargeType = "PostPaid"
      request.VpcId = "vpc-hp32dbx98fhcjp9kr****"
      request.VSwitchId = "vsw-hp34pmwcw8i5was4s****"
      request.NetType = "vpc"
      request.UserDefinedEmrEcsRole = "EMRUserDefineRole"
      request.SshEnable = requests.NewBoolean(true)
      request.MasterPwd = "ABCtest1****"
    
        response, err := client.CreateClusterV2(request)
        if err ! = nil {
            fmt.Print(err.Error())
        }
        fmt.Printf("response is %#v\n", response)
    }
    ```

-   SDK for Python

    ```
    #! /usr/bin/env python
    #coding=utf-8
    
    from aliyunsdkcore.client import AcsClient
    from aliyunsdkcore.acs_exception.exceptions import ClientException
    from aliyunsdkcore.acs_exception.exceptions import ServerException
    from aliyunsdkemr.request.v20160408.CreateClusterV2Request import CreateClusterV2Request
    
    client = AcsClient('<accessKeyId>', '<accessSecret>', 'cn-huhehaote')
    
    request = CreateClusterV2Request()
    request.set_accept_format('json')
    
    request.set_Name("test")
    request.set_EmrVer("EMR-3.22.0")
    request.set_ClusterType("HADOOP")
    request.set_HostGroups([
      {
        "HostGroupType": "MASTER",
        "NodeCount": 1,
        "InstanceType": "ecs.g5.2xlarge",
        "DiskType": "CLOUD_SSD",
        "DiskCapacity": 80,
        "DiskCount": 4,
        "SysDiskType": "CLOUD_SSD",
        "SysDiskCapacity": 80
      },
      {
        "HostGroupType": "CORE",
        "NodeCount": 3,
        "InstanceType": "ecs.g5.2xlarge",
        "DiskType": "CLOUD_SSD",
        "DiskCapacity": 80,
        "DiskCount": 4,
        "SysDiskType": "CLOUD_SSD",
        "SysDiskCapacity": 80
      }
    ])
    request.set_ZoneId("cn-huhehaote-a")
    request.set_SecurityGroupId("sg-hp39gm8n17f3u0ii****")
    request.set_IsOpenPublicIp(True)
    request.set_ChargeType("PostPaid")
    request.set_VpcId("vpc-hp32dbx98fhcjp9kr****")
    request.set_VSwitchId("vsw-hp34pmwcw8i5was4s****")
    request.set_NetType("vpc")
    request.set_UserDefinedEmrEcsRole("EMRUserDefineRole")
    request.set_SshEnable(True)
    request.set_MasterPwd("ABCtest1****")
    
    response = client.do_action_with_exception(request)
    # python2:  print(response) 
    print(str(response, encoding='utf-8'))
    ```

-   SDK for Go

    ```
    package main
    
    import (
        "fmt"
          "github.com/aliyun/alibaba-cloud-sdk-go/services/emr"
    
        "github.com/aliyun/alibaba-cloud-sdk-go/sdk/requests"
    
    )
    
    func main() {
        client, err := emr.NewClientWithAccessKey("cn-huhehaote", "<accessKeyId>", "<accessSecret>")
    
        request := emr.CreateCreateClusterV2Request()
        request.Scheme = "https"
    
      request.Name = "test"
      request.EmrVer = "EMR-3.22.0"
      request.ClusterType = "HADOOP"
      request.HostGroup = &[]emr.CreateClusterV2HostGroup{
        {
          HostGroupType: "MASTER",
          NodeCount: "1",
          InstanceType: "ecs.g5.2xlarge",
          DiskType: "CLOUD_SSD",
          DiskCapacity: "80",
          DiskCount: "4",
          SysDiskType: "CLOUD_SSD",
          SysDiskCapacity: "80",
        },
        {
          HostGroupType: "CORE",
          NodeCount: "3",
          InstanceType: "ecs.g5.2xlarge",
          DiskType: "CLOUD_SSD",
          DiskCapacity: "80",
          DiskCount: "4",
          SysDiskType: "CLOUD_SSD",
          SysDiskCapacity: "80",
        },
      }
      request.ZoneId = "cn-huhehaote-a"
      request.SecurityGroupId = "sg-hp39gm8n17f3u0ii****"
      request.IsOpenPublicIp = requests.NewBoolean(true)
      request.ChargeType = "PostPaid"
      request.VpcId = "vpc-hp32dbx98fhcjp9kr****"
      request.VSwitchId = "vsw-hp34pmwcw8i5was4s****"
      request.NetType = "vpc"
      request.UserDefinedEmrEcsRole = "EMRUserDefineRole"
      request.SshEnable = requests.NewBoolean(true)
      request.MasterPwd = "ABCtest1234!"
    
        response, err := client.CreateClusterV2(request)
        if err ! = nil {
            fmt.Print(err.Error())
        }
        fmt.Printf("response is %#v\n", response)
    }
    ```


