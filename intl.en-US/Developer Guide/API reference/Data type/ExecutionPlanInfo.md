# ExecutionPlanInfo {#concept_ykd_qsb_kfb .concept}

|Name|Type|Description|
|----|----|-----------|
|Id|String |The ID of the execution plan.|
|Parameter|String |The name of the execution plan.|
|CreateClusterOnDemand|Boolean|Indicates whether the execution plan is created on demand automatically.|
|Strategy|String |The method for implementing the execution plan. RUN\_MANUALLY: indicates that you can implement an execution plan manually. SCHEDULE: indicates that an execution plan is automatically implemented at the scheduled time.|
|Status|String|The status of the schedule.|
|StartTime|String|The start time of the effective period of the schedule.|
|TimeUnit|String|The unit of the time interval of the periodic schedule. Valid values: DAY and HOUR.|
|TimeInterval|String|The time interval. If you specify DAY for TimeUnit, the value of this parameter must be 1. If you specify HOUR for TimeUnit, the value of this parameter ranges from 1 to 23.|

