# Connect to clusters using SSH {#concept_sns_sww_y2b .concept}

If the existing jobs and execution plans cannot meet your more complex application requirements, log on to the master node of the E-MapReduce cluster. Here, navigate to the cluster details page where the public network IP address of the cluster master exists. You can log on to the master node through SSH to view various settings and states.

## The function of logining clusters {#section_q5l_htg_1fb .section}

Relevant environment variables have been set on the machine, including the following common ones:

-   JAVA\_HOME

-   HADOOP\_HOME

-   HADOOP\_CONF\_DIR

-   HADOOP\_LOG\_DIR

-   YARN\_LOG\_DIR

-   HIVE\_HOME

-   HIVE\_CONF\_DIR

-   PIG\_HOME

-   PIG\_CONF\_DIR


You can quote these variables in the script, however, we recommend that you do not change them to avoid unexpected E-MapReduce errors.

## Connect to the Master { .section}

1.  SSH logs on to the master with the following commands. Obtain the public network IP of the cluster master in the hardware information column on the [Cluster details](intl.en-US/User Guide/Cluster/Cluster details.md#).

    ```
    ssh root@ip.of.master
    ```

2.  Enter the password set during creation.

## Connect to a cluster using SSH without a password {#section_eqh_5tg_1fb .section}

You must connect to the cluster for management and operation. To connect to the cluster master, you can break through the SSH password-less logon from the master machine \(by default, the cluster master opens up the public network IP\). The procedure is as follows:

1.  Connect to the master with the root and password mode as mentioned previously.
2.  Change to Hadoop or hdfs user.

## \#\# SSH mode of Linux { .section}

1.  Copy the private key to the local machine.

    ```
    sz ~/.ssh/id_rsa
    ```

2.  Return to your local machine and attempt to connect to the master again.

    ```
    ssh -i private_key_path/id_rsa hadoop@server_ip_address
    ```

    If only one private key exits, you can put it in your ~/.ssh/ and use it by default without designation of `-i`.


## Connect to the Master Node using SSH on Windows {#section_xz1_c5g_1fb .section}

You can connect to the master through SSH without input password with multiple methods under Windows.

-   Method I: PuTTY
    1.  Click [Download PuTTY](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html).
    2.  Download PuTTYgen from the same location.
    3.  Open PuTTYgen and load your private key.

        **Note:** Keep the private key safe. In case of accidental disclosure, generate a new private key immediately for replacement.

    4.  Use default configuration and save the private key. Obtain a secret PuTTY key file with a suffix of ppk.
    5.  Operate PuTTY and select Session on the configuration page.
    6.  Enter the public network IP address of the target machine you will connect to and add the user name \(for example, hadoop@MasterNodeIP\).
    7.  Select **Connetion** \> **SSH** \> **Auth** on the configuration page.
    8.  Select the generated ppk file.
    9.  Click **Open** to log on to the master node automatically.
-   Method II: Cygwin | MinGW

    It is a convenient tool to simulate Linux env in Windows.

    For this method, see the preceding SSH method of Linux.

    MinGW method is recommended for use because it is the most compact. If the official website cannot be opened, download a Git client. The default Git Bash can be used.


## View webui of Hadoop, Spark, Ganglia, and other systems {#section_plk_45g_1fb .section}

**Note:** Confirm you have finished the preceding [SSH logon without password](#) process before this step.

For safety, the webui monitoring system ports of Hadoop, Spark, Ganglia, and other systems in the E-MapReduce cluster are not opened to the outside world. If you want to visit these webUIs, a SSH tunnel needs to be built to forward through a port. The following two methods are available:

**Note:** The following operations are completed in your local machine, instead of the machine in the cluster.

-   Method I: Port dynamic forwarding

    Create a SSH tunnel that can connect certain dynamic port connections between the local machine and the master machine in E-MapReduce cluster.

    ```
    ssh -i /path/id_xxx -ND 8157 hadoop@masterNodeIP
    ```

    8157 is any port not used in the local machine and can be customized by you.

    After dynamic forwarding, you can view the following:

    -   Recommended methods

        We recommend that you use the Chrome browser. Visit web UIs in the following methods:

        ```
        chrome --proxy-server="socks5://localhost:8157" --host-resolver-rules="MAP * 0.0.0.0 , EXCLUDE localhost" --user-data-dir=/tmp/
        ```

        For Windows system, thetmppath can be written into similar d:/tmppath. For Linux or OSX, /tmp/ can be written directly.

        The Chrome location varies with operating systems. See the following table.

        |Operating System|Chrome Location|
        |:---------------|:--------------|
        |Mac OS X|/Applications/Google Chrome.app/Contents/MacOS/Google Chrome|
        |Linux|/usr/bin/google-chrome|
        |Windows|C:\\Program Files \(x86\)\\Google\\Chrome\\Application\\chrome.exe|

    -   插件方式

        此时，您本地机器跟 E-MapReduce 集群的 master 主机的 SSH 通道已经打通，要在浏览器中查看 Hadoop、Spark、Ganglia 的 webui，您还需要配置一个本地代理。 操作步骤如下：

        1.  假设您使用的是 Chrome 或者 Firefox 浏览器，请点击[下载 FoxyProxy Standard 代理软件](http://foxyproxy.mozdev.org/downloads.html)。
        2.  安装完成并重启浏览器后，打开一个文本编辑器，编辑如下内容：

            ```
            <? xml version="1.0" encoding="UTF-8"? >
            <foxyproxy>
            <proxies>
            <proxy name="aliyun-emr-socks-proxy" id="2322596116" notes="" fromSubscription="false" enabled="true" mode="manual" selectedTabIndex="2" lastresort="false" animatedIcons="true" includeInCycle="true" color="#0055E5" proxyDNS="true" noInternalIPs="false" autoconfMode="pac" clearCacheBeforeUse="false" disableCache="false" clearCookiesBeforeUse="false" rejectCookies="false">
            <matches>
            <match enabled="true" name="120.*" pattern="http://120.*" isRegEx="false" isBlackList="false" isMultiLine="false" caseSensitive="false" fromSubscription="false" ></match>
            </matches>
            <manualconf host="localhost" port="8157" socksversion="5" isSocks="true" username="" password="" domain="" ></manualconf>
            </proxy>
            </proxies>
            </foxyproxy>
            ```

            其中：

            -   `Port 8157` 是您本地用来建立与集群 master 机器 SSH 连接的端口，这个需要跟您之前执行的在终端中执行的 SSH 命令中使用的端口匹配。
            -   `120.*` 这个匹配是用来匹配 master 主机的 IP 地址，请根据 master 的 IP 地址的情况来定。
        3.  在浏览器中单击**Foxyproxy**按钮，选择 **Options**。
        4.  选择 **Import/Export**。
        5.  选择刚才您编辑的 xml 文件，单击 **Open**。
        6.  在 **Import FoxyProxy Setting** 对话框中，单击 **Add**。
        7.  点击浏览器中的 **Foxyproxy** 按钮，选择 **Use Proxy aliyun-emr-socks-proxy for all URLs**。
        8.  在浏览器中输入 localhost:8088，就可以打开远端的 Hadoop 界面了。
-   **Method II: Local port forwarding**

    **Note:** A local port forwarding disadvantage is that only the interface in the outermost layer can be seen. The viewing of detailed job information results in an error.

    ```
    ssh -i /path/id_rsa -N -L 8157:masterNodeIP:8088 hadoop@masterNodeIP
    ```

    Parameter description:

    -   path: Private key storage path.
    -   masterNodeIP: IP address of the master node to be connected.
    -   8088: Access port number of ResourceManager on the master node.

