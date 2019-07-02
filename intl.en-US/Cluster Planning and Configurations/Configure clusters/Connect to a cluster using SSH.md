# Connect to a cluster using SSH {#concept_sns_sww_y2b .concept}

You can connect to the master node using SSH to view detailed settings and status of the jobs in the CLI. The public IP address of the master node is shown on the cluster overview page.

## Environment variables on a cluster {#section_q5l_htg_1fb .section}

Environment variables have been configured on the instances. Frequently-used environment variables are as follows.

-   JAVA\_HOME

-   HADOOP\_HOME

-   HADOOP\_CONF\_DIR

-   HADOOP\_LOG\_DIR

-   YARN\_LOG\_DIR

-   HIVE\_HOME

-   HIVE\_CONF\_DIR

-   PIG\_HOME

-   PIG\_CONF\_DIR


You can reference the variables in the script. We recommend that you do not modify the values of the variables to avoid unexpected errors in the EMR cluster.

## Connect to the master node {#section_2wn_p5g_77q .section}

1.  Run the following command to connect to the master node using SSH. Check the public IP address of the master node in the Host section on the [Cluster list and details](reseller.en-US/Cluster Planning and Configurations/Configure clusters/Cluster list and details.md#) page.

    ``` {#codeblock_kvn_7ux_7f7}
    ssh root@ip.of.master
    ```

2.  Enter the password you have set when creating the cluster.

## Connect to the master node using SSH without a password {#section_eqh_5tg_1fb .section}

Typically, you need to connect to the cluster for management and operations. You can quickly connect to the master node by using SSH without a password. The master node in a cluster is assigned with a public IP address by default. Procedure:

1.  Connect to the master node as the root user by using a password.
2.  Switch to the Hadoop user or the HDFS user.

## Use SSH on Linux {#section_338_lnr_sg9 .section}

1.  Send the private key file to your local system.

    ``` {#codeblock_7wk_r24_qn9}
    sz ~/.ssh/id_rsa
    ```

2.  Go back to your local machine and connect to the master node again.

    ``` {#codeblock_nsw_zzh_72b}
    ssh -i private_key_path/id_rsa hadoop@120.26.221.130
    ```

    If you only have one private key file, you can store it under the ~/.ssh/ directory for immediate use without specifying the key path using the `-i` option every time.


## Use SSH on Windows {#section_xz1_c5g_1fb .section}

You have multiple methods to connect to the master node using SSH without a password on Windows. The methods are shown as follows.

-   Method 1: Use PuTTY
    1.  Click [Download PuTTY](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html).
    2.  Download PuTTYgen through this link.
    3.  Start PuTTYgen and load your private key.

        **Note:** Keep the private key safe. Generate a new key once the private key is stolen.

    4.  Save the private key with the default configurations. A PuTTY private key file with the .ppk extension is generated.
    5.  Run PuTTY and choose Session on the configuration page.
    6.  Enter the public IP address of the node that you want to connect to with a logon username. For example, hadoop@MasterNodeIP.
    7.  Select **Connetion** \> **SSH** \> **Auth** on the configuration page.
    8.  Select the generated PPK file.
    9.  Click **Open** to connect to the master node.
-   Method 2: Use Cygwin or MinGW

    Cygwin and MinGW are easy-to-use tools for compiling Linux on Windows.

    You can refer to the operations described in the Use SSH on Linux section.

    We recommend that you use MinGW, which is the most light-weight method for using SSH on Windows. If you cannot load the official website, download Git for Windows and use the built-in Git BASH instead.


## View the Web UIs of Hadoop, Spark, and Ganglia {#section_plk_45g_1fb .section}

**Note:** Make sure that you have completed the preceding [Connect to the master node using SSH without a password](#section_eqh_5tg_1fb) procedure.

For security reasons, the ports of Web UIs for monitoring Hadoop, Spark, and Ganglia in an EMR cluster are closed. If you want to access these Web UIs, you need to create SSH tunnels and enable port forwarding. The methods are shown as follows:

**Note:** The following operations are based on your local machine, not on a cluster node.

-   Method A: Dynamic port forwarding

    -   Use a private key:

        Create an SSH tunnel to allow communication between your local machine and a dynamic port on the master node in the EMR cluster.

        ``` {#codeblock_f2z_yoo_sl4}
        ssh -i /path/id_xxx -ND 8157 hadoop@masterNodeIP
        ```

    -   Use a username and password:

        ``` {#codeblock_pfh_jiv_0zv}
        ssh -ND 8157 hadoop@masterNodeIP
        ```

    Replace 8157 with any port number on your local machine that has not been used.

    After dynamic port forwarding is complete, you can view a Web UI using the following methods.

    -   Recommended method:

        We recommend that you use the Chrome browser. You can run the following command to access a Web UI.

        ``` {#codeblock_czi_3tj_mwn}
        chrome --proxy-server="socks5://localhost:8157" --host-resolver-rules="MAP * 0.0.0.0 , EXCLUDE localhost" --user-data-dir=/tmp/
        ```

        For Windows, an example of the temporary file path is d:/tmppath. For Linux and OSX, the format of the temporary file path is like /tmp/.

        Chrome is installed on different paths based on the operating system. For more details, see the following table.

        |OS|Chrome path|
        |:-|:----------|
        |Mac OS X|/Applications/Google Chrome.app/Contents/MacOS/Google Chrome|
        |Linux|/usr/bin/google-chrome|
        |Windows|C:\\Program Files \(x86\)\\Google\\Chrome\\Application\\chrome.exe|

    -   Extension:

        -   Use a Chrome extension to view the Web UIs.

            1.  Install the Chrome extension [SwitchyOmega](https://chrome.google.com/webstore/detail/proxy-switchyomega/padekgcemlokbadohgkifijomclgjgif?hl).
            2.  After installation is complete, click the icon of SwitchyOmega and click **Options** in the pop-up menu to perform configurations.
            3.  Click **New Profile**. In the Profile name field, enter "SSH tunnel." Click PAC Profile for the type of the profile.
            4.  In the PAC Script editor, enter the following code.

                ``` {#codeblock_781_y86_fdh}
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

            5.  In the left-side navigation pane, click **Apply changes** to complete the configurations.
            6.  Start a CLI. Choose one of the following methods and run the corresponding command.

                ``` {#codeblock_h8l_emp_vot}
                //Method a: Use a private key
                ssh -i /path/id_xxx -ND 8157 hadoop@masterNodeIP
                ```

                ``` {#codeblock_9bc_2ju_3c1}
                //Method b: Use a username and password
                ssh -ND 8157 hadoop@masterNodeIP
                ```

            7.  Click the SwitchyOmega icon in the Chrome menu. From the drop-down list, select SSH tunnel.
            8.  In the address bar, enter the IP address of the node and the port number to access the Web UI. The node refers to the one that you want to connect to using the SSH command. Generally, it is a master node. Two frequently-used ports are port 8088 for YARN and port 50070 for HDFS.
            By using this method, you can browse webpages and use Web UIs of clusters at the same time.

        -   After creating an SSH tunnel between your local machine and the master node of the EMR cluster, you need to configure a local agent for viewing the Web UIs of Hadoop, Spark, and Ganglia through browsers. Procedure:

1.  Assume that you use Chrome or Firefox. Click [Download FoxyProxy Standard](http://foxyproxy.mozdev.org/downloads.html).
2.  After the installation is complete, restart your browser, open a text editor, and then enter the following code.

    ``` {#codeblock_tdh_kl1_2ht}
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

    In this example:

    -   Port `8157` on your local machine is used to establish an SSH tunnel to the master node of the cluster.
    -   Replace `120.*` with the actual IP address of the master node.
3.  Click the **Foxyproxy** icon in the Chrome menu. From the drop-down list, select **Options**.
4.  Select **Import/Export**.
5.  Select the XML file you have created. Click **Open**.
6.  In the **Import FoxyProxy Setting** dialogue box, click **Add**.
7.  Click the **Foxyproxy** icon. From the drop-down list, select **Use Proxy aliyun-emr-socks-proxy for all URLs**.
8.  Enter localhost: 8088 in the address bar to view the Web UI of Hadoop.
-   Method 2: Local port forwarding

    **Note:** If you use this method to view a Web UI, errors will occur when you try to open the subpages.

    -   Use a private key:

        ``` {#codeblock_3ya_t3l_7wu}
        ssh -i /path/id_rsa -N -L 8157:masterNodeIP:8088 hadoop@masterNodeIP
        ```

    -   Use a username and password:

        ``` {#codeblock_jxr_g81_aaf}
        ssh -N -L 8157:masterNodeIP:8088 hadoop@masterNodeIP 
        ```

    Parameter description:

    -   path: The path where the private key file is stored.
    -   masterNodeIP: The IP address of the master node to be connected.
    -   8088: The port that is used by the Resource Manager Web UI on the master node.

