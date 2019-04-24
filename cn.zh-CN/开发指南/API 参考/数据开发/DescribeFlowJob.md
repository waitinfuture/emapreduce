# DescribeFlowJob {#concept_xxy_ylw_fgb .concept}

调用 DescribeFlowJob 接口查询作业信息

## 请求参数 {#section_obj_yxl_2gb .section}

|参数|类型|是否必选|示例值|描述|
|--|--|----|---|--|
|Id|String|是|FJ-BCCAE48B90CCB37B|作业 ID|
|ProjectId|String|是|FP-257A173659F59685|项目 ID|
|RegionId|String|是|cn-hangzhou|区域 ID|

## 返回参数 {#section_vbj_yxl_2gb .section}

|字段|类型|示例值|描述|
|--|--|---|--|
|RequestId|String|1549175a-6d14-4c8a-89f9-5e28300f6d7e|请求 ID|
|Id|String|FJ-BCCAE48B90CCB37B|作业 ID|
|GmtCreate|Long|1538017814000|创建时间|
|GmtModified|Long|1538017814000|修改时间|
|Name|String|Name|作业名称|
|Type|String|SHELL|作业类型，目前支持：MR、SPARK、HIVE\_SQL、HIVE、PIG、SQOOP、SPARK\_SQL、SPARK\_STREAMING、SHELL|
|Description|String|这是一个数据开发作业描述|作业的描述|
|FailAct|String|CONTINUE|失败策略，支持：CONTINUE（跳过）、STOP（停止工作流）|
|MaxRetry|Integer|5|最大重试次数，0 - 5|
|RetryInterval|Long|200|重试间隔 0-300\(秒\)|
|Params|String|ls -l|作业内容|
|ParamConf|String|\{"date":"$\{yyyy-MM-dd\}"\}|参数设置|
|EnvConf|String|\{"key":"value"\}|环境变量设置|
|RunConf|String|\{"priority":1,"userName":"hadoop","memory":2048,"cores":1\}|运行配置 priority： 优先级； userName： 提交作业的 Linux 用户； memory：内存 单位为 MB； cores： 核数|
|MonitorConf|String|\{"inputs":\[\{"type":"KAFKA","clusterId":"C-1234567","topics":"kafka\_topic","consumer.group":"kafka\_consumer\_group"\}\],"outputs":\[\{"type":"KAFKA","clusterId":"C-1234567","topics":"kafka\_topic"\}\]\}|监控配置，只有SPARK\_STREAMING 类型作业支持|
|CategoryId|String|FC-5BD9575E34623940|作业对应的目录 ID|
|mode|String|YARN|模型模式，支持：YARN、LOCAL|
|LastInstanceId|String|FJI-ACCAE48B90CCB37B|最后一次执行的实例 ID|
|Adhoc|String|true|是否临时查询|
|ResourceList| | |资源列表|
|Path|String|oss://path/demo.jar|资源路径|
|Alias|String|demo.jar|资源别名|

## 示例 {#section_dcj_yxl_2gb .section}

-   请求示例

    ```
    
    /?Action=DescribeFlowJob
    &Id=FJ-BCCAE48B90CCB37B
    &ProjectId=FP-257A173659F59685
    &RegionId=cn-hangzhou
    &<公共请求参数>
    ```

-   正常返回示例

    JSON 格式

    ```
    {
    	"CategoryId":"FC-F2495319DA05CEE5",
    	"Description":"shell",
    	"FailAct":"STOP",
    	"GmtCreate":1538017814000,
    	"GmtModified":1538017814000,
    	"Id":"FJ-C7FB9F1075C7D62A",
    	"MaxRetry":0,
    	"Name":"shell_copy",
    	"Params":"ls -l",
    	"RequestId":"7D2B1B2E-8D89-49C1-8D31-097C83879D20",
    	"ResourceList":{
    		"Resource":[]
    	},
    	"RetryInterval":15,
    	"Type":"SHELL"
    }
    ```

-   异常返回示例

    JSON 格式

    ```
    
    {
    	"code":"FLOW_API_FAILED",
    	"message":"Flow Job does not exist [FJ-A10DB75C7D62A]",
    	"requestId":"243D5A48-96A5-4C0C-8966-93CBF65635ED",
    	"successResponse":false
    }
    ```


## 错误码 {#section_fcj_yxl_2gb .section}

[查看本产品错误码](https://error-center.alibabacloud.com/status/product/Emr)

