# What is Presto {#concept_bxd_422_z2b .concept}

Presto is a distributed SQL-on-Hadoop analytics engine powered by FaceBook. Presto is currently maintained by the open source community and FaceBook engineers, and has derived multiple commercial versions.

## Basic features {#section_ppy_p22_z2b .section}

Presto is implemented in Java. It is easy-to-use, offers high-performance, strong expandability and other features include:

-   fully supports ANSI SQL
-   supports rich data sources. Presto can access rich data sources as follows:

-   interaction with Hive data warehouse
-   Cassandra
-   Kafka
-   MongoDB
-   MySQL
-   PostgreSQL
-   SQL Server
-   Redis
-   Redshift
-   Local files
-   supports advanced data structures.

-   array and Map data
-   JSON data
-   GIS data
-   color data
-   Strong expandability. Presto provides multiple expansion configurations.

-   Data connector expansion
-   Custom data types
-   Custom SQL functions
    Users can expand the corresponding modules according to their own service features to achieve efficient service processes.

-   Based on the Pipeline process model, data is returned to users in real time during the process.

-   Improved monitoring interfaces.

-   Friendly WebUI is provided to present the execution processes of the query tasks visually.
-   Supports JMX protocol.

## Scenarios {#section_wpy_p22_z2b .section}

Presto is a distributed SQL engine located in data warehouse and data analytics services and is well suited to the following scenarios:

-   ETL
-   Ad-Hoc query
-   Massive structured and semi-structured data analysis
-   Massive multi-dimensional data aggregation/reports

In particular, Presto is a data warehouse product, which is not designed to replace traditional RDBMS databases such as MySQL and PostgreSQL. It has limited support for transactions and is not suitable for online service scenarios.

## Benefits {#section_ypy_p22_z2b .section}

In addition to the advantages of open source, the EMR Presto product comes with the following advantages:

-   You can purchase it for immediate use to build a Presto cluster with hundreds of nodes in minutes.
-   It supports elastic resizing, you can complete up and down resizing of the cluster with simple operations.
-   It works perfect in connection with the EMR software stacks, and supports processing of data stored in OSS.
-   O&M 24x7 free all-in-one service.

