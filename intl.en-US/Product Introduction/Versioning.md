# Versioning {#concept_dtk_nj3_y2b .concept}

-   E-MapReduce follows an X.Y.Z format in its software version numbers:

    -   X indicates that major changes have been made in the version.
    -   Y indicates that minor changes have been made to some components in the version.
    -   Z indicates that while bugs have been fixed in the version, it is still compatible with previous versions.
    If you upgrade from version 1.0.0 to version 2.0.0, for example, a number of major changes will have been made. We therefore recommend verifying that all previous jobs can run normally. An update from version 1.0.0 to 1.1.0, however, is typically made to upgrade a component. We recommend performing a similar test to verify that jobs are running normally. An update from version 1.0.0 to version 1.0.1 is typically to perform some kind of maintenance, and it remains fully compatible with previous versions.

-   In each release of E-MapReduce, the software and software version are fixed. You cannot select multiple software versions, and changing the software version manually is not recommended.

-   If you select a version of E-MapReduce and use it in a cluster, it is not upgraded automatically. If the images corresponding to this version are upgraded, the cluster you created is not impacted. Only new clusters will use the new images.

-   When you upgrade the version of your cluster \(for example, from 1.0.x to 1.1.x\), we recommend performing a test to verify that your jobs are running normally in the new software environment.


For more information about the different E-MapReduce versions, see [Release notes](intl.en-US/Product Introduction/Release notes.md#).

