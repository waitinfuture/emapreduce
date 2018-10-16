# OSS ACL {#concept_jrw_vnl_z2b .concept}

## Procedure {#section_nbm_14l_z2b .section}

E-MapReduce supports using RAM to isolate the data of different sub-accounts. The procedures are shown as follows:

1.  Log on to the [Alibaba Cloud RAM Management console](https://ram.console.aliyun.com/).
2.  Create the sub-account in RAM with the process in [How to create the sub-account in RAM](https://www.alibabacloud.com/help/zh/doc-detail/28637.html).
3.  Click **Authorization Policy Management** at the left of [Alibaba Cloud RAM Management console](https://ram.console.aliyun.com/) to enter the page of authorization policy management.
4.  Click **Customized Authorization Policy**.
5.  Click **New Authorization Policy** at the upper right of the page to enter the authorization policy creation page. Create the policy according to the prompted steps. You can create as many policies as the sets of authorization control you need.

    It is assumed that you need the following 2 sets of data control policies:

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

6.  Click **User Management** at the left of [Alibaba Cloud RAM Management console](https://ram.console.aliyun.com/?spm=5176.6660585.774526198.1.2yNQJH#/policy/list/system).
7.  Find out the sub-account item which the policy is given to and click the right **Management** button to enter the user management page.
8.  Click **User Authorization Policy** at the left of page.
9.  Click **Edit Authorization Policy** at the upper right to enter the authorization policy page.
10. Select and add authorization policy.
11. Click **Confirm** to complete the policy authorization of sub-account.
12. Click **User Details** at the left of user management page to enter the user details page of sub-account.
13. Click **Start Console Logon** in the Web console logon management bar to start up the authorization of sub-account logon console.

## Complete and use {#section_dcf_jpl_z2b .section}

After completing all preceding steps, use the corresponding sub-account to log on to E-MapReduce with following limits:

-   All buckets can be seen in the OSS selection interface for cluster, operation, and plan execution creations, but the authorized bucket can only be entered.

-   The content under authorized bucket can only be seen, rather than those under other buckets.

-   The authorized bucket can only be read and written. Otherwise, an error is reported.


