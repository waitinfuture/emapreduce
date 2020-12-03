# Faiss-Server

Faiss-Server基于Faiss的开源实现，是一个使用gRPC协议提供向量检索的服务。

## 支持加载的向量文件格式

文件格式通过后缀来区分，支持的文件格式如下：

-   .fvecs（Fvecs格式文件）
-   .svm（Libsvm格式文件）

    Libsvm要求的格式如下。

    ```
    21187279 1:0.2663046344870018 2:0.36652042181588923 3:1.0686633708024278 4:0.48038935355720136 5:0.25852895390543884 6:0.3548201486996048 7:0.5256999599627583 8:0.6950687558969181 9:0.678305894615746 10:0.22776047265501018
    21264640 1:0.3939700179792862 2:0.2754414668803289 3:0.9439534329739017 4:0.40100161008614665 5:0.4995636031244379 6:0.013824724791385053 7:0.5587276328436032 8:0.5785080421720411 9:0.2784678162956546 10:0.5387681136371216
    ```

    通用的形式标识为`tagid 1:xx 2:xx 3:xx`。其中`tagid`可以是任意的字符串，例如在推荐的场景中，通常设置为itemid。数字`1`是向量的数据，通常从1开始。`xx`是维度的具体值。

-   .index（Faiss生成的Index文件）

    如果您直接加载数据量大的Index文件，创建索引会非常慢。但是在线下通过Libsvm格式的文件build成Faiss的Index文件，再通过在线Faiss Server加载Index文件，加载速度非常快。


**说明：** 文件命名时，请保持文件格式与后缀一致。在部分场景下生成OSS文件时，OSS会在后缀名添加随机的字符，此时并不影响文件的加载，只需要与原始的后缀保持一致即可。

## 加载本地的向量文件

