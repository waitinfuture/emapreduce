# Use JindoFS FUSE

This topic describes how to access JindoFS from a Filesystem in Userspace \(FUSE\) client. FUSE allows you to use JindoFS in block storage and cache mode \(only JFS Scheme supported\).

An E-MapReduce \(EMR\) cluster is created. For more information, see [Create a cluster](/intl.en-US/Cluster Management/Configure clusters/Create a cluster.md).

FUSE is a method provided by the Linux kernel to mount a file system. You can use a FUSE client to map files in a JindoFS cluster to a local disk. Then, you can directly access data in the JindoFS cluster without the need to use the `hadoop fs -ls jfs://<namespace>/` command.

## Mount a directory

**Note:** Perform the following steps on each node of the cluster in sequence.

1.  Log on to the master node of the cluster in SSH mode. For more information, see [Connect to the master node of an EMR cluster in SSH mode](/intl.en-US/Cluster Management/Configure clusters/Connect to a cluster/Connect to the master node of an EMR cluster in SSH mode.md).

2.  Run the following command to create a directory:

    ```
    mkdir /mnt/jfs
    ```

3.  Run the following command to mount the directory:

    ```
    jindofs-fuse /mnt/jfs
    ```

    In this example, the /mnt/jfs/ directory is mounted.


## Read and write files

1.  Run the following command to query all the sub-directories under the /mnt/jfs/ directory:

    ```
    ls /mnt/jfs/
    ```

    All the namespaces that you have configured on the server are returned.

    ```
    test  testcache
    ```

2.  Run the following command to query all the files in the test namespace:

    ```
    ls /mnt/jfs/test/
    ```

3.  Run the following command to create a directory:

    ```
    mkdir /mnt/jfs/test/dir1
    ls /mnt/jfs/test/
    ```

4.  Run the following command to write data into a file:

    ```
    echo "hello world" > /tmp/hello.txt
    cp /tmp/hello.txt /mnt/jfs/test/dir1/
    ```

5.  Run the following command to read data from a file:

    ```
    cat /mnt/jfs/test/dir1/hello.txt
    ```

    The following information is returned:

    ```
    hello world
    ```


The following example shows how to read data from and write data into files in Python:

1.  Write data into a file in Python. Create the write.py script file, which contains the following content:

    ```
    #! /usr/bin/env python36
    with open("/mnt/jfs/test/test.txt",'w',encoding = 'utf-8') as f:
       f.write("my first file\n")
       f.write("This file\n\n")
       f.write("contains three lines\n")
    ```

2.  Read data from a file in Python. Create the read.py script file, which contains the following content:

    ```
    #! /usr/bin/env python36
    with open("/mnt/jfs/test/test.txt",'r',encoding = 'utf-8') as f:
        lines = f.readlines()
        [print(x, end = '') for x in lines]
    ```

    Run the following command to read the data that is written into the test.txt file:

    ```
    [hadoop@emr-header-1 ~]$ ./read.py
    ```

    The following information is returned:

    ```
    my first file
    This file
    ```


## Unmount FUSE

**Note:** Perform the following steps on each node of the cluster in sequence.

1.  Log on to the master node of the cluster in SSH mode. For more information, see [Connect to the master node of an EMR cluster in SSH mode](/intl.en-US/Cluster Management/Configure clusters/Connect to a cluster/Connect to the master node of an EMR cluster in SSH mode.md).

2.  Run the following command to unmount FUSE:

    ```
    umount jindofs-fuse
    ```

    If the `target is busy` error is returned, switch to another directory, stop all the programs that are reading data from and writing data into FUSE files, and then unmount FUSE.


