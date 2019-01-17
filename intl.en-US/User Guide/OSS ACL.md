# OSS ACL {#concept_jrw_vnl_z2b .concept}

This article introduces how to use RAM Access control to isolate the data of different sub-account.

## Procedure {#section_nbm_14l_z2b .section}

E-MapReduce supports using RAM to isolate the data of different sub-accounts. To do so, complete the following steps:

1.  Log on to the [Alibaba Cloud RAM console](https://partners-intl.aliyun.com/login-required#/ram).
2.  Create a sub-account in RAM. For more information, see [Create a RAM user](../../../../../reseller.en-US/Quick Start/Create a RAM user.md#).
3.  In the navigation pane on the left, click **Policies** to enter the policy management page.
4.  Click **Custom Policy**.
5.  In the upper-right corner, click **Create Authorization Policy**. Follow the steps there to create a policy. You can create as many policies as the number of authorization control sets that you need.

    In this example, it is assumed that you need the following two sets of data control policies:

    -   Bucketname of the testing environment: test-bucket. The corresponding policy is as follows:

```
{
"Version": "1",
"Statement": [
{
"Effect": "Allow",
"Action": [
  "oss:ListBuckets"
],
"Resource": [
  "acs:oss:*:*:*"
]
},
{
"Effect": "Allow",
"Action": [
  "oss:Listobjects",
  "oss:GetObject",
  "oss:PutObject",
  "oss:DeleteObject"
],
"Resource": [
  "acs:oss:*:*:test-bucket",
  "acs:oss:*:*:test-bucket/*"
]
}
]
}
```

    -   Bucketname of the production environment: prod-bucket. The corresponding policy is as follows:

        ```
        {
        "Version": "1",
        "Statement": [
        {
        "Effect": "Allow",
        "Action": [
          "oss:ListBuckets"
        ],
        "Resource": [
          "acs:oss:*:*:*"
        ]
        },
        {
        "Effect": "Allow",
        "Action": [
          "oss:Listobjects",
          "oss:GetObject",
          "oss:PutObject"
        ],
        "Resource": [
          "acs:oss:*:*:prod-bucket",
          "acs:oss:*:*:prod-bucket/*"
        ]
        }
        ]
        }
        ```

6.  Click **Users**.
7.  Find the sub-account item which the policy is given to and click **Manage**.
8.  In the navigation pane on the left, click **User Authorization Policy**.
9.  In the upper-right corner, click **Edit Authorization Policy**.
10. Select and add authorization policies.
11. Click **OK** to complete the policy authorization of the sub-account.
12. In the upper-right corner, click **User Details** to enter the user details page of the sub-account.
13. Click **Start Console Logon** in the console logon management bar to initialize the authorization of the sub-account logon console.

## Complete and use {#section_dcf_jpl_z2b .section}

Once you have completed all of the preceding steps, you can use the sub-account to log on to E-MapReduce. Note the following limitations:

-   All buckets are displayed in the OSS interface for clusters, operations, and plan executions. However, you can only enter the authorized bucket.

-   Only the content under the authorized bucket is displayed.

-   The authorized bucket can only be read and written. Otherwise, an error is reported.


