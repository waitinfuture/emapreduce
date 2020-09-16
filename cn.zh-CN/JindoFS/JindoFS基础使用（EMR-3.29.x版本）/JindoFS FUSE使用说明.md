# JindoFS FUSE使用说明

本文介绍如何通过FUSE客户端访问JindoFS。FUSE支持Block和JFS Scheme的Cache两种模式。

已创建集群，详情请参见[创建集群](/cn.zh-CN/集群管理/集群配置/创建集群.md)。

FUSE是Linux系统内核提供的一种挂载文件系统的方式。通过JindoFS的FUSE客户端，将JindoFS集群上的文件映射到本地磁盘，您可以像访问本地磁盘一样访问JindoFS集群上的数据，无需再使用`hadoop fs -ls jfs://<namespace>/`方式访问数据。

## 挂载

**说明：** 依次在每个节点上执行挂载操作。

1.  使用SSH方式登录到集群主节点，详情请参见[使用SSH连接主节点](/cn.zh-CN/集群管理/集群配置/连接集群/使用SSH连接主节点.md)。

2.  执行如下命令，新建目录。

    ```
    mkdir /mnt/jfs
    ```

3.  执行如下命令，挂载目录。

    ```
    jindofs-fuse /mnt/jfs
    ```

    /mnt/jfs作为FUSE的挂载目录。


## 读写文件

1.  列出/mnt/jfs/下的所有目录。

    ```
    ls /mnt/jfs/
    ```

    返回用户在服务端配置的所有命名空间列表。

    ```
    test  testcache
    ```

2.  列出命名空间test下面的文件列表。

    ```
    ls /mnt/jfs/test/
    ```

3.  创建目录。

    ```
    mkdir /mnt/jfs/test/dir1
    ls /mnt/jfs/test/
    ```

4.  写入文件。

    ```
    echo "hello world" > /tmp/hello.txt
    cp /tmp/hello.txt /mnt/jfs/test/dir1/
    ```

5.  读取文件。

    ```
    cat /mnt/jfs/test/dir1/hello.txt
    ```

    返回如下信息。

    ```
    hello world
    ```


如果您想使用Python方式写入和读取文件，请参见如下示例：

1.  使用Python写write.py文件，包含如下内容。

    ```
    #!/usr/bin/env python36
    with open("/mnt/jfs/test/test.txt",'w',encoding = 'utf-8') as f:
       f.write("my first file\n")
       f.write("This file\n\n")
       f.write("contains three lines\n")
    ```

2.  使用Python读文件。创建脚本read.py文件，包含如下内容。

    ```
    #!/usr/bin/env python36
    with open("/mnt/jfs/test/test.txt",'r',encoding = 'utf-8') as f:
        lines = f.readlines()
        [print(x, end = '') for x in lines]
    ```

    读取写入test.txt文件的内容。

    ```
    [hadoop@emr-header-1 ~]$ ./read.py
    ```

    返回如下信息。

    ```
    my first file
    This file
    ```


## 卸载

**说明：** 依次在每个节点上执行卸载操作。

1.  使用SSH方式登录到集群主节点，详情请参见[使用SSH连接主节点](/cn.zh-CN/集群管理/集群配置/连接集群/使用SSH连接主节点.md)。

2.  执行如下命令，卸载FUSE。

    ```
    umount jindofs-fuse
    ```

    如果出现`target is busy`错误，请切换到其它目录，停止所有正在读写FUSE文件的程序，再执行卸载操作。


