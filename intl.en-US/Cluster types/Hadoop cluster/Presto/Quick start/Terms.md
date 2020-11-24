# Terms

This topic introduces the basic terms that are used in Presto for you to better understand the working mechanism of Presto.

## data source model

A data source model is a data organization form. Presto uses three levels of components to manage data, which are catalogs, schemas, and tables.

-   Catalog

    A catalog contains multiple schemas and references an external data source, which can be accessed by using connectors. When you execute an SQL statement in Presto, you can access one or more catalogs.

-   Schema

    A schema is a database instance that contains multiple data tables.

-   Table

    A data table is the same as a general database table.


The following figure shows the relationships between catalogs, schemas, and tables.

![Relationships between catalogs, schemas, and tables](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/2513249751/p10900.png)

## connector

Presto uses connectors to connect to various external data sources. Presto provides a standard [SPI](https://prestodb.io/docs/current/develop/spi-overview.html), which allows you to develop your own connectors to access custom data sources.

A catalog is typically associated with a specific type of connector that is configured in the Properties file of the catalog. Presto contains multiple built-in connectors.

