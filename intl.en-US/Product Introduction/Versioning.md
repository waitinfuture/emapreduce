# Versioning {#concept_dtk_nj3_y2b .concept}

-   E-MapReduce applies a version number rule in the a.b.c format:

    -   a indicates major changes to the version.
    -   b indicates moderate changes to some components in the version.
    -   c indicates bug fixes in the version and can be compatible with previous versions.
    For example, updates from 1.0.0 to 2.0.0 are major version changes. After a version upgrade, we recommend that you test to make sure all previous jobs can run normally. An update from 1.0.0 and 1.1.0 is a change generally conducted to upgrade a component version. We recommend that you perform a similar test to verify jobs run normally. An update from 1.0.0 and 1.0.1 is a c position change, and remains fully compatible with previous versions.

-   The software and software version bound on each E-MapReduce release version are fixed. E-MapReduce does not support selection from multiple different versions of software, and manual changes to the software version are not recommended.

-   If a released version of E-MapReduce is selected, and is then created on a cluster, the version used by the cluster is not upgraded automatically. The images corresponding to the subsequent version do not affect the currently cluster created after upgrade as only new clusters use the new images.

-   When you upgrade the version of a cluster \(for example, from 1.0.x to 1.1.x\), we recommend that you must test your jobs to make sure that they run normally in the new software environment.


For more information about version details of E-MapReduce, see [Release notes](intl.en-US/Product Introduction/Release notes.md#).

