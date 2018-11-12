# Cross-region access {#concept_l5t_bpd_z2b .concept}

Kerberos in E-MapReduce supports the cross-region feature, that is, different Kerberos clusters can inter-access each other.

This article describe cross-region access by using Cluster-A and Cluster-B as an example.

-   hostname of emr-header-1 in Cluster-A → emr-header-1.cluster-1234 ; region → EMR.1234.COM
-   hostname of emr-header-1 in Cluster-B → emr-header-1.cluster-6789 ; region → EMR.6789.COM
-   **Note:** 

-   The hostname can be obtained through executing the command hostname on emr-header-1.
-   Region can be obtained in /etc/krb5.conf on emr-header-1.

## Add principal {#section_r3x_2nv_1fb .section}

emr-header-1 nodes in Cluster-A and Cluster-B run the same command exactly as follows:

```
# root account
       sh /usr/lib/has-current/bin/hadmin-local.sh /etc/ecm/has-conf -k /etc/ecm/has-conf/admin.keytab
       HadminLocalTool.local: addprinc -pw 123456 krbtgt/EMR.6789.COM@EMR.1234.COM 6789. COM@EMR. 1234. Com
```

**Note:** 

-   123456 is the password, which can be changed.
-   EMR.6789.COM is the region of Cluster-B, namely, the region of the cluster to be accessed.
-   EMR.1234.COM is the region of Cluster-A, namely, the region of the cluster that initiates the access.

## Configure /etc/krb5.conf for Cluster-A {#section_u4x_nnv_1fb .section}

Configure \[regions\]/\[domain\_region\]/\[capaths\] on Cluster-A as follows:

```
[libdefaults]
    kdc_realm = EMR. 1234. COM
    default_realm = EMR. 1234. COM
    udp_preference_limit = 4096
    kdc_tcp_port = 88
    kdc_udp_port = 88
    dns_lookup_kdc = false
[realms]
    EMR. 1234. COM = {
                kdc = 10.81.49.3:88
    }
    EMR. 6789. COM = {
                kdc = 10.81.49.7:88
    }
[domain_realm]
    .cluster-1234 = EMR. 1234. COM
    .cluster-6789 = EMR. 6789. COM
[capaths]
    EMR. 1234. COM = {
       EMR. 6789. COM = .
    }
    EMR. 6789. COM = {
       EMR. 1234. COM = .
    }
```

Synchronize /etc/krb5.conf to all Cluster-A nodes.

Copy the binding information \(only the long domain name emr-xxx-x.cluster-xxx is needed\) in the file /etc/hosts in Cluster-B to /etc/hosts for all Cluster-A nodes.

```
10.81.45.89  emr-worker-1.cluster-xxx
 10.81.46.222  emr-worker-2.cluster-xx
 10.81.44.177  emr-header-1.cluster-xxx
```

**Note:** 

-   If a job is running on Cluster-A to access Cluster-B. yarn must be restarted.
-   Configure host binding information for all Cluster-A nodes.

## Access services in Cluster-B {#section_dwx_wnv_1fb .section}

The keytab file /ticket in Kerberos of Cluster-A can be used on Cluster-A as a cache to access services in Cluster-B. Matters:

For example, access hdfs service in Cluster-B:

```
su has;
hadoop fs -ls hdfs://emr-header-1.cluster-6789:9000/
Found 4 items
-rw-r-----   2 has    hadoop         34 2017-12-05 18:15 hdfs://emr-header-1.cluster-6789:9000/abc
drwxrwxrwt   - hadoop hadoop          0 2017-12-05 18:32 hdfs://emr-header-1.cluster-6789:9000/spark-history
drwxrwxrwt   - hadoop hadoop          0 2017-12-05 17:53 hdfs://emr-header-1.cluster-6789:9000/tmp
drwxrwxrwt   - hadoop hadoop          0 2017-12-05 18:24 hdfs://emr-header-1.cluster-6789:9000/user
```

