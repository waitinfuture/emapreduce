# JindoFS数据管理策略

JindoFS块存储模式对文件数据管理提供了高级策略，以满足不同情形下的存储需求，目前主要包括存储策略（Storage Policy）和压缩策略（Compression Policy）。本文详细介绍相关策略及其使用方式。

## 存储策略

JindoFS提供了Storage Policy功能，提供更加灵活的存储策略以适应不同的存储需求。支持设置以下五种存储策略。

|策略名称|策略说明|
|----|----|
|AR|数据仅在OSS上有一个备份，并且使用OSS归档存储（Archive）类型存储。|
|IA|数据仅在OSS上有一个备份，并且使用OSS低频访问（Infrequent Access）类型存储。|
|COLD|数据仅在OSS上有一个备份，并且使用OSS标准存储（Standard）类型存储。|
|WARM|数据在OSS和本地分别有一个备份， 本地备份能够有效的提供后续的读取加速。默认策略。 |
|HOT|数据在OSS和本地分别有一个备份，并且本地备份强制Pin住，不受自动缓存清理影响，针对一些最热的数据提供更加高优先级的加速效果。|

OSS存储类型的详细介绍，请参见[存储类型介绍](/cn.zh-CN/开发指南/存储类型/存储类型介绍.md)。

示例，新增的文件将会以父目录所指定的Storage Policy进行存储。

-   您可以通过以下命令，设置存储类型。

    ```
    jindo jfs -setStoragePolicy [-R] <StoragePolicy>(AR/IA/COLD/WARM/HOT) <path> ...
    ```

    其中，涉及参数如下

    -   `[-R]`：递归设置该路径下的所有路径。
    -   `<path>`：设置Storage Policy的路径名称。
-   您通过以下命令，获取某个目录的存储策略。

    ```
    jindo jfs -getStoragePolicy <path>
    ```


## 压缩策略

JindoFS提供了Compression Policy功能，可以针对数据块进行压缩后存储，能够有效地减少存储空间和数据读写，适用于一些高压缩比的文件。支持以下两种压缩策略。

|策略名称|策略说明|
|NONE|不对数据块进行压缩。默认策略。 |
|ZSTD|对数据块使用ZSTD（Zstandard）压缩算法。|

示例，新增的文件将会以父目录所指定的Compression Policy进行压缩后存储。

-   您可以通过以下命令，设置压缩类型。

    ```
    jindo jfs -setCompressionPolicy [-R] <CompressionPolicy>(NONE/ZSTD) <path> ...
    ```

    其中，涉及参数如下

    -   `[-R]`：递归设置该路径下的所有路径。
    -   `<path>`：设置Compression Policy的路径名称。
-   您通过以下命令，获取某个目录的压缩策略。

    ```
    jindo jfs -getCompressionPolicy <path> ...
    ```


