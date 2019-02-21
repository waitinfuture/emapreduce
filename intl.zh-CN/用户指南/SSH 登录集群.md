# SSH 登录集群 {#concept_sns_sww_y2b .concept}

若您觉得在网页上的作业和执行计划无法满足您更加复杂的应用需求，您可以登录到 E-MapReduce 集群的主机上，找到集群的详情页，其中就有集群 master 机器的公网 IP 地址，您可以直接 SSH 登录到这台机器上，查看各种设置与状态。

## 登录集群内部的用途 {#section_q5l_htg_1fb .section}

机器上已为您设置好相关的环境变量，其中包括以下常用的环境变量：

-   JAVA\_HOME

-   HADOOP\_HOME

-   HADOOP\_CONF\_DIR

-   HADOOP\_LOG\_DIR

-   YARN\_LOG\_DIR

-   HIVE\_HOME

-   HIVE\_CONF\_DIR

-   PIG\_HOME

-   PIG\_CONF\_DIR


您可以在脚本中直接引用这些变量，但请不要去修改这些变量的值，以免造成 E-MapReduce 的意外错误。

## 登录 master 主机步骤 { .section}

1.  使用如下命令 SSH 登录到 master 主机。请在[集群详情页](intl.zh-CN/用户指南/集群/集群详情.md#)的主机信息栏中获取集群 master 机器的公网 IP。

    ```
    ssh root@ip.of.master
    ```

2.  输入创建时设定的密码。

## 打通本地机器与集群 master 机器的 SSH 无密码登录 {#section_eqh_5tg_1fb .section}

通常，您需要登录到集群上进行一些管理和操作。为便于登录集群 master 机器，您可打通与 master 机器的 SSH 无密码登录（集群 master 机器默认开通了公网 IP）。操作步骤如下：

1.  通过上面提到的 root + 密码的方式登录到 master 主机。
2.  切换到到Hadoop 用户或者 hdfs 用户。

## Linux 的 SSH 方式 { .section}

1.  复制私钥到本地。

    ```
    sz ~/.ssh/id_rsa
    ```

2.  回到您的本地机器，尝试重新登录 master 机器。

    ```
    ssh -i 私钥存放路径/id_rsa hadoop@120.26.221.130
    ```

    当然如果你只有这一个私钥，也可以直接放到你的 ~/.ssh/ 下，默认使用这个私钥，就不需要 `-i` 指定了。


## Windows 的 SSH 方式 {#section_xz1_c5g_1fb .section}

在 Windows 下你可以有多种方式来使用 SSH 免密码登录 master 机器。

-   方式一：使用 PuTTY
    1.  [下载 PuTTY](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html)。
    2.  在同样的位置下载 PuTTYgen。
    3.  打开 PuTTYgen，并 Load 您的私钥。

        **说明：** 请妥善保管这个私钥，保证该私钥的安全。若私钥不幸泄漏了，请立刻重新生成一个新的取代。

    4.  使用默认的配置，并 Save private key。会保存出一个后缀为 ppk 的 PuTTY 使用的密钥文件。
    5.  运行 PuTTY，并在配置页面选择 Session。
    6.  输入您要连接的目标机器公网 IP 地址，要加上登录使用的用户名，类似 hadoop@MasterNodeIP。
    7.  在配置页面，选择**Connetion** \> **SSH** \> **Auth**。
    8.  选择之前生成好的 ppk 文件。
    9.  最后单击 **Open**，就会自动登录到 master 节点了。
-   方式二：使用 Cygwin | MinGW

    这是在 Windows 上模拟 Linux 的非常方便的工具，使用起来也非常简单。

    如果采用这种方式，连接过程就可以参考上面的 Linux 的 SSH 方式了。

    推荐采用 MinGW 的方式，这个是最小巧的一种方式。如果官网打不开，可以下载 git 的客户端，默认带的 Git Bash 就可以满足。


## 查看 Hadoop、Spark、Ganglia 等系统的 webui {#section_plk_45g_1fb .section}

**说明：** 在进行本步骤前，请确认您已经完成了上面的 [SSH 无密码登录](#)流程。

由于安全的缘故，E-MapReduce 集群的 Hadoop、Spark 和 Ganglia 等系统的 webui 监控系统的端口都没有对外开放。如果用户想要访问这些 webui，需要建立一个 SSH 隧道，通过端口转发的方式来达到目的。有如下两种方式：

**说明：** 下面的操作是在您本地机器上完成的，不是集群内部机器。

-   方式一：端口动态转发

    -   密钥的方式

        创建一个 SSH 隧道，该隧道可打通您本地机器跟 E-MapReduce 集群的 master 机器的某个动态端口的连接：

        ```
        ssh -i /path/id_xxx -ND 8157 hadoop@masterNodeIP
        ```

    -   用户名密码的方式，需要输入密码：

        ```
        ssh -ND 8157 hadoop@masterNodeIP
        ```

    8157 是您本地机器没有被使用过的任何一个端口，用户可以自定定义。

    完成动态转发以后，您可以选择如下两种方式来查看。

    -   推荐方式

        推荐使用 Chrome 浏览器，可以使用如下的方式来访问 Web UI：

        ```
        chrome --proxy-server="socks5://localhost:8157" --host-resolver-rules="MAP * 0.0.0.0 , EXCLUDE localhost" --user-data-dir=/tmp/
        ```

        若是 Windows 系统，这里的 tmppath 可以写成类似 d:/tmppath，若是 Linux 或者 OSX，可以直接写成 /tmp/。

        在不同的操作系统中，Chrome 的位置不同，请参见下表：

        |操作系统|Chrome 位置|
        |:---|:--------|
        |Mac OS X|/Applications/Google Chrome.app/Contents/MacOS/Google Chrome|
        |Linux|/usr/bin/google-chrome|
        |Windows|C:\\Program Files \(x86\)\\Google\\Chrome\\Application\\chrome.exe|

    -   插件方式

        -   使用Chome 插件查看Web UI:

            1.  首先安装 Chrome 的插件，[SwitchyOmega](https://chrome.google.com/webstore/detail/proxy-switchyomega/padekgcemlokbadohgkifijomclgjgif?hl)。
            2.  安装完成以后，单击插件，在弹出框中选择**选项**开始进行配置。
            3.  单击**新建情景模式**，命名为 SSH tunnel，类型选择 PAC情景模式。
            4.  配置以下内容到 PAC 脚本中：

                ```
                function regExpMatch(url, pattern) {    
                  try { return new RegExp(pattern).test(url); } catch(ex) { return false; }    
                }
                  
                function FindProxyForURL(url, host) {
                    // Important: replace 172.31 below with the proper prefix for your VPC subnet
                
                    if (shExpMatch(url, "*localhost*")) return "SOCKS5 localhost:8157";
                    if (shExpMatch(url, "*emr-header*")) return "SOCKS5 localhost:8157";
                    if (shExpMatch(url, "*emr-worker*")) return "SOCKS5 localhost:8157";
                
                    return 'DIRECT';
                }
                ```

            5.  在左侧单击**应用选项**完成配置。
            6.  运行一个命令行，在命令行选择一种方式运行以下的命令：

                ```
                // 方式1，密钥的方式
                ssh -i /path/id_xxx -ND 8157 hadoop@masterNodeIP
                ```

                ```
                // 方式2，用户名密码的方式，需要输入密码
                ssh -ND 8157 hadoop@masterNodeIP
                ```

            7.  命令行跑起来以后，在 Chrome 上单击插件，切换到之前创建的 SSH tunnel 情景模式下。
            8.  在浏览器地址栏输入机器 IP 地址 + 端口  即可访问。这个 IP 就是 SSH 命令行去连接的机器IP，一般是 Master 节点 ，例如 YARN :8088，HDFS 是：50070，等等。
            这个方式下，正常网页浏览和集群 WebUI 访问互不干扰，可以同时使用。

        -   此时，您本地机器跟 E-MapReduce 集群的 master 主机的 SSH 通道已经打通，要在浏览器中查看 Hadoop、Spark、Ganglia 的 WebUI，您还需要配置一个本地代理。操作步骤如下：

1.  假设您使用的是 Chrome 或者 Firefox 浏览器，请点击[下载 FoxyProxy Standard 代理软件](http://foxyproxy.mozdev.org/downloads.html)。
2.  安装完成并重启浏览器后，打开一个文本编辑器，编辑如下内容：

    ```
    <?xml version="1.0" encoding="UTF-8"?>
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
-   方式二：本地端口转发

    **说明：** 这个方式的缺陷是只能看到最外层的界面，一旦要看详细的作业信息，就会出错。

    -   密钥的方式：

        ```
        ssh -i /path/id_rsa -N -L 8157:masterNodeIP:8088 hadoop@masterNodeIP
        ```

    -   用户名密码的方式，需要输入密码：

        ```
        ssh -N -L 8157:masterNodeIP:8088 hadoop@masterNodeIP 
        ```

    参数说明：

    -   path：私钥存放路径。
    -   masterNodeIP：要连接的 master节点 IP。
    -   8088：是 master 节点上 ResourceManager 的访问端口号。

