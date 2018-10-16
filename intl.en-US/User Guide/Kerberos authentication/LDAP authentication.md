# LDAP authentication {#concept_bbf_bg5_1fb .concept}

EMR cluster also supports authentication based on LDAP, which manages the account system through LDAP. Kerberos client uses LDAP account information as identity for authentication.

## LDAP Identity Authentication {#section_vth_by5_1fb .section}

LDAP account can be shared with other services, such as Hue. Users must only configure it on the Kerberos server. Users can use the LDAP service \(ApacheDS\) that has been configured in the EMR cluster or use the existing LDAP service. Users must only configure it on the Kerberos server.

Here’s an example of an LDAP service \(ApacheDS\) that has been started by default in a cluster:

-   Configure the basic environment in Gateway management \(the same as that in the second part of the RAM, which can be skipped if it has been configured\).

    The only difference is that auth\_type in /etc/has/has-client.conf needs to be modified to LDAP

    Or the user can also not modify /etc/has/has-client.conf. The user test can copy the file and modify auth\_type with their account and specify the path through environment variables, for example:

    `export HAS_CONF_DIR=/home/test/has-conf`

-   Configure LDAP manager user/password to Kerberos server end \(Has\) in the EMR console.

    Enter the EMR Console Cluster Configuration Management - HAS software, configure the LDAP manager user name and password to the corresponding bind\_dn and bind\_password fields and restart the HAS service.

    In this example, the LDAP service is the ApacheDS in the EMR cluster, and related fields can be obtained from ApacheDS.

-   EMR cluster manager adds user information to LDAP
    -   Obtain ApacheDS LDAP service and manager user and password manager\_dn and manager\_password can be seen in EMR Console Cluster Configuration Management/ApacheDS Configuration
    -   Add user test and password in ApacheDS

        ```
        Log on to root account in the cluster emr-header-1 node
         Create a file test.ldif with the following content:
         dn: cn=test,ou=people,o=emr
         objectclass: inetOrgPerson
         objectclass: organizationalPerson
         objectclass: person
         objectclass: top
         cn: test
         sn: test
         mail: test@example.com
         userpassword: test1234
         #Add to LDAP, in which -w denotes that password is changed to manager_password
         ldapmodify -x -h localhost -p 10389 -D "uid=admin,ou=system" -w "Ns1aSe" -a -f test.ldif
         #Delete test.ldif
         rm test.ldif
        ```

        Provide added user name/passowrd to user test.

-   User test configures LDAP information

    ```
    Log on the test account of Gateway
     # Run the script
     sh add_ldap.sh test
    ```

    Attachment: Script add\_ldap.sh \(modifying LDAP account information\)

    ```
    user=$1
     if [[ `cat /home/$user/.bashrc | grep 'export LDAP_'` == "" ]];then
     echo "
     #Modify to the user test's LDAP_USER/LDAP_PWD
     export LDAP_USER=YOUR_LDAP_USER
     export LDAP_PWD=YOUR_LDAP_USER
     " >>~/.bashrc
     else
        echo $user LDAP user info has been added to .bashrc
     fi
    ```

-   User test access to the cluster services

    Execute HDFS commands

    ```
    [test@iZbp1cyio18s5ymggr7yhrZ ~]$ hadoop fs -ls /
      17/11/19 13:33:33 INFO client.HasClient: The plugin type is: LDAP
      Found 4 items
      drwxr-x---   - has    hadoop          0 2017-11-18 21:12 /apps
      drwxrwxrwt   - hadoop hadoop          0 2017-11-19 13:33 /spark-history
      drwxrwxrwt   - hadoop hadoop          0 2017-11-19 12:41 /tmp
      drwxrwxrwt   - hadoop hadoop          0 2017-11-19 12:41 /user
    ```

    Run Hadoop/Spark job.


