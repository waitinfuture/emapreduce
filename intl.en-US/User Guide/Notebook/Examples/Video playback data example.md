# Video playback data example {#concept_ng4_5vk_z2b .concept}

## Data preparations {#section_bvv_wvk_z2b .section}

In this example, you need to download data from OSS and upload it to your OSS bucket. The data includes:

-   [User Table Sample Data](http://emr-sample-projects.oss-cn-hangzhou.aliyuncs.com/notedata/userdata?spm=a2c4g.11186623.2.4.c37a41abbvLbuq)
-   [Video Table Sample Data](http://emr-sample-projects.oss-cn-hangzhou.aliyuncs.com/notedata/video)
-   [Playvideo Table Sample Data](http://emr-sample-projects.oss-cn-hangzhou.aliyuncs.com/notedata/playvideodata)

Upload the sample data of User Table, Video Table, and Playvideo Table to the specified UserInfo, Videoinfo, and Playvideo, respectively, on your OSS bucket. For example, upload the data to Demo or UserInfo directory under Bucket Example.

In the following created table, replace the SQL \[bucketname\] with your bucket name, replace \[region\] with your OSS region name, and replace \[bucketpath\] with your specified path prefix of the OSS, for example, “Demo”.

## Section one: Create a user table {#section_hn2_hwk_z2b .section}

```
%hive
CREATE EXTERNAL TABLE user_info(id int,sex int,age int, marital_status int) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' LOCATION 'oss://[bucketname].oss-cn-[region]-internal.aliyuncs.com/[bucketpath]/userinfo'
```

## Section two: Create a video table {#section_whw_jwk_z2b .section}

```
%hive
CREATE EXTERNAL TABLE video_info(id int,title string,type string) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' LOCATION 'oss://[bucketname].oss-cn-[region]-internal.aliyuncs.com/[bucketpath]/videoinfo'
```

## Section three: Create a video play table {#section_fkp_lwk_z2b .section}

```
%hive
CREATE EXTERNAL TABLE play_video(user_id int,video_id int, play_time bigint) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' LOCATION 'oss://[bucketname].oss-cn-[region]-internal.aliyuncs.com/[bucketpath]/playvideo'
```

## Section four: User table count {#section_jjy_nwk_z2b .section}

```
%sql select count(*) from user_info
```

## Section five: Video table count {#section_ujp_pwk_z2b .section}

```
%sql select count(*) from video_info
```

## Section six: Video play table count {#section_s3c_rwk_z2b .section}

```
%sql select count(*) from play_video
```

## Section seven: Video play count of each video type {#section_bqj_swk_z2b .section}

```
%sql select video.type, count(video.type) as count from play_video play join video_info video on (play.video_id = video.id) group by video.type order by count desc
```

## Section eight: Video information of video play top 10 {#section_o1b_5wk_z2b .section}

```
%sql select video.id, video.title, video.type, video_count.count from (select video_id, count(video_id) as count from play_video group by video_id order by count desc limit 10) video_count join video_info video on (video_count.video_id = video.id) order by count desc
```

## Section nien: Age groups of audience watching the video with largest video play count {#section_h3v_wwk_z2b .section}

```
%sql select age , count(*) as count  from (select distinct(user_id) from play_video  where video_id =49 ) play join user_info userinfo on (play.user_id = userinfo.id) group by userinfo.age
```

## Section ten: Gender, age group, and marital status of audience watching the video with largest video play count {#section_m1b_ywk_z2b .section}

```
%sql select if(sex=0,'Female','Male') as title, count(*) as count, 'Gender' as type from (select distinct(user_id) from play_video  where video_id =49 ) play join user_info userinfo on (play.user_id = userinfo.id) group by userinfo.sex
union all
select case when userinfo.age<15 then 'Less than 15' when age<25 then '15-25' when age<35 then '25-35' else 'More than 35' end  , count(*) as count, 'Age Group' as type from (select distinct(user_id) from play_video  where video_id =49) play join user_info userinfo on (play.user_id = userinfo.id) group by case when userinfo.age<15 then 'Less than 15' when age<25 then '15-25' when age<35 then '25-35' else 'More than 35' end 
union all
select if(marital_status=0,'Unmarried','Married') as title, count(*) as count, 'Marital Status' as type from (select distinct(user_id) from play_video  where video_id =49 ) play join user_info userinfo on (play.user_id = userinfo.id) group by marital_status
```

