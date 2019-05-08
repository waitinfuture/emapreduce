# Pig 作业配置 {#concept_abt_sgp_y2b .concept}

E-MapReduce 中，用户申请集群的时候，默认为用户提供了 Pig 环境，用户可以直接使用 Pig 来创建和操作自己的表和数据。

## 操作步骤 {#section_tvg_1hp_y2b .section}

1.  用户需要提前准备好 Pig 的脚本，例如：

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

2.  将该脚本保存到一个脚本文件中，例如叫 script1-hadoop-oss.pig，然后将该脚本上传到 OSS 的某个目录中（例如：oss://path/to/script1-hadoop-oss.pig）。
3.  通过主账号登录[阿里云 E-MapReduce 控制台](https://emr.console.aliyun.com/)。
4.  单击上方的数据开发页签，进入项目列表页面。
5.  单击对应项目右侧的**工作流设计**，在左侧导航栏中单击**作业编辑**进入作业编辑页面。
6.  在页面左侧，在需要操作的文件夹上单击右键，选择**新建作业**。
7.  填写作业名称，作业描述。
8.  选择 Pig 作业类型，表示创建的作业是一个 Pig 作业。这种类型的作业，其后台实际上是通过以下的方式提交。

    ```
    pig [user provided parameters]
    ```

9.  单击**确定**。

    **说明：** 您还可以通过在文件夹上单击右键，进行创建子文件夹、重命名文件夹和删除文件夹操作。

10. 在**作业内容**输入框中填入 Pig 命令后续的参数。例如，如果需要使用刚刚上传到 OSS 的 Pig 脚本，则填写如下：

    ```
    -x mapreduce ossref://emr/checklist/jars/chengtao/pig/script1-hadoop-oss.pig
    ```

    您也可以单击**选择 OSS 路径**，从 OSS 中进行浏览和选择，系统会自动补齐 OSS 上 Pig 脚本的绝对路径。请务必将 Pig 脚本的前缀修改为 ossref（单击**切换资源类型**），以保证 E-MapReduce 可以正确下载该文件。

11. 单击**保存**，Shell 作业即定义完成。

