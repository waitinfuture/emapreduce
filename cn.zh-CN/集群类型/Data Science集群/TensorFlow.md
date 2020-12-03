# TensorFlow

Data Science集群内置Python 3的Tensorflow 1.15.0版本，可以直接使用。其中Master节点只支持购买CPU资源计算TensorFlow作业，Core节点支持购买CPU或GPU资源计算TensorFlow作业。本文主要介绍如何查看TensorFlow的版本、切换TensorFlow版本以及如何安装Python包。

## 使用引导

-   [查看版本信息](#section_atn_bt2_ja8)
-   [切换TensorFlow版本](#section_5k4_ulm_lnm)
-   [安装Python包](#section_7a6_a0f_u2e)

## 查看版本信息

1.  使用SSH方式登录到集群主节点，详情请参见[使用SSH连接主节点](/cn.zh-CN/集群管理/集群配置/连接集群/使用SSH连接主节点.md)。

2.  使用pip3 list命令，查看TensorFlow的版本信息。

    ![list](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/9349449951/p127294.png)


## 切换TensorFlow版本

1.  下载切换TensorFlow版本的压缩包。

    切换TensorFlow版本压缩包：[install\_tf\_header.tar.gz](https://docs-aliyun.cn-hangzhou.oss.aliyun-inc.com/assets/attach/183066/cn_zh/1605595674996/install_tf_header.tar.gz)

2.  使用文件传输工具，上传install\_tf\_header.tar.gz至Data Science集群Master节点的任意目录下。

    **说明：** 本文上传至/root目录。

3.  使用SSH方式登录到集群主节点，详情请参见[使用SSH连接主节点](/cn.zh-CN/集群管理/集群配置/连接集群/使用SSH连接主节点.md)。

4.  执行如下命令，切换TensorFlow版本。

    1.  解压缩文件。

        ```
        tar -zxvf install_tf_header.tar.gz
        ```

    2.  切换TensorFlow版本。

        -   命令格式

            ```
            sh install_tf_header.sh <version>
            ```

            `version`为您需要切换的版本号。

        -   示例：执行如下命令，切换TensorFlow版本为2.0.3。

            ```
            sh install_tf_header.sh 2.0.3
            ```

5.  使用pip3 list命令，查看TensorFlow版本号。

    ![version_](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/2337525061/p182193.png)

    TensorFlow版本已经切换为2.0.3版本。


## 安装Python包

1.  下载安装Python的压缩包。

    安装Python的压缩包：[install\_app\_onds.tar.gz](https://docs-aliyun.cn-hangzhou.oss.aliyun-inc.com/assets/attach/183066/cn_zh/1605595697817/install_app_onds.tar.gz)

2.  使用文件传输工具，上传install\_app\_onds.tar.gz至Data Science集群Master节点的任意目录下。

    **说明：** 本文上传至/root目录。

3.  使用SSH方式登录到集群主节点，详情请参见[使用SSH连接主节点](/cn.zh-CN/集群管理/集群配置/连接集群/使用SSH连接主节点.md)。

4.  执行如下命令，在Data Science集群所有节点安装Python包。

    1.  解压缩文件。

        ```
        tar -zxvf install_app_onds.tar.gz
        ```

    2.  安装Python包。

        -   命令格式

            ```
            sh install_app_onds.sh <package_name> <version>
            ```

            其中涉及参数如下：

            -   `package_name`为您需要安装的Python包名。
            -   `version`为您需要安装Python包的版本号。
        -   示例：执行如下命令，在Data Science集群的所有节点上安装GNU Readline包，版本号为8.0.0。

            ```
            sh install_app_onds.sh gnureadline 8.0.0
            ```


