# Python instructions {#concept_y55_psw_y2b .concept}

This section provides an overview of different Python instructions.

## Python 2.7 {#section_jz1_rsw_y2b .section}

Python 2.7 is supported in E-MapReduce 2.0.0 and later. The files are located at usr/local/Python-2.7.11/. Numpy is included.

## Python 3.6 {#section_kz1_rsw_y2b .section}

Python 3.6.4 is supported in EMR 2.10.0, 3.10.0 and later. The files are located at /usr/bin/python3. Earlier versions do not support Python 3 by default. If you want to install it, complete the following steps:

-   [Download the Python 3 package](https://www.python.org/ftp/python/3.6.4/Python-3.6.4.tgz).
-   Unzip the downloaded file using the following commands:

    ```
    tar zxvf Python-3.6.4.tgz
    cd Python-3.6.4 ./configure --prefix=/usr/local/Python-3.6.4
    make && make install
    ln -s /usr/local/Python-3.6.4/bin/python3.6 /bin/python3
    ln -s /usr/local/Python-3.6.4/bin/pip3 /bin/pip3
    ```

-   Verify the environment:

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

    If the preceding command information is displayed, you have successfully installed Python 3.


