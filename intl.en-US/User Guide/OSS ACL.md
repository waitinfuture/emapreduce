# OSS ACL {#concept_jrw_vnl_z2b .concept}

## Procedure {#section_nbm_14l_z2b .section}

E-MapReduce supports using RAM to isolate the data of different sub-accounts. The procedures are shown as follows:

1.  Log on to the [Alibaba Cloud RAM Management console](https://ram.console.aliyun.com/).
2.  Create the sub-account in RAM with the process in [How to create the sub-account in RAM](../../../../intl.en-US/Quick Start/Create a RAM user.md#).
3.  In the left-side navigation pane, click **policies** to enter the page of policy management.
4.  Click **Custom Policy**.
5.  In the upper-right corner, click **Create Authorization Policy** enter the authorization policy creation page. Create the policy according to the prompted steps. You can create as many policies as the sets of authorization control you need.

    It is assumed that you need the following two sets of data control policies:

    -   Testing environment, bucketname: test-bucket. The corresponding complete policy is as follows.

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

    -   Production environment, bucketname: prod-bucket. The corresponding complete policy is as follows:

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
7.  Find out the sub-account item which the policy is given to and click the **Manage** button.
8.  In the left-side navigation pane, click **User Authorization Policy**.
9.  In the upper-right corner, click **Edit Authorization Policy**.
10. Select and add authorization policies.
11. Click **OK** to complete the policy authorization of the sub-account.
12. In the upper-right corner, click **User Details** to enter the user details page of the sub-account.
13. Click **Start Console Logon** in the Web console logon management bar to start up the authorization of the sub-account logon console.

## Complete and use {#section_dcf_jpl_z2b .section}

After completing all preceding steps, use the corresponding sub-account to log on to E-MapReduce with following limits:

-   All buckets can be seen in the OSS selection interface for cluster, operation, and plan execution creations, but the authorized bucket can only be entered.

-   The content under authorized bucket can only be seen, rather than those under other buckets.

-   The authorized bucket can only be read and written. Otherwise, an error is reported.


