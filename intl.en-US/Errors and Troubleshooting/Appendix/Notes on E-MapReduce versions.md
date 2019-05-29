# Notes on E-MapReduce versions {#concept_vt4_gy1_pfb .concept}

-   EMR is regularly updated.
-   Versions of software installed within each EMR version are fixed. Currently, EMR does not support choosing different versions of software. We recommend that you do not manually change the versions of software. For example, Hadoop 3.6.0 and Spark 1.4.1 are installed in EMR V1.0.
-   If you have selected a version and created a cluster, the version that the cluster uses will not be automatically updated. If you select V1.0, the Hadoop remains at V2.6.0 and Spark remains at V1.4.1. When EMR is updated to V1.1, Hadoop is updated to V2.7.0 and Spark is updated to V1.5.0. These updates do no effect the clusters that you have created. Only new clusters use new mirror images.
-   When updating the cluster version, for example, from V1.0 to V1.1, test the jobs in the news software environment, to see if they run successfully and to avoid exceptions caused by incompatibility and a change of software environment.

