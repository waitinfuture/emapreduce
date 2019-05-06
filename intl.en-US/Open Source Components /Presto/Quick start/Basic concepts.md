# Basic concepts {#concept_ldh_vvt_xgb .concept}

This section describes the basic Presto concepts for a better understanding of the Presto work mechanism.

## Data model {#section_whx_yvt_xgb .section}

Data model indicates to the data organization form. Presto uses a three-level structure, namely Catalog, Schema, and Table, to manage data.

-   Catalog

    A catalog contains multiple schemas and is physically directed to an external data source, which can be accessed through connectors. When you run an SQL statement in Presto, you are running it against one or more catalogs.

-   Schema

    A schema is a database instance that contains multiple data tables.

-   Table

    A data table is the same as a general database table.


The relationships between catalogs, schemas, and tables are shown in the following figure.

![Catalog, Schema, and Table relational diagram](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17915/155711304810900_en-US.png)

## Connector {#section_gfd_5wt_xgb .section}

Presto uses connectors to connect to various external data sources. To access customized data sources, Presto provides a standard [SPI](https://prestodb.io/docs/current/develop/spi-overview.html), which allows you to develop your own connectors using this standard API.

A catalog is typically associated with a specific connector \(which can be configured in the Properties file of the catalog\). Presto contains multiple built-in connectors.

