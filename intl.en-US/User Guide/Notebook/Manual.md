# Manual {#concept_d4n_mvj_z2b .concept}

This article shows you how to create a new notebook task on the EMR console and guide you through the task creation and operation.

## Create a new notebook task {#section_ebr_pvj_z2b .section}

**Note:** The cluster on which an interactive task is run must be EMR-2.3 or later, and has no less than three nodes, each with at least 4 cores and 8 GB of memory.

1.  Log on to the[Alibaba Cloud E-MapReduce console](https://emr.console.aliyun.com/).
2.  At the top of the navigation bar, click **Old EMR Scheduling**.
3.  In the left-side navigation bar, click **Notebook**.
4.  Click **New notebook** or **File** \> **-New notebook.**

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17928/154201531111017_en-US.jpg)

5.  Input the name and select the default type. The associated cluster is optional. Click **OK** to create a notebook.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17928/154201531111018_en-US.jpg)

    Three types are supported for a notebook task. Spark can be used to write scala spark codes. Spark SQL can be used to write SQL statements supported by Spark. Hive can be used to write SQL statements supported by Hive.

6.  An associated cluster must be a created cluster of EMR-2.3 or later, and has no less than three nodes, each with at least 4 cores and 8 GB of memory. You can also associate the cluster before running the task.

    Up to 20 interactive tasks can be created in one account.


## Enter and save a section {#section_j33_twj_z2b .section}

A paragraph is the smallest unit for running a notebook. For a notebook, you can fill in multiple paragraphs. Each paragraph can start with `%spark`，`%sql`，`%hive` indicating that this paragraph is a scala spark code paragraph, spark SQL paragraph, or Hive SQL paragraph. The type prefix is segregated by a blank space or by line feed and actual content. If the type prefix is not specified, the default type of the interactive task will be used as the run type of this paragraph.

The following is an example showing how to create a temporary Spark table:

Paste the following code in the section and a red \* symbol is displayed, indicating that this notebook has been changed. You can click the **Save Paragraph** button or **run**button to save the modifications to the paragraph. Click **+** below the paragraph to create a new paragraph. Up to 30 paragraphs can be created in one notebook.

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

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17928/154201531111030_en-US.jpg)

## Run a paragraph {#section_l1b_byj_z2b .section}

Before running a notebook, you must associate it to a created cluster. If a created notebook is not associated with a cluster, **Not associated** is displayed in the upper right corner of the page. You can click to select a cluster in the list of available clusters. Note that the associated cluster must be EMR-2.3 or later, and has no less than three nodes, each with at least 4 cores and 8 GB of memory.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17928/154201531111031_en-US.jpg)

Click the**Run**button to automatically save the current paragraph and run the content. If this paragraph is the last one, a new paragraph is automatically created.

**PENDING**means the paragraph has not run yet and **RUNNING**means the paragraph is running. **FINISHED**means the running process has finished. **ERROR**means an error has occurred. The running result is displayed below the run button of the paragraph. FINISHED means the running process has been finished. ERROR means an error occurs. The running result is displayed below the run button of the section. During running, you can click "Cancel" below the "Run" button to cancel running. **ABORT**is displayed after running is canceled.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17928/154201531111032_en-US.jpg)

The paragraph can be run multiple times and only the result of the last running is retained. You cannot modify the entered content of the paragraph during running. The content can be modified only after running of the paragraph is finished.

## Run all {#section_mwv_5yj_z2b .section}

For a notebook, you can click**Run All**on the menu bar to run all paragraphs. The paragraphs are submitted sequentially for running. Different types have independent execution queues. If a notebook contains multiple paragraph types, the order for executing the paragraphs on the cluster is based on type after these paragraphs are submitted sequentially. Spark and Spark SQL support one-by-one execution. Hive supports concurrent execution and the maximum number of concurrently executed interactive paragraphs on the same cluster is 10. Note that all concurrently executed paragraphs are restricted by cluster resources. If the cluster size is small and many paragraphs need to be executed concurrently, the paragraphs still need to queue on the Yarn.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17928/154201531111033_en-US.jpg)

## Cancel the association with clusters {#section_ixg_bpk_z2b .section}

After a notebook is run in a cluster, the cluster creates a process for catching of some context running environments to ensure quick response upon re-run. If you do not need to run other notebooks and you want to release the cluster resources occupied by caching, you can disassociate all notebooks that have been run from the associated clusters. In this way, you can release the memory resources occupied on the original associated clusters.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17928/154201531211037_en-US.jpg)

## Other operations {#section_ulb_hpk_z2b .section}

-   Paragraph operations

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17928/154201531211038_en-US.jpg)

    -   **Hide and display the results**

        You can hide the paragraph results and display the entered content of the paragraph only.

    -   **Delete a paragraph**

        Delete the current paragraph. The paragraph that is running can also be deleted.

-   File menu

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17928/154201531211040_en-US.jpg)

    -   **New notebook**

        Create a notebook and switch to the created notebook interface.

    -   **Create Paragraph**

        Add a new paragraph to the end of the notebook. A notebook can only have up to 30 paragraphs.

    -   **Save all paragraphs**

        Save all modified paragraphs.

    -   **Delete notebook**

        Delete the current notebook. If the cluster has been associated, it will be disassociated.

-   **View**

    Display codes only or display codes and results

    Only the entered codes for all paragraphs are displayed or both the codes and results are displayed.


