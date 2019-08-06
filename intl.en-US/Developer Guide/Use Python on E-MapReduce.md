# Use Python on E-MapReduce {#concept_y55_psw_y2b .concept}

You can use Python on E-MapReduce \(EMR\) 2.0.0 or later. This topic describes how to install Python.

## Python 2.7 {#section_jz1_rsw_y2b .section}

You can use Python 2.7 on EMR 2.0.0 or later.

By default, Python is installed in the usr/local/Python-2.7.11/ directory and the NumPy package is included.

## Python 3.6 {#section_kz1_rsw_y2b .section}

You can use Python 3.6.4 on EMR 2.10.0 or later and 3.10.0 or later. By default, Python 3.6.4 is installed in the /usr/bin/python3.6 directory. You can use the following command to check whether Python 3 is installed on your instance.

``` {#codeblock_fvu_u0d_dqu}
[root@emr-header-1 ~]# python36
```

The pip3 tool is not preinstalled by default. You can install the tool as required.

By default, you cannot use Python 3 on versions earlier than EMR 2.10.0 or EMR 3.10.0. You must download and install Python 3 as follows:

1.  Download the installation package of Python 3 from [this link](https://www.python.org/ftp/python/3.6.4/Python-3.6.4.tgz).
2.  Extract the downloaded file and install Python 3

    ``` {#codeblock_uf6_7sp_nk6}
    tar zxvf Python-3.6.4.tgz
    cd Python-3.6.4 ./configure --prefix=/usr/local/Python-3.6.4
    make && make install
    ln -s /usr/local/Python-3.6.4/bin/python3.6 /bin/python3
    ln -s /usr/local/Python-3.6.4/bin/pip3 /bin/pip3
    ```

3.  Verify the installation result of Python 3

    ``` {#codeblock_uzo_a3h_zil}
    [root@emr-header-1 bin]# python3
    ```

    ``` {#codeblock_cf0_kvn_l2n}
    Python 3.6.4 (default, Mar 12 2018, 14:03:26)
    [GCC 4.8.5 20150623 (Red Hat 4.8.5-16)] on linuxType “help”, “copyright”, “credits” or “license” for more information.
    						
    ```

    ``` {#codeblock_lvv_a04_c6h}
    [root@emr-header-1 bin]# pip3 -V
    ```

    ``` {#codeblock_8gn_9rq_sls}
    pip 9.0.1 from /usr/local/Python-3.6.4/lib/python3.6/site-packages (python 3.6)
    ```

    The preceding result indicates a successful installation.


