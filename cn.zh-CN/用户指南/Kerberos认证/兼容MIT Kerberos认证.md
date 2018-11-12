# 兼容MIT Kerberos认证 {#concept_qk1_5pl_z2b .concept}

本文将通过HDFS服务介绍兼容MIT Kerberos认证流程。

## 兼容MIT Kerberos的身份认证方式 {#section_an3_fql_z2b .section}

EMR集群中Kerberos服务端启动在master节点，涉及一些管理操作需在master节点\(emr-header-1\)的root账号执行。

下面以test用户访问HDFS服务为例介绍相关流程。

-   Gateway上执行`hadoop fs -ls /`
    -   配置krb5.conf

        ```
        Gateway上面使用root账号
         scp root@emr-header-1:/etc/krb5.conf /etc/
        ```

    -   添加principal
        -   登录集群emr-header-1节点，切到root账号。
        -   进入Kerberos的admin工具。
        -   ```
  sh /usr/lib/has-current/bin/hadmin-local.sh /etc/ecm/has-conf -k /etc/ecm/has-conf/admin.keytab
  HadminLocalTool.local: #直接按回车可以看到一些命令的用法
  HadminLocalTool.local: addprinc #输入命令按回车可以看到具体命令的用法
  HadminLocalTool.local: addprinc -pw 123456 test #添加test的princippal,密码设置为123456
```

    -   导出keytab文件

        使用Kerberos的admin工具可以导出principal对应的keytab文件。

        ```
        HadminLocalTool.local: ktadd -k /root/test.keytab test #导出keytab文件,后续可使用该文件
        ```

    -   kinit获取Ticket

        在执行hdfs命令的客户端机器上面，如Gateway。

        -   添加linux账号test

            ```
            useradd test
            ```

        -   安装MITKerberos 客户端工具。

            可以使用MITKerberos tools进行相关操作\(如kinit/klist等\)，详细使用方式参考MITKerberos文档

            `yum install krb5-libs krb5-workstation -y`

        -   切到test账号执行kinit

            ```
                  su test
                  #如果没有keytab文件,则执行
                  kinit #直接回车
                  Password for test: 123456 #即可
                  #如有有keytab文件,也可执行
                  kinit -kt test.keytab test    
                  #查看ticket
                  klist
            ```

            **说明：** MITKerberos工具使用实例

            ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17935/154201092611126_zh-CN.png)

    -   执行hdfs命令

        获取到Ticket后，就可以正常执行hdfs命令了。

        ```
        hadoop fs -ls /
              Found 5 items
             drwxr-xr-x     - hadoop hadoop          0 2017-11-12 14:23 /apps
             drwx------      - hbase  hadoop           0 2017-11-15 19:40 /hbase
             drwxrwx--t+   - hadoop hadoop         0 2017-11-15 17:51 /spark-history
             drwxrwxrwt   - hadoop hadoop          0 2017-11-13 23:25 /tmp
             drwxr-x--t      - hadoop hadoop          0 2017-11-13 16:12 /user
        ```

        **说明：** 跑yarn作业，需要提前在集群中所有节点添加对应的linux账号\(详见下文中\[EMR集群添加test账号\]。

-   java代码访问HDFS
    -   使用本地ticket cache

        **说明：** 需要提前执行kinit获取ticket,且ticket过期后程序会访问异常。

        ```
        public static void main(String[] args) throws IOException {
           Configuration conf = new Configuration();
           //加载hdfs的配置,配置从emr集群上复制一份
           conf.addResource(new Path("/etc/ecm/hadoop-conf/hdfs-site.xml"));
           conf.addResource(new Path("/etc/ecm/hadoop-conf/core-site.xml"));
           //需要在程序所在linux账号下,提前kinit获取ticket
           UserGroupInformation.setConfiguration(conf);
           UserGroupInformation.loginUserFromSubject(null);
           FileSystem fs = FileSystem.get(conf);
           FileStatus[] fsStatus = fs.listStatus(new Path("/"));
           for(int i = 0; i < fsStatus.length; i++){
               System.out.println(fsStatus[i].getPath().toString());
           }
        }
        ```

    -   使用keytab文件\(推荐\)

        **说明：** keytab长期有效，跟本地ticket无关。

        ```
        public static void main(String[] args) throws IOException {
          String keytab = args[0];
          String principal = args[1];
          Configuration conf = new Configuration();
          //加载hdfs的配置,配置从emr集群上复制一份
          conf.addResource(new Path("/etc/ecm/hadoop-conf/hdfs-site.xml"));
          conf.addResource(new Path("/etc/ecm/hadoop-conf/core-site.xml"));
          //直接使用keytab文件,该文件从emr集群master-1上面执行相关命令获取[文档前面有介绍命令]
          UserGroupInformation.setConfiguration(conf);
          UserGroupInformation.loginUserFromKeytab(principal, keytab);
          FileSystem fs = FileSystem.get(conf);
          FileStatus[] fsStatus = fs.listStatus(new Path("/"));
          for(int i = 0; i < fsStatus.length; i++){
              System.out.println(fsStatus[i].getPath().toString());
          }
          }
        ```

        附pom依赖:

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


