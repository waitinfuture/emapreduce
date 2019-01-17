# Operations {#concept_d4n_mvj_z2b .concept}

This section details how to perform a number of notebook operations, including how to create a new notebook task on the E-MapReduce console.

## Create a new notebook task {#section_ebr_pvj_z2b .section}

**Note:** The cluster on which an interactive task is run must be E-MapReduce 2.3 or later and have no less than three nodes, each with at least 4 cores and 8 GB of memory.

1.  Log on to the [Alibaba Cloud E-MapReduce console](https://partners-intl.console.aliyun.com/#/emr).
2.  At the top of the navigation bar, click **Old EMR Scheduling**.
3.  In the navigation bar on the left, click **Notebook**.
4.  Click **New notebook** or **File** \> **-New notebook.**

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17928/154771058311017_en-US.jpg)

5.  Enter a name and select the default type. Associating a cluster is optional. Click **OK** to create a notebook.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17928/154771058311018_en-US.jpg)

    Three types of notebook task are supported. Spark can be used to write Scala code, Spark SQL can be used to write SQL statements supported by Spark, and Hive can be used to write SQL statements supported by Hive.

6.  An associated cluster must be E-MapReduce 2.3 or later and have no less than three nodes, each with at least 4 cores and 8 GB of memory. You can also associate the cluster before running the task.

    Up to 20 interactive tasks can be created in one account.


## Enter and save a section {#section_j33_twj_z2b .section}

A paragraph is the smallest unit for running a notebook. Multiple paragraphs can be entered into a notebook. Each paragraph starts with either `%spark`, `%sql`, or `%hive`, indicating whether it is a Scala code paragraph, Spark SQL paragraph, or Hive SQL paragraph. The type prefix is separated by a blank space or by line feed and actual content. If the type prefix is not specified, the default type of the interactive task is used as the run type of this paragraph.

The following example shows how to create a temporary Spark table:

Paste the following code into the section and a red \* symbol is displayed, indicating that this notebook has been changed. Click **Save Paragraph** or **run** to save the modifications to the paragraph. Click **+** under the paragraph to create a new paragraph. Up to 30 paragraphs can be created in one notebook.

```
%spark
import org.apache.commons.io.IOUtils
import java.net.URL
import java.nio.charset.Charset
// load bank data
val bankText = sc.parallelize(
    IOUtils.toString(
        new URL("http://emr-sample-projects.oss-cn-hangzhou.aliyuncs.com/bank.csv"),
        Charset.forName("utf8")).split("\n"))
case class Bank(age: Integer, job: String, marital: String, education: String, balance: Integer)
val bank = bankText.map(s => s.split(";")).filter(s => s(0) ! = "\"age\"").map(
    s => Bank(s(0).toInt, 
            s(1).replaceAll("\"", ""),
            s(2).replaceAll("\"", ""),
            s(3).replaceAll("\"", ""),
            s(5).replaceAll("\"", "").toInt
        )
).toDF()
bank.registerTempTable("bank")
```

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17928/154771058311030_en-US.jpg)

## Run a paragraph {#section_l1b_byj_z2b .section}

Before running a notebook, you must first associate it to a created cluster. If a created notebook is not associated with a cluster, **Not Attached** is displayed in the upper-right corner of the page. Click it to select a cluster from the list of available clusters. Note that the associated cluster must be E-MapReduce 2.3 or later and have no less than three nodes, each with at least 4 cores and 8 GB of memory.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17928/154771058311031_en-US.jpg)

Click **Run** to save the current paragraph and run the content. If this is the last paragraph, a new paragraph is created automatically.

**PENDING** indicates that the paragraph has not run yet, **RUNNING** indicates that the paragraph is running, **FINISHED** indicates that the running has finished, and **ERROR** indicates that an error has occurred. The running result is displayed beneath the Run button. During running, you can click Cancel beneath the Run button to cancel running. **ABORT** is displayed after running has been canceled.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17928/154771058311032_en-US.jpg)

The paragraph can be run multiple times, but only the result of the last running is retained. You cannot modify the content of a paragraph while it is running. It can only be modified after the running has finished.

## Run all {#section_mwv_5yj_z2b .section}

For a notebook, you can click **Run All** on the menu bar to run all paragraphs. The paragraphs are then submitted sequentially for running. Different types have independent execution queues. If a notebook contains multiple paragraph types, the order for executing them on the cluster is decided based on type after they have been submitted sequentially. Spark and Spark SQL support one-by-one execution. Hive supports concurrent execution, with the maximum number of concurrently executed interactive paragraphs on the same cluster is 10. Note that all concurrently executed paragraphs are restricted by cluster resources. If the cluster size is small and many paragraphs need to be executed concurrently, the paragraphs still need to queue in YARN.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17928/154771058311033_en-US.jpg)

## Cancel the association with clusters {#section_ixg_bpk_z2b .section}

After a notebook is run in a cluster, the cluster creates a process for caching some context running environments to ensure a quick response upon re-execution. If you do not need to run other notebooks, and you want to release the cluster resources occupied by caching, you can disassociate all notebooks that have been run from the associated clusters. In this way, you can release the memory resources occupied on the original associated clusters.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17928/154771058311037_en-US.jpg)

## Other operations {#section_ulb_hpk_z2b .section}

-   Paragraph operations

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17928/154771058311038_en-US.jpg)

    -   **Hide and display the results**

        Hide the paragraph results and only display the entered content of the paragraph.

    -   **Delete a paragraph**

        Delete the current paragraph. Paragraphs that are running can also be deleted.

-   File menu

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17928/154771058311040_en-US.jpg)

    -   **New notebook**

        Create a notebook and switch to the created notebook interface.

    -   **Create Paragraph**

        Add a new paragraph to the end of a notebook. A notebook can have up to 30 paragraphs.

    -   **Save all paragraphs**

        Save all modified paragraphs.

    -   **Delete notebook**

        Delete the current notebook. If a cluster has been associated, it will be disassociated.

-   **View**

    Only display codes or display codes and results.


