# Implement authentication with ApacheDS {#concept_bmr_b45_xgb .concept}

For username and password authentication, you only need to connect the Presto coordinator to the LDAP server.

## Procedure {#section_byq_gp5_xgb .section}

1.  Configure ApacheDS and enable LDAPS.
2.  Create user information in ApacheDS.
3.  Configure Presto Coordinator. Restart Presto Coordinator.
4.  Verify the configurations

## Enable LDAPS {#section_fkx_qp5_xgb .section}

1.  Create a keystore used for ApacheDS server. The following example uses 123456 as the password.

    ```
    ## Create a keystore
    > cd /var/lib/apacheds-2.0.0-M24/default/conf/
    > keytool -genkeypair -alias apacheds -keyalg RSA -validity 7 -keystore ads.keystore
    
    Enter keystore password:
    Re-enter new password:
    What is your first and last name?
      [Unknown]:  apacheds
    What is the name of your organizational unit?
      [Unknown]:  apacheds 
    What is the name of your organization?
      [Unknown]:  apacheds
    What is the name of your City or Locality?
      [Unknown]:  apacheds
    What is the name of your State or Province?
      [Unknown]:  apacheds
    What is the two-letter country code for this unit?
      [Unknown]:  CN
    Is CN=apacheds, OU=apacheds, O=apacheds, L=apacheds, ST=apacheds, C=CN correct?
      [no]:  yes
    
    Enter key password for <apacheds>
    	(RETURN if same as keystore password):
    Re-enter new password:
    
    Warning:
    The JKS keystore uses a proprietary format. It is recommended to migrate to PKCS12 which is an industry standard format using "keytool -importkeystore -srckeystore ads.keystore -destkeystore ads.keystore -deststoretype pkcs12".
    
    ## Change the owner of the keystore file to "apacheds".
    > chown apacheds:apacheds ./ads.keystore
    
    ## Export the certificate.
    ## Enter the password. The password is set in the previous step: 123456.
    > keytool -export -alias apacheds -keystore ads.keystore -rfc -file apacheds.cer 
    Enter keystore password:
    Certificate stored in file <apacheds.cer>
    
    Warning:
    The JKS keystore uses a proprietary format. It is recommended to migrate to PKCS12 which is an industry standard format using "keytool -importkeystore -srckeystore ads.keystore -destkeystore ads.keystore -deststoretype pkcs12".
    
    ## Import the certificate to the cacerts file for self-authentication.
    > keytool -import -file apacheds.cer -alias apacheds -keystore /usr/lib/jvm/java-1.8.0/jre/lib/security/cacerts
    ```

2.  Modify the configurations. Enable LDAPS.

    Start Apache Directory Studio. Connect to the ApacheDS service on the cluster.

    -   Specify the value of DN to "uid=admin,ou=system"
    -   You can view the password under the following path: /var/lib/ecm-agent/cache/ecm/service/APACHEDS/2.0.0.1.1/package/files/modifypwd.ldif

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/133788/155712254439736_en-US.png)

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/133788/155712254439737_en-US.png)

        After the connection is complete, go to the Configuration page. On the page, select the Enable LDAPS Server check box. In the SSL/Start TLS Keystore section, select the keystore file you created in Step 1 and enter the password. Press Ctrl+S to save the configurations.

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/133788/155712254439739_en-US.png)

3.  Restart ApacheDS

    Log on to the cluster. Run the following command to restart ApacheDS.

    ```
    > service apacheds-2.0.0-M24-default restart
    ```

    LDAPS has been started. The port number is 10636.

    **Note:** Handshake exceptions are thrown when you test LDAPS connection in Apache Directory Studio. The cause is that the default timeout value is very short. Actual use is not affected.


## Create user information {#section_rtk_hs5_xgb .section}

In this example, users are created based on DN: dc=hadoop,dc=apache,dc=org.

1.  Go to the Partitions configuration page, perform configurations as shown in the following figure, and press CRTL+S to save the configurations. By doing this, you create a partition with the suffix "dc=hadoop,dc=apache,dc=org". Restart ApacheDS.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/133788/155712254439740_en-US.png)

