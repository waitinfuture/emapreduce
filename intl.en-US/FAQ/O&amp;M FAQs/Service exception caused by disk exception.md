# Service exception caused by disk exception {#concept_wmq_jzt_xfb .concept}

A disk exception can occur when the disk is full or when the disk is corrupted.

The following details describe how to resolve these issues:

## The disk is full {#section_up1_tzt_xfb .section}

1.  Log on to the corresponding machine, locate the full disk, and delete any unnecessary data to free up some of the disk space. Before you delete any data, note the following:
    -   Do NOT delete Kafka data directories. Otherwise, you will lose all of your data.
    -   We recommend that you review the oldest log data in your selected partitions \(that is, the oldest segments and corresponding index files\) and delete data you no longer require.
    -   We recommend you do not clean up Kafka topics, such as **consumer\_offsets** or schema.
2.  Restart the Kafka broker service.

## The disk is corrupted {#section_d35_p15_xfb .section}

If more than 25% of the disk is corrupted, the machine migration mode can be used for operation and maintenance. To access the machine maintenance mode, submit a ticket to Alibaba Cloud technical support.

