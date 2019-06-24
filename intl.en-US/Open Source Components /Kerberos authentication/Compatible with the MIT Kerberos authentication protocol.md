# Compatible with the MIT Kerberos authentication protocol {#concept_qk1_5pl_z2b .concept}

This topic takes the HDFS service as an example to describe the authentication process of the Massachusetts Institute of Technology \(MIT\) Kerberos protocol.

## Identities authenticated by using the MIT Kerberos protocol {#section_an3_fql_z2b .section}

In an EMR cluster, the Kerberos service is running on the master node of the cluster. You must use the root account to perform administrative operations. You can only perform these administrative operations on the master node named emr-header-1.

The following example describes the authentication process of how to use a test user to access the HDFS service:

-   Run the `hadoop fs -ls /` command on a gateway.
    -   Download the krb5.conf file

        ``` {#codeblock_zyx_xmo_tsd}
        Log on to the gateway by using the root account.
         scp root@emr-header-1:/etc/krb5.conf /etc/
        ```

    -   Add a principal
        -   Log on to the emr-header-1 node and switch to the root account. The master node you log on to must be emr-header-1 because you cannot configure high availability \(HA\) on the emr-header-2 node.
        -   Start the administration tool of Kerberos.
        -   ``` {#codeblock_x3a_buk_vib}
  sh /usr/lib/has-current/bin/hadmin-local.sh /etc/ecm/has-conf -k /etc/ecm/has-conf/admin.keytab
  HadminLocalTool.local: #Press Enter to view a list of commands and the usage of each command.
  HadminLocalTool.local: addprinc #Press Enter to view the usage of the command.
  HadminLocalTool.local: addprinc -pw 123456 test #Add a principal named test and specify 123456 as the password.
```

    -   Export a keytab file

        Log on to the emr-header-1 node and import the keytab file. The node must be emr-header-1 because you cannot configure HA on the emr-header-2 node.

        You can use the Kerberos administration tool to export the keytab file of the principal.

        ``` {#codeblock_lct_ihg_zis}
        HadminLocalTool.local: ktadd -k /root/test.keytab test #Export the keytab file for later use.
        ```

    -   Use the kinit command to obtain a ticket

        Run HDFS commands on a client \(the gateway\).

        -   Create a Linux account named test

            ``` {#codeblock_kze_pel_jnd}
            useradd test
            ```

        -   Install MIT Kerberos clients.

            You can use MIT Kerberos clients run commands, such as kinit and klist. For more information, see [MIT Kerberos documentation](https://web.mit.edu/kerberos/krb5-1.12/doc/user/user_commands/kinit.html).

             `yum install krb5-libs krb5-workstation -y`

        -   Switch to the test account and run the kinit command.

            ``` {#codeblock_6xr_nsc_cna}
                  su test
                  #If the keytab file does not exist, run the following command:
                  kinit #Press Enter
                  Password for test: 123456 #Enter the password.
                  #If the keytab file exists, run the following command:
                  kinit -kt test.keytab test    
                  #View a ticket
                  klist
            ```

            **Note:** Example of MIT Kerberos commands

            ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17935/156135062911126_en-US.png)

    -   Run HDFS commands

        After obtaining a ticket, you can run HDFS commands.

        ``` {#codeblock_bkh_nb5_zdx}
        hadoop fs -ls /
              Found 5 items
             drwxr-xr-x     - hadoop hadoop          0 2017-11-12 14:23 /apps
             drwx------      - hbase  hadoop           0 2017-11-15 19:40 /hbase
             drwxrwx--t+   - hadoop hadoop         0 2017-11-15 17:51 /spark-history
             drwxrwxrwt   - hadoop hadoop          0 2017-11-13 23:25 /tmp
             drwxr-x--t      - hadoop hadoop          0 2017-11-13 16:12 /user
        ```

        **Note:** Before running a YARN job, you must add the corresponding Linux account for all nodes of a cluster. For more information, see [Add a test account to an EMR cluster](reseller.en-US/Open Source Components /Kerberos authentication/RAM authentication.md#ul_myx_vc5_1fb).

-   Access HDFS through a Java snippet
    -   Use a cached ticket

        **Note:** You must run the kinit command to obtain a ticket in advance. An error occurs if applications attempt to access an expired ticket.

        ``` {#codeblock_rjt_oa3_01e}
        public static void main(String[] args) throws IOException {
           Configuration conf = new Configuration();
           //Load the configurations of HDFS. You can retrieve a copy of configurations from the EMR cluster.
           conf.addResource(new Path("/etc/ecm/hadoop-conf/hdfs-site.xml"));
           conf.addResource(new Path("/etc/ecm/hadoop-conf/core-site.xml"));
           //Run the kinit command by using the Linux account to obtain a ticket before running the Java snippet.
           UserGroupInformation.setConfiguration(conf);
           UserGroupInformation.loginUserFromSubject(null);
           FileSystem fs = FileSystem.get(conf);
           FileStatus[] fsStatus = fs.listStatus(new Path("/"));
           for(int i = 0; i < fsStatus.length; i++){
               System.out.println(fsStatus[i].getPath().toString());
           }
        }
        ```

    -   \(recommended\) Use the keytab file

        **Note:** The keytab file is permanently valid. The validity of the key tab file is irrelevant to local tickets.

        ``` {#codeblock_dzz_nkr_thm}
        public static void main(String[] args) throws IOException {
          String keytab = args[0];
          String principal = args[1];
          Configuration conf = new Configuration();
          //Load the configurations of HDFS. You can retrieve a copy of configurations from the EMR cluster.
          conf.addResource(new Path("/etc/ecm/hadoop-conf/hdfs-site.xml"));
          conf.addResource(new Path("/etc/ecm/hadoop-conf/core-site.xml"));
          //Use the keytab file. You can retrieve the keytab file from the master-1 node by running the kinit command.
          UserGroupInformation.setConfiguration(conf);
          UserGroupInformation.loginUserFromKeytab(principal, keytab);
          FileSystem fs = FileSystem.get(conf);
          FileStatus[] fsStatus = fs.listStatus(new Path("/"));
          for(int i = 0; i < fsStatus.length; i++){
              System.out.println(fsStatus[i].getPath().toString());
          }
          }
        ```

        The following dependencies are specified in the pom.xml file.

        ``` {#codeblock_g4k_d1y_3p4}
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


