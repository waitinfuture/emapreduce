# Use ApacheDS for authentication---机翻，需重新提翻 {#concept_bmr_b45_xgb .concept}

Presto can connect to LDAP for user password authentication. You only need to connect the Coordinato r node to LDAP.

## Main Steps {#section_byq_gp5_xgb .section}

1.  Configure ApacheDS and enable LDAPS
2.  Create user information in ApacheDS
3.  Configure Presto Coordinator and restart it to take effect.
4.  Verify Configuration

## Enable LDAPS {#section_fkx_qp5_xgb .section}

1.  Create the keystore used by the ApacheDS server. Here, all passwords use '123456 ':

    ```
    # Create a keystore
    > Cd/var/lib/apacheds-2.0.0-M24/default/conf/
    > Keytool-genkeypair-alias apacheds-keyalg RSA-validity 7-keystore ads. keystore
    
    Enter keystore password:
    
    Reenter new password:
    What is your first and last name?
      [Unknown]: apacheds
    What is the name of your organizational
      [Unknown]: apacheds
    What is the name of your organization?
      [Unknown]: apacheds
    What is the name of your city or
      [Unknown]: apacheds
    What is the name of your state or
      [Unknown]: apacheds
    What is the two-letter country code for this
      [Unknown]: CN
    Is CN = apacheds, OU = apacheds, O = apacheds, L = apacheds, ST = apacheds, C = CN correct?
      [No]: yes
    
    Enter key password for <apacheds>
    	(RETURN if same as keystore password ):
    
    Reenter new password:
    
    123Warning:
    The JKS keystore uses a proprietary format. It is recommended to migrate to PKCS12 which is an industry standard format using "keytool-importkeystore-srckeystore ads. keystore-destkeystore ads. keystore-deststoretype pkcs12 ".
    
    # Modify the file user; otherwise, ApacheDS has no permission to read the file
    > Chown apacheds: apacheds./ads. keystore
    
    # Export a certificate.
    # Enter the password. The password is set in the previous step. The value is 123456.
    > Keytool-export-alias apacheds-keystore ads. keystore-rfc-file apacheds. cer
    Enter keystore password:
    Certificate stored in file <apacheds. cer>
    
    123Warning:
    The JKS keystore uses a proprietary format. It is recommended to migrate to PKCS12 which is an industry standard format using "keytool-importkeystore-srckeystore ads. keystore-destkeystore ads. keystore-deststoretype pkcs12 ".
    
    # Import the certificate to the system certificate library for self-Authentication
    > Keytool-import-file apacheds. cer-alias apacheds-keystore/usr/lib/jvm/java-1.8.0/jre/lib/security/cacerts
    ```

2.  Modify configuration and enable LDAPS

    Open ApacheDS Studio and link to the cluster to the ApacheDS service:

    -   DN: uid = admin, ou = system
    -   View the password in this file:/Var/lib/ecm-agent/cache/ecm/service/APACHEDS/2.0.0.1.1/package/files/modifypwd. ldif

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/133788/155255209439736_en-US.png)

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/133788/155255209439737_en-US.png)

        After the link, open the configuration page, enable LDAPs, set the keystore created in step 1 to the relevant configuration, and save \(ctrl + s \).

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/133788/155255209439739_en-US.png)

3.  Restart ApacheDS

    Log on to the cluster and run the following command to restart ApacheDS:

    ```
    > Service apacheds-2.0.0-M24-default restart
    ```

    At this point, LDAPS is started, and the service port is 10636.

    **Note:** ApacheDS Studio has a Bug. When you test the LDAPS service connection on the connection property page, handshaking fails, mainly because the internal default timeout time is too short and does not affect actual use.


## Create user information {#section_rtk_hs5_xgb .section}

In this example, users are created under DN: dc = hadoop, dc = apache, dc = org.

1.  Create dc = hadoop, dc = apache, dc = org partition, open the configuration page, configure as follows, and save \(ctrl + s \). Restart the ApacheDS service.

    ![](images/39740_en-US.png)

2.  Create User

    Log on to the cluster and create the following files:/Tmp/users. ldif

    ```
    # Entry for a sample people container
    # Please replace with site specific values
    Dn: ou = people, dc = hadoop, dc = apache, dc = org
    objectclass: top
    Objectclass: organizationalUnit
    Ou: people
    
    # Entry for a sample end user
    # Please replace with site specific values
    Dn: uid = guest, ou = people, dc = hadoop, dc = apache, dc = org
    objectclass: top
    objectclass: person
    objectclass: organizationalPerson
    objectclass: inetOrgPerson
    Cn: Guest
    Sn: User
    Uid: guest
    UserPassword: guest-password
    
    # Entry for sample user admin
    Dn: uid = admin, ou = people, dc = hadoop, dc = apache, dc = org
    objectclass: top
    objectclass: person
    objectclass: organizationalPerson
    objectclass: inetOrgPerson
    Cn: Admin
    Sn: Admin
    Uid: admin
    UserPassword: admin-password
    
    # Entry for sample user sam
    Dn: uid = sam, ou = people, dc = hadoop, dc = apache, dc = org
    objectclass: top
    objectclass: person
    objectclass: organizationalPerson
    objectclass: inetOrgPerson
    Cn: sam
    Sn: sam
    Uid: sam
    UserPassword: sam-password
    
    # Entry for sample user tom
    Dn: uid = tom, ou = people, dc = hadoop, dc = apache, dc = org
    objectclass: top
    objectclass: person
    objectclass: organizationalPerson
    objectclass: inetOrgPerson
    Cn: tom
    Sn: tom
    Uid: tom
    UserPassword: tom-password
    
    # Create FIRST Level groups branch
    Dn: ou = groups, dc = hadoop, dc = apache, dc = org
    objectclass: top
    Objectclass: organizationalUnit
    Ou: groups
    Description: generic groups branch
    
    # Create the analyst group under groups
    Dn: cn = analyst, ou = groups, dc = hadoop, dc = apache, dc = org
    objectclass: top
    Objectclass: groupofnames
    Cn: analyst
    Description: analyst group
    Member: uid = sam, ou = people, dc = hadoop, dc = apache, dc = org
    Member: uid = tom, ou = people, dc = hadoop, dc = apache, dc = org
    
    
    # Create the scientist group under groups
    Dn: cn = scientist, ou = groups, dc = hadoop, dc = apache, dc = org
    objectclass: top
    Objectclass: groupofnames
    Cn: scientist
    Description: scientist group
    Member: uid = sam, ou = people, dc = hadoop, dc = apache, dc = or
    ```

    Run the following command to import the user:

    ```
    ldapmodify -x -h localhost -p 10389 -D "uid=admin,ou=system" -w "Ns1aSe" -a -f test.ldif
    ```

    After the execution is complete, you can see the relevant users on ApacheDS Studio, as shown below:

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/133788/155255209439753_en-US.png)


