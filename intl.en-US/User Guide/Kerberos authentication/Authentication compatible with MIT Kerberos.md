# Authentication compatible with MIT Kerberos {#concept_qk1_5pl_z2b .concept}

This section details the MIT Kerberos authentication process through the HDFS service.

## Authentication method {#section_an3_fql_z2b .section}

The Kerberos server in the E-MapReduce cluster is started at the master node. Some management operations must be performed with the root account of the master node \(emr-header-1\).

In the following example, the test user accesses the HDFS service to introduce relevant procedures.

-   Execute `hadoop fs -ls /` on the gateway.
    -   Configure krb5.conf.

        ```
        Use root account on the Gateway
         scp root@emr-header-1:/etc/krb5.conf /etc/
        ```

    -   Add principal.
        -   Log on to the emr-header-1 node and switch to the root account.
        -   Open the admin tool in Kerberos.
        -   ```
  sh /usr/lib/has-current/bin/hadmin-local.sh /etc/ecm/has-conf -k /etc/ecm/has-conf/admin.keytab
  HadminLocalTool.local: #Press Enter to view the use of the commands
  HadminLocalTool.local: addprinc #Input the command and press Enter to view the use of the specific command
  HadminLocalTool.local: addprinc -pw 123456 test #Add principal for the user test, and set the password to 123456
```

    -   Export the keytab file.

        Use the Kerberos admin tool to export the keytab file that corresponds to the principal.

        ```
        HadminLocalTool.local: ktadd -k /root/test.keytab test #Export the keytab file, which can be used subsequently
        ```

    -   Use kinit to obtain the ticket.

        On the client machine where HDFS commands are executed, such as the gateway:

        -   Add the Linux account test `useradd test`.
        -   Install MIT Kerberos client tools.

            MIT Kerberos tools can be used for related operations \(such as kinit and klist\). For more information, see [MIT Kerberos](https://web.mit.edu/kerberos/krb5-1.12/doc/user/user_commands/kinit.html).

            `yum install krb5-libs krb5-workstation -y`

        -   Switch to the test account to execute kinit.

            ```
                  su test
                  #If the keytab file does not exist, execute
                  kinit #Press Enter
                  Password for test: 123456 #Done
                  #the keytab file exists, execute
                  kinit -kt test.keytab test    
                  #View the ticket
                  klist
            ```

            **Note:** Application of MIT Kerberos tools

            ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17935/155255226311126_en-US.png)

    -   Execute HDFS commands.

        When a ticket is obtained, HDFS commands can be executed as usual.

        ```
        hadoop fs -ls /
              Found 5 items
             drwxr-xr-x     - hadoop hadoop          0 2017-11-12 14:23 /apps
             drwx------      - hbase  hadoop           0 2017-11-15 19:40 /hbase
             drwxrwx--t+   - hadoop hadoop         0 2017-11-15 17:51 /spark-history
             drwxrwxrwt   - hadoop hadoop          0 2017-11-13 23:25 /tmp
             drwxr-x--t      - hadoop hadoop          0 2017-11-13 16:12 /user
        ```

        **Note:** To run a YARN job, you need to add the corresponding Linux accounts to all of the nodes in the cluster in advance. For more information, see [Add test account to the E-MapReduce cluster](reseller.en-US/User Guide/Kerberos authentication/RAM authentication.md#).

-   Use Java to access HDFS.
    -   Use a local ticket cache.

        **Note:** To obtain the ticket, you need to execute kinit in advance. When the ticket expires, the application is not accessed.

        ```
        public static void main(String[] args) throws IOException {
           Configuration conf = new Configuration();
           //Load the HDFS configuration, which is copied from the EMR cluster
           conf.addResource(new Path("/etc/ecm/hadoop-conf/hdfs-site.xml"));
           conf.addResource(new Path("/etc/ecm/hadoop-conf/core-site.xml"));
           //kinit needs to be executed in advance to obtain the ticket with the Linux account of the application
           UserGroupInformation.setConfiguration(conf);
           UserGroupInformation.loginUserFromSubject(null);
           FileSystem fs = FileSystem.get(conf);
           FileStatus[] fsStatus = fs.listStatus(new Path("/"));
           for(int i = 0; i < fsStatus.length; i++){
               System.out.println(fsStatus[i].getPath().toString());
           }
        }
        ```

    -   \(Recommended\) Use a keytab file.

        **Note:** The keytab is valid for a long time and has nothing to do with the local ticket.

        ```
        public static void main(String[] args) throws IOException {
          String keytab = args[0];
          String principal = args[1];
          Configuration conf = new Configuration();
          //Load the HDFS configuration, which is copied from the EMR cluster
          conf.addResource(new Path("/etc/ecm/hadoop-conf/hdfs-site.xml"));
          conf.addResource(new Path("/etc/ecm/hadoop-conf/core-site.xml"));
          //Directly use keytab file, which is obtained through executing relevant commands on master-1 in the EMR cluster [the commands are introduced earlier in this article]
          UserGroupInformation.setConfiguration(conf);
          UserGroupInformation.loginUserFromKeytab(principal, keytab);
          FileSystem fs = FileSystem.get(conf);
          FileStatus[] fsStatus = fs.listStatus(new Path("/"));
          for(int i = 0; i < fsStatus.length; i++){
              System.out.println(fsStatus[i].getPath().toString());
          }
          }
        ```

        POM dependencies are attached:

        ```
        <dependencies>
                    <dependency>
                        <groupId>org.apache.hadoop</groupId>
                        <artifactId>hadoop-common</artifactId>
                        <version>2.7.2</version>
                    </dependency>
                    <dependency>
                        <groupId>org.apache.hadoop</groupId>
                        <artifactId>hadoop-hdfs</artifactId>
                        <version>2.7.2</version>
                    </dependency>
                </dependencies>
        ```


