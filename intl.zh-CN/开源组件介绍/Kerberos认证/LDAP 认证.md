# LDAP 认证 {#concept_bbf_bg5_1fb .concept}

E-MapReduce 集群还支持基于 LDAP 的身份认证，通过 LDAP 来管理账号体系，Kerberos 客户端使用 LDAP 中的账号信息作为身份信息进行身份认证。

## LDAP 身份认证 {#section_vth_by5_1fb .section}

LDAP 账号可以是和其它服务共用，比如 Hue 等，只需要在 Kerberos 服务端进行相关配置即可。用户可以使用 EMR 集群中已经配置好的 LDAP 服务（ApacheDS），也可以使用已经存在的 LDAP 服务，只需要在 Kerberos 服务端进行相关配置即可。

下面以集群中已经默认启动的 LDAP 服务（ApacheDS）为例：

-   Gateway 管理对基础环境进行配置（跟第二部分RAM中的一致，如果已经配置可以跳过）

    区别的地方只是 /etc/has/has-client.conf 中的 auth\_type 需要改为 LDAP

    也可以不修改 /etc/has/has-client.conf，用户 test 在自己的账号下拷贝一份该文件进行修改 auth\_type，然后通过环境变量指定路径，如：

    `export HAS_CONF_DIR=/home/test/has-conf`

-   EMR 控制台配置 LDAP 管理员用户名/密码到 Kerberos 服务端（HAS）

    进入 EMR 控制台集群的配置管理-HAS软件下，将 LDAP 的管理员用户名和密码配置到对应的 bind\_dn 和 bind\_password 字段，然后重启 HAS 服务。

    此例中，LDAP 服务即 EMR 集群中的 ApacheDS 服务，相关字段可以从 ApacheDS 中获取。

-   EMR 集群管理员在 LDAP 中添加用户信息
    -   获取 ApacheDS 的 LDAP 服务的管理员用户名和密码在 EMR 控制台集群的配置管理 /ApacheDS 的配置中可以查看 manager\_dn 和 manager\_password
    -   在 ApacheDS 中添加 test 用户名和密码：

        ``` {#codeblock_qjh_em1_gb4}
        登录集群emr-header-1节点root账号
         新建test.ldif文件,内容如下：
         dn: cn=test,ou=people,o=emr
         objectclass: inetOrgPerson
         objectclass: organizationalPerson
         objectclass: person
         objectclass: top
         cn: test
         sn: test
         mail: test@example.com
         userpassword: test1234
         #添加到LDAP，其中-w 对应修改成密码manager_password
         ldapmodify -x -h localhost -p 10389 -D "uid=admin,ou=system" -w "Ns1aSe" -a -f test.ldif
         #删除test.ldif
         rm test.ldif
        ```

        将添加的用户名/密码提供给 test 使用

-   用户 test 配置 LDAP 信息

    ``` {#codeblock_j23_q5w_hrx}
    登录Gateway的test账号
     #执行脚本
     sh add_ldap.sh test
    ```

    附： add\_ldap.sh 脚本

    ``` {#codeblock_aey_cup_wm4}
    user=$1
     if [[ `cat /home/$user/.bashrc | grep 'export LDAP_'` == "" ]];then
     echo "
     #修改为test用户的LDAP_USER/LDAP_PWD
     export LDAP_USER=YOUR_LDAP_USER
     export LDAP_PWD=YOUR_LDAP_USER
     " >>~/.bashrc
     else
        echo $user LDAP user info has been added to .bashrc
     fi
    ```

-   用户 test 访问集群服务

    执行 hdfs 命令

    ``` {#codeblock_fgl_cj3_qn3}
    [test@iZbp1cyio18s5ymggr7yhrZ ~]$ hadoop fs -ls /
      17/11/19 13:33:33 INFO client.HasClient: The plugin type is: LDAP
      Found 4 items
      drwxr-x---   - has    hadoop          0 2017-11-18 21:12 /apps
      drwxrwxrwt   - hadoop hadoop          0 2017-11-19 13:33 /spark-history
      drwxrwxrwt   - hadoop hadoop          0 2017-11-19 12:41 /tmp
      drwxrwxrwt   - hadoop hadoop          0 2017-11-19 12:41 /user
    ```

    跑 Hadoop/Spark 作业等


