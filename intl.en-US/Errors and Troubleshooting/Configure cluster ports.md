# Configure cluster ports {#concept_ff3_3yb_pfb .concept}

## Hadoop HDFS {#section_dtw_jyb_pfb .section}

|Service|Limit|Port number|Access requirement|Parameter|Description|
|-------|-----|-----------|------------------|---------|-----------|
|NameNode|-|9000|External|fs.default.name or fs.defaultFS|fs.default.name is expired but still available|
|NameNode|-|50070|External|dfs.http.address or dfs.namenode.http-address|dfs.http.address is expired but still available.|

## Hadoop YARN \(MRv2\) {#section_axk_myb_pfb .section}

|Service|Limit|Port number|Access requirement|Parameter|Description|
|-------|-----|-----------|------------------|---------|-----------|
|JobHistory Server|-|10020|Internal|mapreduce.jobhistory.address|-|
|JobHistory Server|-|19888|External|mapreduce.jobhistory.webapp.address|-|
|ResourceManager|-|8025|Internal|yarn.resourcemanager.resource-tracker.address|-|
|ResourceManager|-|8032|Internal|yarn.resourcemanager.address|-|
|ResourceManager|-|8030|Internal|yarn.resourcemanager.scheduler.address|-|
|ResourceManager|-|8088|Internal|yarn.resourcemanager.webapp.address|-|

## Hadoop MapReduce \(MRv1\) {#section_swr_4yb_pfb .section}

|Service|Limit|Port number|Access requirement|Parameter|Description|
|-------|-----|-----------|------------------|---------|-----------|
|JobTracker|-|8021|External|mapreduce.jobtracker.address|-|

## Hadoop HBase {#section_rjn_qyb_pfb .section}

|Service|Limit|Port number|Access requirement|Parameter|Description|
|-------|-----|-----------|------------------|---------|-----------|
|HMaster|-|16000|Internal|hbase.master.port|-|
|HMaster|-|16010|External|hbase.master.info.port|-|
|HRegionServer|-|16020|Internal|hbase.regionserver.port|-|
|HRegionServer|-|16030|External|hbase.regionserver.info.port|-|
|ThriftServer|-|9099|External|-|-|

## Hadoop Spark {#section_czc_syb_pfb .section}

|Service|Limit|Port number|Access requirement|Parameter|Description|
|-------|-----|-----------|------------------|---------|-----------|
|SparkHistory|-|18080|External|-|-|

## Storm {#section_hyr_d3j_ngb .section}

|Service|Port number|Parameter|
|-------|-----------|---------|
|Storm UI     |9999|ui\_port|

## Druid {#section_sbd_d3f_4gb .section}

|Service|Port number|Parameter|
|-------|-----------|---------|
|overlord|18090|overlord.runtime -\> druid.port|
|coordinator|18081|coordinator.runtime -\> druid.port|
|middleManager|18091|middleManager.runtime -\> druid.port|
|historical|18083|historical.runtime -\> druid.port|

