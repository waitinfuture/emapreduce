# Python instructions {#concept_y55_psw_y2b .concept}

Python instructions

## Python 2.7 {#section_jz1_rsw_y2b .section}

Supported Python 2.7 in E-MapReduce 2.0.0 and later versions.

Python files location: usr/local/Python-2.7.11/ included Numpy.

## Python 3.6 {#section_kz1_rsw_y2b .section}

EMR 2.10.0, 3.10.0 and later versions support Python 3.6.4 and the installation directory of Python is /usr/bin/python3. The earlier versions do not support Python 3 by default, and you have to download and install the package through the following link:

-   [Download Python 3 package](https://www.python.org/ftp/python/3.6.4/Python-3.6.4.tgz)
-   Unzip the downloaded file by using the following commands:

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

    Install successfully if you see these messages.


