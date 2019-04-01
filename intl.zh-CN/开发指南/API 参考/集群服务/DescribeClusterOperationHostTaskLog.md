# DescribeClusterOperationHostTaskLog {#concept_ewt_zgf_2gb .concept}

集群操作历史中，指定主机上的指定 Task 的执行日志详情。

## 请求参数 {#section_p5v_ffb_kfb .section}

|参数|类型|是否必选|示例值|描述|
|--|--|----|---|--|
|ClusterId|String|是|C-0E995C0EE7E5ECB3|集群 ID|
|HostId|String|是|41008|待查询日志的主机 ID，可从 ListClusterOperationHost 接口的返回值中获取|
|OperationId|String|是|11123|操作历史的 ID，可从ListClusterOperation 接口的返回值中获取|
|TaskId|String|是|1098803|待查询的 task 的ID 信息，可从ListClusterOperationHostTask 接口的返回中获取|
|Status|String|否|SUCCESS|task的状态信息，非必选|
|RegionId|String|是|cn-hangzhou|对应的 regionId 信息|
|AccessKeyId|String|否|LTAI8ljWyu7y\*\*\*\*|账号的阿里云 AccessKeyId 信息|

## 返回参数 {#section_vdw_lv4_dgb .section}

|字段|类型|示例值|描述|
|--|--|---|--|
|RequestId|String|A544317F-4A60-4532-AC96-191B9D80420AF|请求 ID|
|Stdout|String|2018-11-27 14:21:59========================================================================================================== touch -c /var/lib/ecm-agent/cache/ecm/service/ILOGTAIL/1.0.0.0.1-df/package/scripts/IlogtaildComponentOperator.py touch -c /var/lib/ecm-agent/cache/ecm/service/ILOGTAIL/1.0.0.0.1-df/package/scripts/IlogtaildConfigService.py root 2844 0.0 0.0 49244 2380 ? Ss 14:20 0:00 /usr/local/ilogtail/ilogtail root 2846 0.5 0.2 231416 18352 ? Sl 14:20 0:00 /usr/local/ilogtail/ilogtail Command completed successfully!|任务的 stdout 日志信息|
|Stderr|String|2018-11-27 14:21:59========================================================================================================== Tue, 27 Nov 2018 14:21:59 IlogtaildComponentOperator.py\[line:140\] INFO ilogtail has been started, return|任务的 stderr 日志信息|

## 示例 {#section_ydw_lv4_dgb .section}

-   请求示例

    ```
    /?ClusterId=C-F32FB31D82954C64
    &HostId=41008
    &OperationId=11123
    &RegionId=cn-hangzhou
    &TaskId=1098803
    &AccessKeyId=LTAI8ljWyu7y****
    &Status=SUCCESS
    &<公共请求参数>
    ```

-   正常返回示例

    JSON 格式

    ```
    {
    	"requestId":"xxx",
    	"stderr":"2018-11-27 14:21:59========================================================================================================== Tue, 27 Nov 2018 14:21:59 IlogtaildComponentOperator.py[line:140] INFO ilogtail has been started, return",
    	"stdout":"2018-11-27 14:21:59========================================================================================================== touch -c /var/lib/ecm-agent/cache/ecm/service/ILOGTAIL/1.0.0.0.1-df/package/scripts/IlogtaildComponentOperator.py touch -c /var/lib/ecm-agent/cache/ecm/service/ILOGTAIL/1.0.0.0.1-df/package/scripts/IlogtaildConfigService.py root 2844 0.0 0.0 49244 2380 ? Ss 14:20 0:00 /usr/local/ilogtail/ilogtail root 2846 0.5 0.2 231416 18352 ? Sl 14:20 0:00 /usr/local/ilogtail/ilogtail   Command completed successfully!"
    }
    ```

-   异常返回示例

    JSON 格式

    ```
    {
    	"code":"RAM.Permission.NotAllow",
    	"message":"It is not allow to execute this operation,please use RAM to authorize!",
    	"requestId":"9AEDC439-1F63-491D-B8C6-9737C372BF3A",
    	"successResponse":false
    }
    ```


## 错误码 {#section_a2w_lv4_dgb .section}

[查看本产品错误码](https://error-center.alibabacloud.com/status/product/Emr)