2.  Create User

    Log on to the cluster. Create a file named /tmp/users.ldif.

    ```
    # Entry for a sample people container
    # Please replace with site specific values
    dn: ou=people,dc=hadoop,dc=apache,dc=org
    objectclass:top
    objectclass:organizationalUnit
    ou: people
    
    # Entry for a sample end user
    # Please replace with site specific values
    dn: uid=guest,ou=people,dc=hadoop,dc=apache,dc=org
    objectclass:top
    objectclass:person
    objectclass:organizationalPerson
    objectclass:inetOrgPerson
    cn: Guest
    sn: User
    uid: guest
    userPassword:guest-password
    
    # entry for sample user admin
    dn: uid=admin,ou=people,dc=hadoop,dc=apache,dc=org
    objectclass:top
    objectclass:person
    objectclass:organizationalPerson
    objectclass:inetOrgPerson
    cn: Admin
    sn: Admin
    uid: admin
    userPassword:admin-password
    
    # entry for sample user sam
    dn: uid=sam,ou=people,dc=hadoop,dc=apache,dc=org
    objectclass:top
    objectclass:person
    objectclass:organizationalPerson
    objectclass:inetOrgPerson
    cn: sam
    sn: sam
    uid: sam
    userPassword:sam-password
    
    # entry for sample user tom
    dn: uid=tom,ou=people,dc=hadoop,dc=apache,dc=org
    objectclass:top
    objectclass:person
    objectclass:organizationalPerson
    objectclass:inetOrgPerson
    cn: tom
    sn: tom
    uid: tom
    userPassword:tom-password
    
    # create FIRST Level groups branch
    dn: ou=groups,dc=hadoop,dc=apache,dc=org
    objectclass:top
    objectclass:organizationalUnit
    ou: groups
    description: generic groups branch
    
    # create the analyst group under groups
    dn: cn=analyst,ou=groups,dc=hadoop,dc=apache,dc=org
    objectclass:top
    objectclass: groupofnames
    cn: analyst
    description:analyst  group
    member: uid=sam,ou=people,dc=hadoop,dc=apache,dc=org
    member: uid=tom,ou=people,dc=hadoop,dc=apache,dc=org
    
    
    # create the scientist group under groups
    dn: cn=scientist,ou=groups,dc=hadoop,dc=apache,dc=org
    objectclass:top
    objectclass: groupofnames
    cn: scientist
    description: scientist group
    member: uid=sam,ou=people,dc=hadoop,dc=apache,dc=or
    ```

    Run the following command to import the users.

    ```
    > ldapmodify -x -h localhost -p 10389 -D "uid=admin,ou=system" -w {password} -a -f /tmp/users.ldif
    ```

    You can view the users in Apache Directory Studio as shown in the following figure.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/133788/155712254439753_en-US.png)


## Configure Presto {#section_dll_zrz_xgb .section}

1.  Enable Coordinator Https
    1.  Configure the keystore used for the Presto coordinator.

        ```
        ## Use the script that comes with EMR to generate a keystore.
        ## keystore path: /etc/ecm/presto-conf/keystore
        ## keystore password: 81ba14ce6084
        > expect /var/lib/ecm-agent/cache/ecm/service/PRESTO/0.208.0.1.2/package/files/tools/gen-keystore.exp
        ```

    2.  Configure the Presto coordinator.

        Enter the following lines in the /etc/ecm/presto-conf/config.properties file.

        ```
        http-server.https.enabled=true
        http-server.https.port=7778
        
        http-server.https.keystore.path=/etc/ecm/presto-conf/keystore
        http-server.https.keystore.key=81ba14ce6084
        ```

2.  Configure the authentication type.

    1.  Enter the following configurations in the/etc/ecm/presto-conf/config.properties file.

        ```
        http-server.authentication.type=PASSWORD
        ```

    2.  Enter the following configurations in the jvm.config file.

        ```
        -Djavax.net.ssl.trustStore=/usr/lib/jvm/java-1.8.0/jre/lib/security/cacerts
        -Djavax.net.ssl.trustStorePassword=changeit
        ```

    3.  Create the password-authenticator.properties file and enter the following configurations.

        ```
        password-authenticator.name=ldap
        ldap.url=ldaps://emr-header-1.cluster-84423:10636
        ldap.user-bind-pattern=uid=${USER},ou=people,dc=hadoop,dc=apache,dc=org
        ```

    4.  Create the jndi.properties file and enter the following configurations.

        ```
        java.naming.security.principal=uid=admin,ou=system
        java.naming.security.credentials={password}
        java.naming.security.authentication=simple
        ```

    5.  Use the jndi.properties file to create a JAR file. Copy the JAR file to the Presto library.

        ```
        jar -cvf jndi-properties.jar jndi.properties
        > cp ./jndi-properties.jar /etc/ecm/presto-current/lib/
        ```

    **Note:** 

    -   The following parameters are used to connect to LDAP. However, you cannot configure these parameters in Presto. Parameters added to the jvm.config file do not take effect. java.naming.security.principal=uid=admin,ou=system java.naming.security.credentials=ZVixyOY+5k java.naming.security.authentication=simple
    -   JNDI uses classloaders to load the jndi.properties file. Add these parameters to the jndi.properties file.
    -   Presto launchers only add JAR files to the classpath. You need to package jndi.properties as a JAR file and include it in the lib directory.
3.  Restart Presto.

## Verify the configurations {#section_ipx_4vz_xgb .section}

Use the Presto CLI to verify whether the configurations take effect.

```
## Correct password
> presto  --server https://emr-header-1:7778  --keystore-path /etc/ecm/presto-conf/keystore --keystore-password 81ba14ce6084 --catalog hive --schema default --user sam --password
Password: <correct password>
presto:default> show schemas;
              Schema
----------------------------------
 tpcds_bin_partitioned_orc_5
 tpcds_oss_bin_partitioned_orc_10
 tpcds_oss_text_10
 tpcds_text_5
 tst
(5 rows)

Query 20181115_030713_00002_kp5ih, FINISHED, 3 nodes
Splits: 36 total, 36 done (100.00%)
0:00 [20 rows, 331B] [41 rows/s, 694B/s]

## Wrong password
> presto  --server https://emr-header-1:7778  --keystore-path /etc/ecm/presto-conf/keystore --keystore-password 81ba14ce6084 --catalog hive --schema default --user sam --password
Password: <wrong password>
presto:default> show schemas;
Error running command: Authentication failed: Access Denied: Invalid credentials
```

