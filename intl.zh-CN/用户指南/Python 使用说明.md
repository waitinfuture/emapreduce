# Python 使用说明 {#concept_y55_psw_y2b .concept}

Python 使用说明

## Python 2.7支持 {#section_jz1_rsw_y2b .section}

从 E-MapReduce 的 2.0.0 版本开始，支持Python 2.7版本。

Python文件安装位置为：usr/local/Python-2.7.11/ 已包含Numpy

## Python 3.6支持 {#section_kz1_rsw_y2b .section}

EMR 2.10.0、3.10.0及以上版本开始支持python 3.6.4 ，安装目录为 /usr/bin/python3；EMR 2.10.0、3.10.0以下版本默认不支持Python3版本，用户需自行下载安装：

-   下载Python3软件包：[wget链接地址](https://www.python.org/ftp/python/3.6.4/Python-3.6.4.tgz)
-   解压下载文件并安装

    ```
    tar zxvf Python-3.6.4.tgz
    cd Python-3.6.4 ./configure --prefix=/usr/local/Python-3.6.4
    make && make install
    ln -s /usr/local/Python-3.6.4/bin/python3.6 /bin/python3
    ln -s /usr/local/Python-3.6.4/bin/pip3 /bin/pip3
    ```

-   验证Python3的环境

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


