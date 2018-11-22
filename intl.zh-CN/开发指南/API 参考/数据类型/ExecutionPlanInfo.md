# ExecutionPlanInfo {#concept_ykd_qsb_kfb .concept}

|名称|类型|描述|
|--|--|--|
|Id|String|执行计划Id|
|Name|String|执行计划名称|
|CreateClusterOnDemand|Boolean|是否是按需自动创建|
|Stragety|String|执行计划的运行策略，RUN\_MANUALLY：手动执行，只有当用户主动触发的时候才运行,SCHEDULE：周期调度，设置时间在指定的时间自动运行|
|Status|String|调度状态|
|StartTime|String|周期调度开始生效的时间|
|TimeUnit|String|周期调度的时间间隔，DAY：天为单位调度，HOUR：小时为单位调度|
|TimeInterval|String|间隔时间，若时间单位是DAY：只能是1， 若时间单位是HOUR：可以设置1-23|

