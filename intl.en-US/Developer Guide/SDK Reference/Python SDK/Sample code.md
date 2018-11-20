# Sample code {#concept_v1l_wz3_kfb .concept}

Python sample code

## Query the cluster list {#section_z1s_k1j_kfb .section}

```
#! /usr/bin/python
from aliyunsdkcore import client
from aliyunsdkemr.request.v20160408 import ListClustersRequest
clt = client.AcsClient('SFAW************','Nc2nZ6dQoiqck0*************','cn-hangzhou') # set acessId and accessKey
request = ListClustersRequest.ListClustersRequest()
request.set_accept_format('xml') # xml or json
# You can set filter conditions to obtain clusters in the RUNNING or IDLE state. The state parameter is optional. You can choose not to specify this parameter.
request.add_query_param('StatusList. 1', 'RUNNING')
request.add_query_param('StatusList. 2', 'IDLE')
result = clt.do_action(request)
print result
```

## Create a cluster {#section_sph_m1j_kfb .section}

```
#! /usr/bin/env python
from aliyunsdkcore import client
from aliyunsdkemr.request.v20160408 import CreateClusterRequest
clt = client.AcsClient('SFAW************','Nc2nZ6dQoiqck0*************','cn-hangzhou') # set acessId and accessKey
request = CreateClusterRequest.CreateClusterRequest()
request.set_Name("pydemo")
request.set_ZoneId("cn-hangzhou-b")
request.set_LogEnable(False)
request.set_SecurityGroupId("sg-********")
request.set_IsOpenPublicIp(True)
request.set_ChargeType("PostPaid")
request.set_EmrVer("EMR-1.3.0")
request.set_ClusterType("HADOOP")
request.set_IoOptimized(True)
request.set_InstanceGeneration("ecs-2")
# set EcsOrder
request.add_query_param('EcsOrder. 1. NodeCount', '1')
request.add_query_param('EcsOrder. 1. NodeType', 'MASTER')
request.add_query_param('EcsOrder. 1. InstanceType', 'ecs.n1.large')
request.add_query_param('EcsOrder. 1. DiskType', 'CLOUD_EFFICIENCY')
request.add_query_param('EcsOrder. 1. DiskCapacity', '80')
request.add_query_param('EcsOrder. 1. DiskCount', '1')
request.add_query_param('EcsOrder. 1. Index', '1')
request.add_query_param('EcsOrder. 2. NodeCount', '3')
request.add_query_param('EcsOrder. 2. NodeType', 'CORE')
request.add_query_param('EcsOrder. 2. InstanceType', 'ecs.n1.large')
request.add_query_param('EcsOrder. 2. DiskType', 'CLOUD_EFFICIENCY')
request.add_query_param('EcsOrder. 2. DiskCapacity', '80')
request.add_query_param('EcsOrder. 2. DiskCount', '4')
request.add_query_param('EcsOrder. 2. Index', '2')
request.set_accept_format('json')
result = clt.do_action(request)
print result
```

**Note:** All SDKs are generated automatically for various Alibaba Cloud services. You must modify the original code of some SDKs to apply to specific use cases. For example, in the current Python SDK, you must refer to the previous sample code when you use a List as a request parameter. If you specify a List with values of basic data types for request parameters, see configurations of the StatusList parameter in Query the list of clusters. If you specify a List with objects for request parameters, see configurations of the EcsOrder parameter in the Create a cluster section. For Python APIs that use a List as a request parameter such as BootstrapAction, you can refer to the same places described in the preceding sentence according to a List type. We recommend that you use the Java SDK in which a List can be used properly.

For more information, see [API overview](EN-US_TP_18000.dita#concept_cyb_sx1_kfb).

