# Python 使用说明 {#concept_y55_psw_y2b .concept}

E-MapReduce 从 2.0.0 版本开始，支持 Python 的使用。本文将介绍 Python 安装使用说明。

## Python 2.7 支持 {#section_jz1_rsw_y2b .section}

从 E-MapReduce 的 2.0.0 版本开始，支持 Python 2.7版本。

Python 文件安装位置为：usr/local/Python-2.7.11/ 已包含 Numpy

## Python 3.6 支持 {#section_kz1_rsw_y2b .section}

EMR 2.10.0、3.10.0 及以上版本开始支持 Python 3.6.4 ，安装目录为 /usr/bin/python3；EMR 2.10.0、3.10.0 以下版本默认不支持 Python 3 版本，用户需自行下载安装，步骤如下：

-   下载 Python 3 软件包：[wget 链接地址](https://www.python.org/ftp/python/3.6.4/Python-3.6.4.tgz)
-   解压下载文件并安装

    ```
    tar zxvf Python-3.6.4.tgz
    cd Python-3.6.4 ./configure --prefix=/usr/local/Python-3.6.4
    make && make install
    ln -s /usr/local/Python-3.6.4/bin/python3.6 /bin/python3
    ln -s /usr/local/Python-3.6.4/bin/pip3 /bin/pip3
    ```

-   验证 Python 3 的环境

    ```
    [root@emr-header-1 bin]# python3
    ```

    ```
    Python 3.6.4 (default, Mar 12 2018, 14:03:26)
    [GCC 4.8.5 20150623 (Red Hat 4.8.5-16)] on linuxType “help”, “copyright”, “credits” or “license” for more information.
    						
    ```

    ```
    [root@emr-header-1 bin]# pip3 -V
    ```

    ```
    pip 9.0.1 from /usr/local/Python-3.6.4/lib/python3.6/site-packages (python 3.6)
    ```

    输出以上信息说明安装成功。