## Configure Presto {#section_dll_zrz_xgb .section}

1.  Enable Coordinator Https
    1.  Create the keystore used by Presto coordinator

        ```
        # Use the script provided by EMR to generate a keystore
        --keystore-path /etc/ecm/presto-conf/keystore \
        # Keystore password: 81ba14ce6084
        > Keep CT/var/lib/ecm-agent/cache/ecm/service/PRESTO/0.208.0.1.2/package/files/tools/gen-keystore.exp
        ```

    2.  Configure Presto coordinator

        Edit/Etc/ecm/presto-conf/config. properties, Add the following content:

        ```
        Http-server.https.enabled = true
        Http-server.https.port = 7778
        
        Http-server.https.keystore.path =/etc/ecm/presto-conf/keystore
        Http-server.https.keystore.key = 81ba14ce6084.
        ```

2.  Configure Authentication Mode to access ApacheDS

    1.  Edit/Etc/ecm/presto-conf/config. properties, Add the following content:

        ```
        Http-server.authentication.type = PASSWORD
        ```

    2.  EditJvm. config, Add the following content:

        ```
        -Djavax.net. ssl. trustStore =/usr/lib/jvm/java-1.8.0/jre/lib/security/cacerts
        -Djavax.net. ssl. trustStorePassword = changeit
        ```

    3.  CreatePassword-authenticator.properties, Add the following content:

        ```
        Password-authenticator.name = ldap
        Ldap. url = ldaps: // emr-header-1.cluster-84423: 10636
        Ldap. user-bind-pattern = uid =$ {USER}, ou = people, dc = hadoop, dc = apache, dc = org
        ```

    4.  CreateJndi. properties, Add the following content:

        ```
        Java. naming. security. principal = uid = admin, ou = system
        Java. naming. security. credentials = {password}
        Java. naming. security. authentication = simple
        ```

    5.  SetJndi. propertiesPackage it into the jar package and copy it to the presto library file directory:

        ```
        Jar-cvf jndi-properties.jar jndi. properties
        > Cp./jndi-properties.jar/etc/ecm/presto-current/lib/
        ```

    **Note:** 

    -   The following three parameters are used to log on to the LDAP service. However, there is no place to configure these parameters on Presto. You can add these parameters to the jvm parameters for source code analysis. \( Will be filtered out\): java. naming. security. principal = uid = admin, ou = system java. naming. security. credentials = ZVixyOY + 5 k java. naming. security. authentication = simple
    -   Further code analysis showed that the JNDI library will use classload to load the resource file jndi. properties. Therefore, you can put these parameters in the jndi. properties file;
    -   Presto launcher only adds the jar file to classpath. Therefore, you need to compress this jndi. properties into a jar package and copy it to the lib directory.
3.  Restart Presto to complete all configurations.

## Verify Configuration {#section_ipx_4vz_xgb .section}

Use Presto cli to verify whether the configuration takes effect.

```
# Use user sam and enter the correct password
> Presto -- server https: // emr-header-1: 7778 -- keystore-path/etc/ecm/presto-conf/keystore -- keystore-password 81ba14ce6084 -- catalog hive -- schema default -- user sam -- password
Password: <enter the correct Password>
Presto: Default> show schemas;
              schema.
----------------------------------
 Tpcds_bin_partitioned_orc_5
 Tpcds_oss_bin_partitioned_orc_10
 Tpcds_oss_text_10
 Tpcds_text_5
 Tst
(5 rows)

Query 20181115_030713_1_2_kp5ih, FINISHED, 3 nodes
Splits: 36 total, 36 done (100.00%)
0: 00 [20 rows, 331B] [41 rows/s, 694B/s]

# Use user sam and enter the wrong password
> Presto -- server https: // emr-header-1: 7778 -- keystore-path/etc/ecm/presto-conf/keystore -- keystore-password 81ba14ce6084 -- catalog hive -- schema default -- user sam -- password
Password: <enter the wrong Password>
Presto: Default> show schemas;
Error running command: Authentication failed: Access Denied: Invalid credentials
```