示例文件：[article\_word2vec.svm](http://docs-aliyun.cn-hangzhou.oss.aliyun-inc.com/assets/attach/33813/cn_zh/1591948286428/article_word2vec.svm)

加载本地的向量文件代码示例如下。

```
docker run -it -p  9000:9000  -v /home/bruceding.jing:/index registry.cn-beijing.aliyuncs.com/pai-recommend/faiss-server:0.0.2 --index_source=/index/article_word2vec.svm --index_params=Flat
// 以下是docker的输出内容。
2020-05-01 15:18:01,134 INFO src/faiss_parameter.cc:39 index source:/index/article_word2vec.svm,index params:Flat
2020-05-01 15:18:01,134 INFO src/faiss_index.cc:26 init index
2020-05-01 15:18:01,377 INFO src/faiss_index.cc:119 size:10907,dimension:10
2020-05-01 15:18:01,377 INFO src/faiss_index.cc:120 index_params=Flat
2020-05-01 15:18:01,377 INFO src/main.cc:55 create index success
2020-05-01 15:18:01,377 INFO src/faiss_server.cc:23 Server listening on 0.0.0.0:9000
```

运行命令后，输出相关的日志信息，启动gRPC server成功，监听9000的端口。

示例中相关参数描述如下。

|参数|描述|
|--|--|
|-p|使用docker的`-p`参数进行端口映射。 本示例中为9000，表示Faiss-Server的默认监听端口。 |
|registry.cn-beijing.aliyuncs.com/pai-recommend/faiss-server:0.0.2|Faiss-Server提供的docker镜像地址。|
|--index\_source|指定向量文件的具体路径。|
|--index\_params|指定Faiss Index构建的参数，本示例中使用了`Flat`搜索的方式构建索引。 Faiss-Server使用Faiss的index\_factory的形式构建索引。 `--index_params`参数详情请参见[Faiss indexes](https://github.com/facebookresearch/faiss/wiki/Faiss-indexes)。 |

## 加载OSS中的向量文件

通常，向量文件生成在OSS中，Faiss-Server支持直接从OSS的向量文件进行索引构建。

```
docker run  -it -p 9000:9000  registry.cn-beijing.aliyuncs.com/pai-recommend/faiss-server:0.0.2  --index_params=IVF256,Flat --search_params=nprobe=128 --accessid=xxx --accesskey=xxx --endpoint=oss-cn-beijing.aliyuncs.com --bucket_name=faiss-server --object_name=demo_data/article_word2vec.svm
```

示例中相关参数描述如下。

|参数|描述|
|--|--|
|--index\_params|使用的是`IVF256,Flat`，使用了聚簇索引的方式，分成了256份。|
|--search\_params|查询向量最相近的128个聚簇集合。具体的查询参数设置请参见[Index IO, cloning and hyper parameter tuning](https://github.com/facebookresearch/faiss/wiki/Index-IO,-cloning-and-hyper-parameter-tuning)。|
|--bucket\_name|OSS的bucket名称。|
|--object\_name|OSS的向量文件地址。|
|--endpoint|OSS的访问地址， 这里是外网的地址。在VPC内网的情况下，一定要使用内网地址，节省流量费用，读取速度也会更快。|

**说明：** 因为要读取OSS的数据，所以需要设置阿里云账号的AccessKey ID和AccessKey Secret。

您可以执行以下命令，查看Faiss-Server支持的参数列表。

```
docker run registry.cn-beijing.aliyuncs.com/pai-recommend/faiss-server:0.0.2 --help
```

## 加载OSS中Version检查机制

通常，Faiss-Server是在线的服务，直接加载向量文件耗时长。因此您可以生成Index索引文件，通过Faiss-Server直接加载Index文件节省时间。

部分场景下，向量文件需要定时加载。

您可以生成多个版本的向量文件，通过Version文件记录当前加载的文件。Faiss-Server会异步检查Version文件内容，一旦有变动，会自动加载最新的Index和Idxmap文件。

![流程](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/8349449951/p127141.png)

对应图中步骤描述如下：

-   1：向量召回算法，生成item的向量文件，存储至OSS文件中。
-   2和3：Faiss-Server读取OSS的向量文件，进行构建索引。

    Version文件包含版本内容，实质是Index和Idxmap两个文件地址：

    -   Index文件是构建出来的Faiss Index文件。
    -   Idxmap文件是映射表，是tagid和Index具体向量位置的映射。
-   4：在线的Faiss-Server服务一直是启动状态，会监听Version文件内容。

介绍离线流程中，如何构造Index文件。

1.  构建Index索引文件。

    此示例构建索引，构建完成后，进程退出。因为不涉及gRPC服务，所以docker的`-p`参数不需要设置。

    ```
    docker run  -it registry.cn-beijing.aliyuncs.com/pai-recommend/faiss-server:0.0.2  --index_params=IVF256,Flat --search_params=nprobe=128 --accessid=xxx --accesskey=xxxx --endpoint=oss-cn-beijing.aliyuncs.com --bucket_name=faiss-server --object_name=demo_data/article_word2vec.svm --version_name=demo_data/versions --action=build_index
    ```

    示例中相关参数描述如下：

    -   `--version_name`： 指定Version文件在OSS上的具体位置。如果Version文件不存在，可以自动生成。
    -   `--action`：build流程设置build\_index即可。
    构建成功后，生成以下三个文件。

    ![Index](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/8349449951/p127134.png)

    如果svm的向量文件在本地，执行以下命令，将生成的Index等文件放到OSS中。

    ```
    docker run  -it -v /home/bruceding.jing:/index  registry.cn-beijing.aliyuncs.com/pai-recommend/faiss-server:0.0.2  --index_params=IVF256,Flat --search_params=nprobe=128 --accessid=xxx --accesskey=xxx --endpoint=oss-cn-beijing.aliyuncs.com --bucket_name=faiss-server --index_source=/index/article_word2vec.svm --version_name=demo_data/versions --action=build_index
    ```

    **说明：** `-v`：表示把本地的svm文件传给docker。

2.  启动Faiss-Server在线服务。

    您可以重新开启一个终端，模拟启动Faiss-Server的在线服务。

    ```
    docker run  -it  -p 9000:9000  registry.cn-beijing.aliyuncs.com/pai-recommend/faiss-server:0.0.2  --index_params=IVF256,Flat --search_params=nprobe=128 --accessid=xxxx --accesskey=xxxx --endpoint=oss-cn-beijing.aliyuncs.com --bucket_name=faiss-server  --version_name=demo_data/versions  --check_version=true
    ```

    示例中相关参数描述如下：

    -   `--index_params`和`--search_params`需要和构建的Index文件保持一致。
    -   `--version_name`：指定了Version文件在OSS上的具体位置。如果文件不存在，可以自动生成。
    -   `--check_version=true`：表示开启Version检查机制。开启Version检查机制，Version文件变更，Faiss-Server才会自动加载。
    启动成功后，回到前一个窗口，再次构建Index索引。构建成功后，如果输出信息类似如下，表示最新的Index加载成功。

    ```
    2020-05-01 17:09:37,478 INFO src/faiss_index.cc:197 start to load
    2020-05-01 17:09:37,567 INFO src/oss_util.cc:30 GetObjectToFile success62
    2020-05-01 17:09:37,568 INFO src/faiss_index.cc:219 localPath=/tmp/20200501170924.index
    2020-05-01 17:09:37,703 INFO src/oss_util.cc:30 GetObjectToFile success535963
    2020-05-01 17:09:37,815 INFO src/oss_util.cc:30 GetObjectToFile success98159
    2020-05-01 17:09:37,817 INFO src/faiss_index.cc:245 load index success
    ```

3.  追加向量文件。

    加载全量的Index文件之后，Faiss-Server还支持加载增量的SVM格式的向量文件。

    ```
    docker run  -it  -p 9000:9000  registry.cn-beijing.aliyuncs.com/pai-recommend/faiss-server:0.0.2  --index_params=IVF256,Flat --search_params=nprobe=128 --accessid=xxx --accesskey=xxxx --endpoint=oss-cn-beijing.aliyuncs.com --bucket_name=faiss-server  --version_name=demo_data/versions  --check_version=true --append_dir=demo_data/append
    ```

    `--append_dir`：指定SVM格式增量文件追加目录。

    当您把增量文件放到append\_dir后，会看到如下类似的输出信息。

    ```
    2020-05-01 17:24:53,161 INFO src/oss_util.cc:30 GetObjectToFile success2428471
    2020-05-01 17:24:53,357 INFO src/faiss_index.cc:383 append index, file:article_word2vec.svm, size=10907, idx size=10907
    ```


## 测试向量服务

启动Faiss-Server之后，可以使用如下工具测试。

工具和源码下载：[faiss-client-go.tar.gz](http://docs-aliyun.cn-hangzhou.oss.aliyun-inc.com/assets/attach/33813/cn_zh/1591951881009/faiss-client-go.tar.gz)和[faiss-client-java.tar.gz](https://docs-aliyun.cn-hangzhou.oss.aliyun-inc.com/assets/attach/183066/cn_zh/1605614238484/faiss-client-java.tar.gz)。

```
./faiss-client-go --host "11.158.166.161:9000" --vector "1:0.8254435895816826 2:0.3546776922906237 3:0.14982954432756015 4:0.4796270382425792 5:0.5478646494350313 6:0.3771617042371068 7:0.5107318036421319 8:0.7005006312566686 9:0.7030542773195687 10:0.692132791047145"  --k 20
```

示例中相关参数描述如下。

|参数|描述|
|--|--|
|--host|指定Faiss-Server的地址，包括IP地址和端口号。|
|--vector|指定请求的向量数据，和SVM的格式一样，维度也一致。本示例中是10维的数据。|
|--k|指定返回TopN的⼤小。 默认是10，本示例中指定为20。 |

返回信息如下。

![回显](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/8349449951/p127112.png)

**说明：** `tagid`指的是SVM文件里的tagid，通常是`itemid`。

## 在EMR上部署Faiss-server服务

1.  创建EMR的Data Science集群，相关信息请参见[概述](/cn.zh-CN/集群类型/Data Science集群/概述.md)。

    默认支持Faiss-Server服务。

    ![data_science](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/2908096061/p127044.png)

    **说明：** 镜像registry.cn-beijing.aliyuncs.com/pai-recommend/faiss-server:0.0.2已经打包在EMR上，镜像名称为 faiss-server:1.0.0。

2.  新建Shell类型的作业来加载向量文件，启动gRPC服务。

    ```
    sudo docker run -d --restart unless-stopped -p 9001:9000  faiss-server:1.0.0  --index_params=IVF256,Flat --search_params=nprobe=128 --accessid=xxxx --accesskey=xxx --endpoint=oss-cn-huhehaote-internal.aliyuncs.com --bucket_name=faiss-test  --object_name=article_word2vec.svm
    ```


