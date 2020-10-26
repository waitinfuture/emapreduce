# JindoFS分层存储命令使用说明

EMR-3.30版本JindoFS引入分层存储功能。通过该功能您可以根据数据冷热程度选择不同的存储介质来存储数据，以减少数据存储成本，或者加速访问数据的速度。

## 使用Jindo jfs

执行以下命令，获取帮助信息。

```
[root@emr-header-1 ~]# jindo jfs -help archive
-archive -i/a <path> ... :
  Archive commands.
```

JindoFS分层存储命令均为异步执行，分层存储命令只是启动相关任务执行。

常用命令如下：

-   [Cache命令](#section_69h_xwy_36s)
-   [Uncache命令](#section_8fa_jvl_g2w)
-   [Archive命令](#section_6rs_viw_b2x)
-   [Unarchive命令](#section_o06_4ll_fcf)
-   [Status命令](#section_kk5_kul_gkv)
-   [ls2命令](#section_m3n_mxc_40v)

## Cache命令

Cache命令可以备份对应路径的数据至本集群的磁盘，以便于后续可以读取本地数据，无需读取OSS上的数据。

```
jindo jfs -cache -p <path>
```

`-p`参数可以保证本地数据不受磁盘水位清理。

## Uncache命令

Uncache命令可以删除本地集群中的本地备份，只存储数据在OSS标准存储上，以便于后续读取OSS上的数据。

```
jindo jfs -uncache  <path>
```

## Archive命令

Archive命令可以归档存储数据，删除本地磁盘上的数据备份，归档OSS上的数据至低频访问存储或者归档存储上。存储类型请参见对象存储OSS的[存储类型介绍](/cn.zh-CN/开发指南/存储类型/存储类型介绍.md)。

```
jindo jfs -archive -i|-a <path>
```

`-i`参数可以归档数据至OSS低频存储类型。`-a`参数可以归档数据至OSS归档存储类型。

## Unarchive命令

Unarchive命令可以将数据从归档（存储类型或者低频存储）类型恢复到低频存储或者标准存储，同时可以临时解冻归档存储类型，使数据临时可读，有效时间为1天。

```
jindo jfs -unarchive -i/-o <path>
```

`-i`参数可以恢复数据至OSS低频存储类型。`-o`参数可以临时解冻归档存储类型，使数据临时可读。

## Status命令

Status命令可以查看任务进度信息，默认会统计该路径需要执行分层存储的文件数目以及已经完成的数据。

```
jindo jfs -status -detail/-sync <path>
```

`-detail`参数可以查看文件进度信息。`-sync`参数表示该命令需要同步等待分层存储任务结束才会退出。

## ls2命令

JindoFS扩展hadoop ls相关操作，提供ls2命令可以查看文件存储状态。

```
hadoop fs -ls2 <path>
```

返回信息会包含文件的存储类型，示例如下。

```
drwxrwxrwx  - -         0    2020-06-05 04:27 oss://xxxx/warehouse
-rw-rw-rw-  1 Archive   1484 2020-09-23 16:40 oss://xxxx/wikipedia_data.csv
-rw-rw-rw-  1 Standard  1676 2020-06-07 20:04 oss://xxxx/wikipedia_data.json
```

