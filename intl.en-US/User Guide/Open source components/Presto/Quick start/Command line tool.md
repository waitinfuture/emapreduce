# Command line tool {#concept_kfx_nyt_xgb .concept}

This section describes how to use the command line tool to operate the Presto console.

The command line tool uses [SSH to log on to an EMR cluster](reseller.en-US/Quick Start/Connect to clusters using SSH.md#) and executes the following command to enter the Presto console:

```
$ presto --server emr-header-1:9090 --catalog hive --schema default --user hadoop
```

High-security clusters use the following command:

```
$ presto  --server https://emr-header-1:7778  \
          --enable-authentication \
          --krb5-config-path /etc/krb5.conf \
          --krb5-keytab-path  /etc/ecm/presto-conf/presto.keytab \
          --krb5-remote-service-name presto \
          --keystore-path /etc/ecm/presto-conf/keystore \
          --keystore-password 81ba14ce6084 \
          --catalog hive --schema default \
          --krb5-principal  presto/emr-header-1.cluster-XXXX@EMR.XXXX.COM
```

-   XXXX is the ECM ID of the cluster, a string of numbers that can be obtained through cat /etc/hosts .
-   81ba14ce6084 is the default password of /etc/ecm/presto-conf/keystore . We recommend that you use your own keystore after deployment.

You can execute the following command from the console:

```
Presto: Default> show schemas;
    schema.
--------------------
default
Hive
information_schema
tpch_100gb_orc
tpch_10gb_orc
tpch_10tb_orc
tpch_1tb_orc
(7 rows)
```

You can then execute the presto --help command to obtain help from the console. The parameters and definitions are as follows:

```
--server <server>                       # Specifies the URI of a Coordinator
--user <user>                           # Sets the username
--catalog <catalog>                     # Specifies the default Catalog
--schema <schema>                       # Specifies the default Schema
--execute <execute>                     # Executes a statement and then exits
-f <file>, --file <file>                # Executes an SQL statement and then exits
--debug                                 # Shows debugging information
--client-request-timeout <timeout>      # Specifies the client timeout value, which is 2 minutes by default
--enable-authentication                 # Enables client authentication
--keystore-password <keystore password> # KeyStore password
--keystore-path <keystore path>         # KeyStore path
--krb5-config-path <krb5 config path>   # Kerberos configuration file path (default: /etc/krb5.conf)
--krb5-credential-cache-path <path>     # Kerberos credential cache path
--krb5-keytab-path <krb5 keytab path>   # Kerberos Key table path
--krb5-principal <krb5 principal>       # Kerberos principal to be used
--krb5-remote-service-name <name>       # Remote Kerberos node name
--log-levels-file <log levels>          # Configuration file path for debugging logs
--output-format <output-format>         # Bulk export data format, which is CSV by default
--session <session>                     # Specifies the session attribute, in the format key=value
--socks-proxy <socks-proxy>             # Sets the proxy server
--source <source>                       # Sets query source
--version                               # Shows version info
-h, --help                              # Shows help info
```

