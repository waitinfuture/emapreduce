# 通过RAM角色进行EMR集群的OSS数据权限隔离

本文介绍如何创建RAM角色，以及如何在E-MapReduce中使用自定义的角色进行OSS数据权限隔离。

通过创建RAM角色，方便您通过访问MetaService服务时获取到的AccessKey，都是对应这个角色权限的，从而可以实现以下限制：

-   指定集群只能访问指定OSS的数据目录。
-   指定集群访问指定的外部资源。

访问MetaService详情请参见[基于MetaService免AccessKey访问阿里云资源](/intl.zh-CN/集群管理/集群配置/基于MetaService免AccessKey访问阿里云资源.md)。

## 创建RAM角色

1.  使用云账号登录[RAM控制台](https://ram.console.aliyun.com/)。

2.  创建一个授权策略。

    1.  在左侧导航栏，单击**权限管理** \> **权限策略管理**。

    2.  单击**创建权限策略**。

    3.  填写**策略名称**，选择**脚本配置**，输入策略内容。

        ![创建授权策略](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/8340659951/p96074.png)

        本示例策略内容如下。

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

        **说明：**

        -   `Action`表示OS可以使用的读取和查询目录的权限。
        -   `Resource`表示您可以访问的OSS bucket资源，本示例中为`bigdata001`。
    4.  单击**确定**。

3.  创建RAM角色。

    1.  在左侧导航栏中单击**RAM角色管理**。

    2.  单击**创建 RAM 角色**。

    3.  选择**阿里云服务**，单击**下一步**。

    4.  设置角色名称，选择授信服务**云服务器**。

        ![创建RAM角色](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/8340659951/p96079.png)

        **说明：** 创建RAM角色时选择云服务器，后面再修改RAM角色的可信实体为EMR云服务。

    5.  单击**完成**。

4.  修改授信服务。

    1.  在**RAM角色管理**页面，单击刚创建的**RAM角色名称**。

    2.  单击**信任策略管理**页签。

    3.  单击**修改信任策略**。

    4.  修改`ecs.aliyuncs.com`为`emr.aliyuncs.com`。

        ![修改授信服务](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/8340659951/p96084.png)

    5.  单击**确定**。

5.  添加相应权限。

    1.  单击**权限管理**页签。

    2.  单击**添加权限**，选中**自定义策略**，添加相应的权限。

    3.  单击**确定**。

    4.  单击**完成**。


## 使用自定义角色

目前在E-MapReduce控制台上，还不支持直接配置自定义的角色，需要使用SDK，可以参照以下方式来指定和创建集群。

-   Java版本

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
        if err != nil {
            fmt.Print(err.Error())
        }
        fmt.Printf("response is %#v\n", response)
    }
    ```

-   Python版本

    ```
    #!/usr/bin/env python
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

-   Go版本

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
        if err != nil {
            fmt.Print(err.Error())
        }
        fmt.Printf("response is %#v\n", response)
    }
    ```


