# Create and run a Pig job {#concept_h3g_qsm_hfb .concept}

This topic describes how to create and run a Pig job.

## Use OSS in Pig {#section_r4m_35m_hfb .section}

Use the following format for OSS paths:

```
oss://${AccessKeyId}:${AccessKeySecret}@${bucket}.${endpoint}/${path}
```

Parameter descriptions:

-   $\{accessKeyId\}: The AccessKey ID of your account.

-   $\{accessKeySecret\}: The AccessKey Secret corresponding to the AccessKey ID.

-   $\{bucket\}: The bucket corresponding to the AccessKey ID.

-   $\{endpoint\}: The network used for accessing OSS. It depends on the region in which your cluster exists, and the corresponding OSS should also be in the region in which your cluster exists.

    For more information about specific values, see [OSS endpoint](../../SP_21/DNOSS11827291/EN-US_TP_4350.dita#concept_zt4_cvy_5db).

-   $\{path\}: Path in the bucket.


Take script1-hadoop.pig in Pig as an example. Upload [tutorial.jar](http://emr-agent-pack.oss-cn-hangzhou.aliyuncs.com/pig/0.14.0/tutorial.jar) and [excite.log.bz2](http://emr-agent-pack.oss-cn-hangzhou.aliyuncs.com/pig/0.14.0/excite.log.bz2) in Pig to OSS. Suppose that the URLs for uploading files are oss://emr/jars/tutorial.jar and oss://emr/data/excite.log.bz2 respectively.

Follow these steps:

1.  Create scripts

    Edit the path of the JAR file and the input and output paths in the script, as shown below: Note that the format of the OSS path is oss://$\{accesskeyId\}:$\{accessKeySecret\}@$\{bucket\}.$\{endpoint\}/object/path.

    ```
    /*
     * Licensed to the Apache Software Foundation (ASF) under one
    * or more contributor license agreements.  See the NOTICE file
    * distributed with this work for additional information
    * regarding copyright ownership.  The ASF licenses this file
    * to you under the Apache License, Version 2.0 (the
    * "License"); you may not use this file except in compliance
    * with the License.  You may obtain a copy of the License at
    *
    *     http://www.apache.org/licenses/LICENSE-2.0
    *
    * Unless required by applicable law or agreed to in writing, software
    * distributed under the License is distributed on an "AS IS" BASIS,
    * WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
    * See the License for the specific language governing permissions and
    * limitations under the License.
    */
    -- Query Phrase Popularity (Hadoop cluster)
    -- This script processes a search query log file from the Excite search engine and finds search phrases that occur with particular high frequency during certain times of the day.
    -- Register the tutorial JAR file so that the included UDFs can be called in the script.
    REGISTER oss://${AccessKeyId}:${AccessKeySecret}@${bucket}.${endpoint}/data/tutorial.jar;
    -- Use the  PigStorage function to load the excite log file into the ▒raw▒ bag as an array of records.
    -- Input: (user,time,query)
    raw = LOAD 'oss://${AccessKeyId}:${AccessKeySecret}@${bucket}.${endpoint}/data/excite.log.bz2' USING PigStorage('\t') AS (user, time, query);
    -- Call the NonURLDetector UDF to remove records if the query field is empty or a URL.
    clean1 = FILTER raw BY org.apache.pig.tutorial.NonURLDetector(query);
    -- Call the ToLower UDF to change the query field to lowercase.
    clean2 = FOREACH clean1 GENERATE user, time,     org.apache.pig.tutorial.ToLower(query) as query;
    -- Because the log file only contains queries for a single day, we are only interested in the hour.
    -- The excite query log timestamp format is YYMMDDHHMMSS.
    -- Call the ExtractHour UDF to extract the hour (HH) from the time field.
    houred = FOREACH clean2 GENERATE user, org.apache.pig.tutorial.ExtractHour(time) as hour, query;
    -- Call the NGramGenerator UDF to compose the n-grams of the query.
    ngramed1 = FOREACH houred GENERATE user, hour, flatten(org.apache.pig.tutorial.NGramGenerator(query)) as ngram;
    -- Use the  DISTINCT command to get the unique n-grams for all records.
    ngramed2 = DISTINCT ngramed1;
    -- Use the  GROUP command to group records by n-gram and hour.
    hour_frequency1 = GROUP ngramed2 BY (ngram, hour);
    -- Use the  COUNT function to get the count (occurrences) of each n-gram.
    hour_frequency2 = FOREACH hour_frequency1 GENERATE flatten($0), COUNT($1) as count;
    -- Use the  GROUP command to group records by n-gram only.
    -- Each group now corresponds to a distinct n-gram and has the count for each hour.
    uniq_frequency1 = GROUP hour_frequency2 BY group::ngram;
    -- For each group, identify the hour in which this n-gram is used with a particularly high frequency.
    -- Call the ScoreGenerator UDF to calculate a "popularity" score for the n-gram.
    uniq_frequency2 = FOREACH uniq_frequency1 GENERATE flatten($0), flatten(org.apache.pig.tutorial.ScoreGenerator($1));
    -- Use the  FOREACH-GENERATE command to assign names to the fields.
    uniq_frequency3 = FOREACH uniq_frequency2 GENERATE $1 as hour, $0 as ngram, $2 as score, $3 as count, $4 as mean;
    -- Use the  FILTER command to move all records with a score less than or equal to 2.0.
    filtered_uniq_frequency = FILTER uniq_frequency3 BY score > 2.0;
    -- Use the  ORDER command to sort the remaining records by hour and score.
    ordered_uniq_frequency = ORDER filtered_uniq_frequency BY hour, score;
    -- Use the  PigStorage function to store the results.
    -- Output: (hour, n-gram, score, count, average_counts_among_all_hours)
    STORE ordered_uniq_frequency INTO 'oss://${AccessKeyId}:${AccessKeySecret}@${bucket}.${endpoint}/data/script1-hadoop-results' USING PigStorage();
    ```

2.  Create a job

    Store the script created in step 1 to OSS. For example, if the storage path is oss://emr/jars/script1-hadoop.pig, follow these steps to create a job in E-MapReduce:

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17986/154217727413205_en-US.png)

3.  Create and run an execution plan.

    Create the execution plan in E-MapReduce and add the created Pig job to the execution plan. Select **Run Now** as the policy so that the script1-hadoop job can run in the selected cluster.


