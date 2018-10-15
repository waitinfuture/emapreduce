# Pig job configuration {#concept_abt_sgp_y2b .concept}

When you are applying for clusters in E-MapReduce, a Pig environment is provided by default. You can create and operate tables and data by using Pig.

## The procedure is as follows. {#section_tvg_1hp_y2b .section}

1.  Prepare the Pig script in advance, for example:

    ```
    ```shell
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
     REGISTER oss://emr/checklist/jars/chengtao/pig/tutorial.jar;
     -- Use the  PigStorage function to load the excite log file into the “raw” bag as an array of records.
     -- Input: (user,time,query) 
     raw = LOAD 'oss://emr/checklist/data/chengtao/pig/excite.log.bz2' USING PigStorage('\t') AS (user, time, query);
     -- Call the NonURLDetector UDF to remove records if the query field is empty or a URL. 
     clean1 = FILTER raw BY org.apache.pig.tutorial.NonURLDetector(query);
     -- Call the ToLower UDF to change the query field to lowercase. 
     clean2 = FOREACH clean1 GENERATE user, time, org.apache.pig.tutorial.ToLower(query) as query;
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
     STORE ordered_uniq_frequency INTO 'oss://emr/checklist/data/chengtao/pig/script1-hadoop-results' USING PigStorage();
     ```
    ```

2.  Save this script into a script file, such as script1-hadoop-oss.pig, and then upload it to an OSS directory\(for example, oss://path/to/script1-hadoop-oss.pig\).
3.  Log on to the [Alibaba Cloud E-MapReduce Console Job List](https://emr.console.aliyun.com/?spm=5176.doc28084.2.1.gxpx8G#/job/region/cn-hangzhou).
4.  Click **Create a job** in the upper right corner to enter the job creation page.
5.  Enter the job name.
6.  Select the Pig job type to create a Pig job. This type of job is submitted in the background by using the following process:

    ```
    pig [user provided parameters]
    ```

7.  Enter the **Parameters** in the option box with parameters subsequent to Pig commands. For example, if it is necessary to use a Pig script uploaded to OSS, the following must be entered:

    ```
    -x mapreduce ossref://emr/checklist/jars/chengtao/pig/script1-hadoop-oss.pig
    ```

    You can click **Select OSS path** to view and select from OSS. The system will automatically complete the path of Pig script on OSS. Switch the Pig script prefix to ossref \(click **Switch resource type**\) to guarantee this file is properly downloaded by E-MapReduce.

8.  Select the policy for failed operations.
9.  Click **OK** to complete the Pig job configuration.

