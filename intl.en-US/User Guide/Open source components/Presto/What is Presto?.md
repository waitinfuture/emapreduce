# What is Presto? {#concept_bxd_422_z2b .concept}

Presto is an open-source distributed SQL-on-Hadoop query engine powered by Facebook. It is currently maintained by the open source community and Facebook engineers, and has derived multiple commercial versions.

## Basic features {#section_ppy_p22_z2b .section}

Presto is implemented in Java. It is easy to use and offers high performance and strong scalability. Its other features are as follows:

-   Fully supports ANSI SQL.
-   Supports rich data sources, accessing them as follows:

-   Interaction with Hive
-   Cassandra
-   Kafka
-   MongoDB
-   MySQL
-   PostgreSQL
-   SQL Server
-   Redis
-   Redshift
-   Local files
-   Supports advanced data structures.

-   Array and map data
-   JSON data
-   GIS data
-   Color data
-   Presto provides the following expansion configurations:

-   Data connector expansion
-   Custom data types
-   Custom SQL functions
    To achieve efficient service processes, you can expand the corresponding modules according to your own service features.

-   Based on the Pipeline process model, data is returned to you in real time.

-   Improved monitoring interfaces.

-   Friendly WebUI is provided to present the execution processes of the query tasks visually.
-   Supports the JMX protocol.

## Scenarios {#section_wpy_p22_z2b .section}

Presto is a distributed SQL engine that is well-suited to the following scenarios:

-   ETL
-   Ad-hoc queries
-   Massive structured and semi-structured data analysis
-   Massive multi-dimensional data aggregation/reports

In particular, Presto is a data warehouse product, which is not designed to replace traditional RDBMS databases such as MySQL and PostgreSQL. It has limited support for transactions and is not suitable for online service scenarios.

## Benefits {#section_ypy_p22_z2b .section}

In addition to being open source, the E-MapReduce Presto product comes with the following advantages:

-   You can purchase it for immediate use to build a Presto cluster with hundreds of nodes in minutes.
-   It supports elastic scalability, meaning that you can scale the cluster up and down with simple operations.
-   It works perfectly in connection with the E-MapReduce software stacks, and supports the processing of data stored in OSS.
-   O&M is free 24/7, providing an all-in-one service.

