# LDAP认证 {#concept_bbf_bg5_1fb .concept}

EMR集群还支持基于LDAP的身份认证，通过LDAP来管理账号体系，Kerberos客户端使用LDAP中的账号信息作为身份信息进行身份认证。

## LDAP身份认证 {#section_vth_by5_1fb .section}

LDAP账号可以是和其它服务共用，比如Hue等，只需要在Kerberos服务端进行相关配置即可。用户可以使用EMR集群中已经配置好的LDAP服务\(ApacheDS\)，也可以使用已经存在的LDAP服务，只需要在Kerberos服务端进行相关配置即可。

下面以集群中已经默认启动的LDAP服务\(ApacheDS\)为例:

-   Gateway管理对基础环境进行配置\(跟第二部分RAM中的一致，如果已经配置可以跳过\)

    区别的地方只是/etc/has/has-client.conf中的auth\_type需要改为LDAP

    也可以不修改/etc/has/has-client.conf,用户test在自己的账号下拷贝一份该文件进行修改auth\_type，然后通过环境变量指定路径，如:

    `export HAS_CONF_DIR=/home/test/has-conf`

-   EMR控制台配置LDAP管理员用户名/密码到Kerberos服务端\(HAS\)

    进入EMR控制台集群的配置管理-HAS软件下，将LDAP的管理员用户名和密码配置到对应的bind\_dn和bind\_password字段，然后重启HAS服务。

    此例中，LDAP服务即EMR集群中的ApacheDS服务，相关字段可以从ApacheDS中获取。

-   EMR集群管理员在LDAP中添加用户信息
    -   获取ApacheDS的LDAP服务的管理员用户名和密码在EMR控制台集群的配置管理/ApacheDS的配置中可以查看manager\_dn和manager\_password
    -   在ApacheDS中添加test用户名和密码

        ```
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

        将添加的用户名/密码提供给test使用

-   用户test配置LDAP信息

    ```
    登录Gateway的test账号
     #执行脚本
     sh add_ldap.sh test
    ```

    附: add\_ldap.sh脚本\(修改一下LDAP账号信息\)

    ```
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

-   用户test访问集群服务

    执行hdfs命令

    ```
    [test@iZbp1cyio18s5ymggr7yhrZ ~]$ hadoop fs -ls /
      17/11/19 13:33:33 INFO client.HasClient: The plugin type is: LDAP
      Found 4 items
      drwxr-x---   - has    hadoop          0 2017-11-18 21:12 /apps
      drwxrwxrwt   - hadoop hadoop          0 2017-11-19 13:33 /spark-history
      drwxrwxrwt   - hadoop hadoop          0 2017-11-19 12:41 /tmp
      drwxrwxrwt   - hadoop hadoop          0 2017-11-19 12:41 /user
    ```

    跑Hadoop/Spark作业等


