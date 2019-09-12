# Overview {#concept_1062611 .concept}

This topic describes the window functions, window types, and time attribute that Spark Streaming SQL supports.

## Window function {#section_b7w_ixf_ml5 .section}

Window functions support aggregation over a specific window. For example, to calculate the number of users who visited a certain webpage in the past minute, you can define a window to collect data in the past minute, and calculate data in this window. Spark Streaming SQL supports the following two types of windows:

-   Tumbling window
-   Sliding window

## Time attribute {#section_tnt_x80_7al .section}

Spark SQL supports aggregation of data in a window by using the event time attribute.

Generally, the event time attribute indicates the time when a data record was created. It is provided in the schema.

## Note {#section_yv7_6ay_0jx .section}

For queries with time windows, a window column is auto-generated, which contains the window start and end time specified by window.start and window.end.

