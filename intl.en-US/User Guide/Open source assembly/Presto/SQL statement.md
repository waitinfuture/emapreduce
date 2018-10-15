# SQL statement {#concept_k1k_mhl_z2b .concept}

SQL statement

## ALTER SCHEMA {#section_fk2_2cf_1fb .section}

-   **Synopsis**

    ```
    ALTER SCHEMA name RENAME TO new_name
    ```

-   **Description**

    Renames SCHEMA.

-   **Examples**

    ```
    ALTER SCHEMA web RENAME TO traffic -- Renames Schema 'web' as 'traffic'
    ```


## ALTER TABLE {#section_sjf_ncf_1fb .section}

-   **Synopsis**

    ```
    ALTER TABLE name RENAME TO new_name
    ALTER TABLE name ADD COLUMN column_name data_type
    ALTER TABLE name DROP COLUMN column_name
    ALTER TABLE name RENAME COLUMN column_name TO new_column_name
    ```

-   **Description**

    Changes the definition of an existing table

-   **Examples**

    ```
    ALTER TABLE users RENAME TO people; --- Rename
    ALTER TABLE users ADD COLUMN zip varchar; --- Add column
    ALTER TABLE users DROP COLUMN zip; --- Drop column
    ALTER TABLE users RENAME COLUMN id TO user_id; --- Rename column
    ```


## CALL {#section_ph3_ncf_1fb .section}

-   **Synopsis**

    ```
    CALL procedure_name ( [ name => ] expression [, ...] )
    ```

-   **Description**

    Calls a stored procedure. Stored procedures can be provided by connectors to perform data manipulation or administrative tasks. Some connectors such as the PostgreSQL Connector, are for systems that have their own stored procedures. These systems must use the stored procedures provided by the connectors to access their own stored procedures, which are not directly callable via **CALL**.

-   **Examples**

    ```
    CALL test(123, 'apple'); --- Call a stored procedure using positional arguments
    CALL test(name => 'apple', id => 123); --- Call a stored procedure using named arguments
    CALL catalog.schema.test(); --- Call a stored procedure using a fully qualified name
    ```


## COMMIT {#section_bq5_ddf_1fb .section}

-   **Synopsis**

    ```
    COMMIT [WORK]
    ```

-   **Description**

    Commits the current transaction.

-   **Examples**

    ```
    COMMIT;
    COMMIT WORK;
    ```


## CREATE SCHEMA {#section_pkx_ddf_1fb .section}

-   **Synopsis**

    ```
    CREATE SCHEMA [ IF NOT EXISTS ] schema_name
    [ WITH ( property_name = expression [, ...] ) ]
    ```

-   **Description**

    Creates a new SCHEMA. Schema is a container that holds tables, views, and other database objects.

    -   The optional `IF NOT EXISTS` clause causes the error to be suppressed if the schema already exists;
    -   The optional `WITH` clause can be used to set properties on the newly created schema. To list all available schema properties, run the following query:

        ```
        SELECT * FROM system.metadata.schema_properties;
        ```

-   **Examples**

    ```
    CREATE SCHEMA web;
    CREATE SCHEMA hive.sales;
    CREATE SCHEMA IF NOT EXISTS traffic;
    ```


## CREATE TABLE {#section_sgd_rdf_1fb .section}

-   **Synopsis**

    ```
    CREATE TABLE [ IF NOT EXISTS ]
    table_name (
      { column_name data_type [ COMMENT comment ]
      | LIKE existing_table_name [ { INCLUDING | EXCLUDING } PROPERTIES ] }
      [, ...]
    )
    [ COMMENT table_comment ]
    [ WITH ( property_name = expression [, ...] ) ]
    ```

-   **Description**

    Creates an empty table. Use the `CREATE TABLE AS` to create a table from an existing data set.

    -   The optional `IF NOT EXISTS` clause causes the error to be suppressed if the table already exists.
    -   The optional `WITH` clause can be used to set properties on the newly created table. To list all available table properties, run the following query:

```
SELECT * FROM system.metadata.table_properties;
```

    -   The `LIKE` clause can be used to include all the column definitions from an existing table in the new table. Multiple `LIKE` clauses may be specified.
    -   If `INCLUDING PROPERTIES` is specified, all of the table properties are copied to a new table. If the `WITH` clause specifies the same property name as one of the copied properties using `INCLUDING PROPERTIES`, the value from the `WITH` clause is used. The default behavior is `EXCLUDING PROPERTIES`.
-   **Examples**

    ```
    --- Create a new table orders:
    CREATE TABLE orders (
      orderkey bigint,
      orderstatus varchar,
      totalprice double,
      orderdate date
    )
    WITH (format = 'ORC')
    --- Create the table orders if it does not already exist, adding a table comment and a column comment:
    CREATE TABLE IF NOT EXISTS orders (
      orderkey bigint,
      orderstatus varchar,
      totalprice double COMMENT 'Price in cents.',
      orderdate date
    )
    COMMENT 'A table to keep track of orders.'
    Create the table bigger_orders, using some column definitions from orders:
    CREATE TABLE bigger_orders (
      another_orderkey bigint,
      LIKE orders,
      another_orderdate date
    )
    ```


## CREATE TABLE AS {#section_csn_22f_1fb .section}

-   **Synopsis**

    ```
    CREATE TABLE [ IF NOT EXISTS ] table_name [ ( column_alias, ... ) ]
    [ COMMENT table_comment ]
    [ WITH ( property_name = expression [, ...] ) ]
    AS query
    [ WITH [ NO ] DATA ]
    ```

-   **Description**

    Creates a new table containing the result of a `SELECT` query.

    -   The optional `IF NOT EXISTS` clause causes the error to be suppressed if the table already exists.
    -   The optional `WITH` clause can be used to set properties on the newly created table. To list all available table properties, run the following query:

        ```
        SELECT * FROM system.metadata.table_properties;
        ```

-   **Examples**

    ```
    --- Select two columns from orders to create a new table
    CREATE TABLE orders_column_aliased (order_date, total_price)
    AS
    SELECT orderdate, totalprice
    FROM orders
    --- Create a new table using the aggregate function
    CREATE TABLE orders_by_date
    COMMENT 'Summary of orders by date'
    WITH (format = 'ORC')
    AS
    SELECT orderdate, sum(totalprice) AS price
    FROM orders
    GROUP BY orderdate
    --- Create a new table, using the **IF NOT EXISTS** clause
    CREATE TABLE IF NOT EXISTS orders_by_date AS
    SELECT orderdate, sum(totalprice) AS price
    FROM orders
    GROUP BY orderdate
    --- Create a new table with the same schema as nation and no data
    Create Table maid
    SELECT *
    FROM nation
    WITH NO DATA
    ```


## CREATE VIEW {#section_ipq_22f_1fb .section}

-   **Synopsis**

    ```
    CREATE [ OR REPLACE ] VIEW view_name AS query
    ```

-   **Description**

    Creates a view. The view is a logic table that does not contain any data. It can be referenced by future queries. The query stored by the view is run every time the view is referenced by another query.

    The optional `OR REPLAE` clause causes the view to be replaced if it already exists rather than raising an error.

-   **Examples**

    ```
    --- Create a simple view
    CREATE VIEW test AS
    SELECT orderkey, orderstatus, totalprice / 2 AS half
    FROM orders
    --- Create view using the aggregate function
    CREATE VIEW orders_by_date AS
    SELECT orderdate, sum(totalprice) AS price
    FROM orders
    GROUP BY orderdate
    --- Create a view that replaces an existing view
    CREATE OR REPLACE VIEW test AS
    SELECT orderkey, orderstatus, totalprice / 4 AS quarter
    FROM orders
    ```


## DEALLOCATE PREPARE {#section_i1p_s2f_1fb .section}

-   **Synopsis**

    ```
    DEALLOCATE PREPARE statement_name
    ```

-   **Synopsis**

    Removes a statement with the name statement\_name from the list of prepared statements in a session.

-   **Examples**

    ```
    --- Deallocate a statement named my_query
    DEALLOCATE PREPARE my_query;
    ```


## DELETE {#section_dqp_s2f_1fb .section}

-   **Synopsis**

    ```
    DELETE FROM table_name [ WHERE condition ]
    ```

-   **Description**

    If the `WHERE` clause is specified, delete the matching rows from the table. If the `WHERE` is not specified, all rows from the table are deleted.

-   **Examples**

    ```
    --- Delete the matching row
    DELETE FROM lineitem WHERE shipmode = 'AIR';
    --- Delete the matching row
    DELETE FROM lineitem
    WHERE orderkey IN (SELECT orderkey FROM orders WHERE priority = 'LOW');
    --- Clear the table
    DELETE FROM orders;
    ```

-   **Limitations**

    Some connectors have limits or no support for `DELETE`.


## DESCRIBE {#section_tjq_bff_1fb .section}

-   **Synopsis**

    ```
    DESCRIBE table_name
    ```

-   **Description**

    Retrieves the table definitions, and is an alias for [SHOW COLUMNS](#).

-   **Examples**

    ```
    DESCRIBE orders;
    ```


## DESCRIBE INPUT {#section_sxs_bff_1fb .section}

-   **Synopsis**

    ```
    DESCRIBE INPUT statement_name
    ```

-   **Description**

    Lists the input parameters of a prepared statement along with the position and type of each parameter.

-   **Examples**

    ```
    --- Create a pre-compiled query 'my_ select1'
    PREPARE my_select1 FROM
    SELECT ? From nation where regionkey =? AND name < ? ;
    --- Get the descriptive information of this prepared statement
    DESCRIBE INPUT my_select1;
    ```

    DESCRIBE INPUT my\_select1;

    ```
    Position | Type
    --------------------
            0 | unknown
            1 | bigint
            2 | varchar
    (3 rows)
    ```


## DESCRIBE OUTPUT {#section_t2v_bff_1fb .section}

-   **Synopsis**

    ```
    DESCRIBE OUTPUT statement_name
    ```

-   **Description**

    Lists the output columns of a prepared statement, including the column name \(or alias\), catalog, schema, table name, type, type size in bytes, and a boolean indicating if the column is aliased.

-   **Examples**
    -   Example one

        Prepare a prepared statement:

        ```
        PREPARE my_select1 FROM
        SELECT * FROM nation;
        ```

        Execute `DESCRIBE OUTPUT`, which outputs:

        ```
        DESCRIBE OUTPUT my_select1;
         Column Name | Catalog | Schema | Table  |  Type   | Type Size | Aliased
        -------------+---------+--------+--------+---------+-----------+---------
         nationkey   | tpch    | sf1    | nation | bigint  |         8 | false
         name        | tpch    | sf1    | nation | varchar |         0 | false
         regionkey   | tpch    | sf1    | nation | bigint  |         8 | false
         comment     | tpch    | sf1    | nation | varchar |         0 | false
        (4 rows)
        ```

    -   Example two

        ```
        PREPARE my_select2 FROM
        SELECT count(*) as my_count, 1+2 FROM nation
        ```

        Execute `DESCRIBE OUTPUT`, which outputs:

        ```
        DESCRIBE OUTPUT my_select2;
         Column Name | Catalog | Schema | Table |  Type  | Type Size | Aliased
        -------------+---------+--------+-------+--------+-----------+---------
         my_count    |         |        |       | bigint |         8 | true
         _col1       |         |        |       | bigint |         8 | false
        (2 rows)
        ```

    -   Example three:

        ```
        PREPARE my_create FROM
        CREATE TABLE foo AS SELECT * FROM nation;
        ```

        Execute `DESCRIBE OUTPUT`, which outputs:

        ```
        DESCRIBE OUTPUT my_create;
         Column Name | Catalog | Schema | Table |  Type  | Type Size | Aliased
        -------------+---------+--------+-------+--------+-----------+---------
         rows        |         |        |       | bigint |         8 | false
        (1 row)
        ```


## DROP SCHEMA {#section_twp_n4f_1fb .section}

-   **Synopsis**

    ```
    DROP SCHEMA [ IF EXISTS ] schema_name
    ```

-   **Description**

    Drops an existing Schema.

    -   The schema must be empty.
    -   The optional `IF EXISTS` clause causes the error to be suppressed if the schema does not exist.
-   **Examples**

    ```
    DROP SCHEMA web;
    DROP TABLE IF EXISTS sales;
    ```


## DROP TABLE {#section_brs_n4f_1fb .section}

-   **Synopsis**

    ```
    DROP TABLE [ IF EXISTS ] table_name
    ```

-   **Description**

    Drops an existing table. The optional **IF EXISTS** clause causes the error to be suppressed if the table does not exist.

-   **Examples**

    ```
    DROP TABLE orders_by_date;
    DROP TABLE IF EXISTS orders_by_date;
    ```


## DROP VIEW {#section_wdt_n4f_1fb .section}

-   **Synopsis**

    ```
    DROP VIEW [ IF EXISTS ] view_name
    ```

-   **Description**

    Drops an existing view. The optional **IF EXISTS** clause causes the error to be suppressed if the view does not exist.

-   **Examples**

    ```
    DROP VIEW orders_by_date;
    DROP VIEW IF EXISTS orders_by_date;
    ```


## EXECUTE {#section_zkg_v4f_1fb .section}

-   **Synopsis**

    ```
    EXECUTE statement_name [ USING parameter1 [ , parameter2, ... ] ]
    ```

-   **Description**

    Executes a prepared statement. Parameter values are defined in the **USING** clause.

-   **Examples**
    -   Example one

        ```
        PREPARE my_select1 FROM
        SELECT name FROM nation;
        --- Execute a prepared statement
        EXECUTE my_select1;
        ```

    -   Example two

        ```
        PREPARE my_select2 FROM
        SELECT name FROM nation WHERE regionkey = ? and nationkey < ? ;
        --- Execute a prepared statement
        EXECUTE my_select2 USING 1, 3; 
        --- The preceding statement is equivalent to executing the following statement:
        SELECT name FROM nation WHERE regionkey = 1 AND nationkey < 3;
        ```


## EXPLAIN {#section_apj_v4f_1fb .section}

-   **Synopsis**

    ```
    EXPLAIN [ ( option [, ...] ) ] statement
    where option can be one of:
        FORMAT { TEXT | GRAPHVIZ }
        TYPE { LOGICAL | DISTRIBUTED | VALIDATE }
    ```

-   **Description**

    Achieves one of the following functions based on the option used:

    -   Shows the logical plan of a query statement
    -   Shows the distributed execution plan of a query statement
    -   Validates a query statement
    Use `TYPE DISTRIBUTED` option to display fragmented plan. Each plan fragment is executed by a single or multiple Presto nodes. Fragments separation represent the data exchange between Presto nodes. Fragment type specifies how the fragment is executed by Presto nodes and how the data is distributed between fragments. Fragment types are as follows:

    -   SINGLE: Fragment is executed on a single node.
    -   HASH: Fragment is executed on a fixed number of nodes with the input data distributed using a hash function.
    -   ROUND\_ROBIN: Fragment is executed on a fixed number of nodes with the input data distributed in a ROUND-ROBIN fashion.
    -   BROADCAST: Fragment is executed on a fixed number of nodes with the input data broadcasted to all nodes.
    -   SOURCE: Fragment is executed on nodes where input splits are accessed.
-   **Examples**
    -   Example one

        Logical plan:

        ```
        presto:tiny> EXPLAIN SELECT regionkey, count(*) FROM nation GROUP BY 1;
                                                        Query Plan
        ----------------------------------------------------------------------------------------------------------
         - Output[regionkey, _col1] => [regionkey:bigint, count:bigint]
                 _ Col1: = count?
             - RemoteExchange[GATHER] => regionkey:bigint, count:bigint
                 - Aggregate(FINAL)[regionkey] => [regionkey:bigint, count:bigint]
                        count := "count"("count_8")
                     - LocalExchange[HASH][$hashvalue] ("regionkey") => regionkey:bigint, count_8:bigint, $hashvalue:bigint
                         - RemoteExchange[REPARTITION][$hashvalue_9] => regionkey:bigint, count_8:bigint, $hashvalue_9:bigint
                             - Project[] => [regionkey:bigint, count_8:bigint, $hashvalue_10:bigint]
                                     $hashvalue_10 := "combine_hash"(BIGINT '0', COALESCE("$operator$hash_code"("regionkey"), 0))
                                 - Aggregate(PARTIAL)[regionkey] => [regionkey:bigint, count_8:bigint]
                                         count_8 := "count"(*)
                                     - TableScan[tpch:tpch:nation:sf0.1, originalConstraint = true] => [regionkey:bigint]
                                             regionkey := tpch:regionkey
        ```

    -   Example two

        Distributed plan:

        ```
        presto:tiny> EXPLAIN (TYPE DISTRIBUTED) SELECT regionkey, count(*) FROM nation GROUP BY 1;
                                                  Query Plan
        ----------------------------------------------------------------------------------------------
         Fragment 0 [SINGLE]
             Output layout: [regionkey, count]
             Output partitioning: SINGLE []
             - Output[regionkey, _col1] => [regionkey:bigint, count:bigint]
                     _col1 := count
                 - RemoteSource[1] => [regionkey:bigint, count:bigint]
         Fragment 1 [HASH]
             Output layout: [regionkey, count]
             Output partitioning: SINGLE []
             - Aggregate(FINAL)[regionkey] => [regionkey:bigint, count:bigint]
                     count := "count"("count_8")
                 - LocalExchange[HASH][$hashvalue] ("regionkey") => regionkey:bigint, count_8:bigint, $hashvalue:bigint
                     - RemoteSource[2] => [regionkey:bigint, count_8:bigint, $hashvalue_9:bigint]
         Fragment 2 [SOURCE]
             Output layout: [regionkey, count_8, $hashvalue_10]
             Output partitioning: HASH [regionkey][$hashvalue_10]
             - Project[] => [regionkey:bigint, count_8:bigint, $hashvalue_10:bigint]
                     $hashvalue_10 := "combine_hash"(BIGINT '0', COALESCE("$operator$hash_code"("regionkey"), 0))
                 - Aggregate(PARTIAL)[regionkey] => [regionkey:bigint, count_8:bigint]
                         count_8 := "count"(*)
                     - TableScan[tpch:tpch:nation:sf0.1, originalConstraint = true] => [regionkey:bigint]
                             regionkey := tpch:regionkey
        ```

    -   Example three:

        Validation:

        ```
        presto:tiny> EXPLAIN (TYPE VALIDATE) SELECT regionkey, count(*) FROM nation GROUP BY 1;
         Valid
        -------
         true
        ```


## EXPLAIN ANALYZE {#section_lrl_v4f_1fb .section}

-   **Synopsis**

    ```
    EXPLAIN ANALYZE [VERBOSE] statement
    ```

-   **Description**

    Executes the statement and shows the distributed execution plan of the statement along with the cost of each operation. The `VERBOSE` option gives more detailed information and low-level statistics.

-   **Examples**

    In the following example, you can see the CPU time spent in each stage, as well as the relative cost of each plan node in the stage. Note that the relative cost of the plan nodes is based on wall time, which may or may not be correlated to CPU time. For each plan node you can see some additional statistics, which are useful if you want to detect data anomalies for a query \(skewness, abnormal hash collisions\).

    ```
    presto:sf1> EXPLAIN ANALYZE SELECT count(*), clerk FROM orders WHERE orderdate > date '1995-01-01' GROUP BY clerk;
                                              Query Plan
    -----------------------------------------------------------------------------------------------
    Fragment 1 [HASH]
        Cost: CPU 88.57ms, Input: 4000 rows (148.44kB), Output: 1000 rows (28.32kB)
        Output layout: [count, clerk]
        Output partitioning: SINGLE []
        - Project[] => [count:bigint, clerk:varchar(15)]
                Cost: 26.24%, Input: 1000 rows (37.11kB), Output: 1000 rows (28.32kB), Filtered: 0.00%
                Input avg.: 62.50 lines, Input std.dev.: 14.77%
            - Aggregate(FINAL)[clerk][$hashvalue] => [clerk:varchar(15), $hashvalue:bigint, count:bigint]
                    Cost: 16.83%, Output: 1000 rows (37.11kB)
                    Input avg.: 250.00 lines, Input std.dev.: 14.77%
                    count := "count"("count_8")
                - LocalExchange[HASH][$hashvalue] ("clerk") => clerk:varchar(15), count_8:bigint, $hashvalue:bigint
                        Cost: 47.28%, Output: 4000 rows (148.44kB)
                        Input avg.: 4000.00 lines, Input std.dev.: 0.00%
                    - RemoteSource[2] => [clerk:varchar(15), count_8:bigint, $hashvalue_9:bigint]
                            Cost: 9.65%, Output: 4000 rows (148.44kB)
                            Input avg.: 4000.00 lines, Input std.dev.: 0.00%
    Fragment 2 [tpch:orders:1500000]
        Cost: CPU 14.00s, Input: 818058 rows (22.62MB), Output: 4000 rows (148.44kB)
        Output layout: [clerk, count_8, $hashvalue_10]
        Output partitioning: HASH [clerk][$hashvalue_10]
        - Aggregate(PARTIAL)[clerk][$hashvalue_10] => [clerk:varchar(15), $hashvalue_10:bigint, count_8:bigint]
                Cost: 4.47%, Output: 4000 rows (148.44kB)
                Input avg.: 204514.50 lines, Input std.dev.: 0.05%
                Collisions avg.: 5701.28 (17569.93% est.), Collisions std.dev.: 1.12%
                count_8 := "count"(*)
            - ScanFilterProject[table = tpch:tpch:orders:sf1.0, originalConstraint = ("orderdate" > "$literal$date"(BIGINT '9131')), filterPredicate = ("orderdate" > "$literal$date"(BIGINT '9131'))] => [cler
                    Cost: 95.53%, Input: 1500000 rows (0B), Output: 818058 rows (22.62MB), Filtered: 45.46%
                    Input avg.: 375000.00 lines, Input std.dev.: 0.00%
                    $hashvalue_10 := "combine_hash"(BIGINT '0', COALESCE("$operator$hash_code"("clerk"), 0))
                    orderdate := tpch:orderdate
                    clerk := tpch:clerk
    ```

    When the `VERBOSE` option is used, some operators may report additional information.

    ```
    EXPLAIN ANALYZE VERBOSE SELECT count(clerk) OVER() FROM orders WHERE orderdate > date '1995-01-01';
                                              Query Plan
    -----------------------------------------------------------------------------------------------
      ...
             - Window[] => [clerk:varchar(15), count:bigint]
                     Cost: {rows: ?, bytes: ?}
                     CPU fraction: 75.93%, Output: 8130 rows (230.24kB)
                     Input avg.: 8130.00 lines, Input std.dev.: 0.00%
                     Active Drivers: [ 1 / 1 ]
                     Index size: std.dev.: 0.00 bytes , 0.00 rows
                     Index count per driver: std.dev.: 0.00
                     Rows per driver: STD. Dev.: 0.00
                     Size of partition: std.dev.: 0.00
                     count := count("clerk")
     ...
    ```


## GRANT {#section_z3r_2qf_1fb .section}

-   **Synopsis**

    ```
    GRANT ( privilege [, ...] | ( ALL PRIVILEGES ) )
    ON [ TABLE ] table_name TO ( grantee | PUBLIC )
    [ WITH GRANT OPTION ]
    ```

-   **Description**

    Grants the specified privileges to the specified grantee.

    -   Specifying `ALL PRIVILEGES` grants DELETE, INSERT and SELECT privileges.
    -   Specifying `PUBLIC`grants privileges to the PUBLIC role and hence to all users.
    -   The optional `WITH GRANT OPTION` clause allows the grantee to grant these same privileges to others.
-   **Examples**

    ```
    GRANT INSERT, SELECT ON orders TO alice; --- Grant privileges to user alice
    GRANT SELECT ON nation TO alice WITH GRANT OPTION; --- Grant SELECT privilege to user alice, additionally allowing alice to grant **SELECT** privilege to others
    GRANT SELECT ON orders TO PUBLIC; --- Grant **SELECT** privilege on the table order to everyone
    ```

-   **Limitations**

    Some connectors have no support for `GRANT`.


## INSERT {#section_c25_2qf_1fb .section}

-   **Synopsis**

    ```
    INSERT INTO table_name [ ( column [, ... ] ) ] query
    ```

-   **Description**

    Inserts new rows into a table. If the list of column names is specified, they must exactly match the list of columns produced by the `query`. Each column in the table not present in the column list is filled with a `null` value.

-   **Examples**

    ```
    INSERT INTO orders SELECT * FROM new_orders; --- Insert the SELECT results into the orders table.
    INSERT INTO cities VALUES (1, 'San Francisco'); --- Insert a single row
    INSERT INTO cities VALUES (2, 'San Jose'), (3, 'Oakland'); --- Insert multiple rows
    INSERT INTO nation (nationkey, name, regionkey, comment) VALUES (26, 'POLAND', 3, 'no comment'); --- Insert a single row
    INSERT INTO nation (nationkey, name, regionkey) VALUES (26, 'POLAND', 3); --- Inserts a single row (only includes some columns)
    ```


## PREPARE {#section_tlw_2qf_1fb .section}

-   **Synopsis**

    ```
    PREPARE statement_name FROM statement
    ```

-   **Description**

    Prepares a statement for execution at a later time. Prepared statements are queries saved in a session with a given name. The statement can include parameters in place of literals to be replaced at execution time. Parameters are represented by ?.

-   **Examples**

    ```
    --- Prepare a query that does not include parameters
    PREPARE my_select1 FROM
    SELECT * FROM nation;
    --- Prepare a query that includes parameters
    PREPARE my_select2 FROM
    SELECT name FROM nation WHERE regionkey = ? AND nationkey < ? ;
    --- Prepare an insert statement that does not include parameters
    PREPARE my_insert FROM
    INSERT INTO cities VALUES (1, 'San Francisco');
    ```


## RESET SESSION {#section_rky_2qf_1fb .section}

-   **Synopsis**

    ```
    RESET SESSION name
    RESET SESSION catalog.name
    ```

-   **Description**

    Reset a session property value to the default value.

-   **Examples**

    ```
    RESET SESSION optimize_hash_generation;
    RESET SESSION hive.optimized_reader_enabled;
    ```


## REVOKE {#section_nsv_fqf_1fb .section}

-   **Synopsis**

    ```
    REVOKE [ GRANT OPTION FOR ]
    (Privilege [,...] | ALL PRIVILEGES )
    ON [ TABLE ] table_name FROM ( grantee | PUBLIC )
    ```

-   **Description**

    Revokes the specified privileges from the specified grantee.

    -   Specifying `ALL PRIVILEGE` revokes `SELECT`, `INSERT` and `DELETE` privileges.
    -   Specifying `PUBLIC` revokes privileges from the `PUBLIC` role. Users will retain privileges assigned to them directly or via other roles.
    -   The optional `GRANT OPTION FOR` clause also revokes the privileges to `GRANT` the specified privileges.
    -   Usage of the term `grantee` denotes both users and roles.
-   **Examples**

    ```
    --- Revoke INSERT and SELECT privileges on the table orders from user alice
    REVOKE INSERT, SELECT ON orders FROM alice;
    --- Revoke SELECT privilege on the table nation from everyone,
    --- additionally revoking the privilege to grant SELECT privilege to others
    REVOKE GRANT OPTION FOR SELECT ON nation FROM PUBLIC;
    --- Revoke all privileges on the table test from user alice
    REVOKE ALL PRIVILEGES ON test FROM alice;
    ```

-   **Limitations**

    Some connectors have no support for `REVOKE`.


## ROLLBACK {#section_jsl_lrf_1fb .section}

-   **Synopsis**

    ```
    ROLLBACK [ WORK ] 
    ```

-   **Description**

    Rollback the current transaction.

-   **Examples**

    ```
    ROLLBACK;
    ROLLBACK WORK;
    ```


## SELECT {#section_a2s_qrf_1fb .section}

-   **Synopsis**

    ```
    [ WITH with_query [, ...] ]
    SELECT [ ALL | DISTINCT ] select_expr [, ...]
    [ FROM from_item [, ...] ]
    [ WHERE condition ]
    [ GROUP BY [ ALL | DISTINCT ] grouping_element [, ...] ]
    [ HAVING condition]
    [ { UNION | INTERSECT | EXCEPT } [ ALL | DISTINCT ] select ]
    [ ORDER BY expression [ ASC | DESC ] [, ...] ]
    [ LIMIT [ count | ALL ] ]
    ```

    where `from_item` is one of:

    ```
    Table_name [[as] alias [(column_alias [,...] ) ] ]
    ```

    ```
    From_item join_type from_item [ON join_condition | using (join_column [,...] ) ]
    ```

    and `join_type` is one of:

    -   \[ INNER \] JOIN
    -   LEFT \[ OUTER \] JOIN
    -   RIGHT \[ OUTER \] JOIN
    -   FULL \[ OUTER \] JOIN
    -   CROSS JOIN
    and `grouping_element` is one of:

    -   \(\)
    -   expression
    -   GROUPING SETS \( \( column \[, …\] \) \[, …\] \)
    -   CUBE \( column \[, …\] \)
    -   ROLLUP \( column \[, …\] \)
-   **Description**

    Retrieve rows from zero or more tables to get data sets.

-   **WITH clause**
    -   **Basic functions**

        The WITH clause defines named relations for use within a query. It allows flattening nested queries or simplifying subqueries. For example, the following queries are equivalent:

        ```
        --- The WITH clause is not used
        SELECT a, b
        FROM (
          SELECT a, MAX(b) AS b FROM t GROUP BY a
        ) AS x;
        --- The WITH clause is used, and the query statement looks to be much clearer
        WITH x AS (SELECT a, MAX(b) AS b FROM t GROUP BY a)
        SELECT a, b FROM x;
        ```

    -   **Define multiple subqueries**

        The WITH clause can be used to define multiple subqueries:

        ```
        WITH
          t1 AS (SELECT a, MAX(b) AS b FROM x GROUP BY a),
          t2 AS (SELECT a, AVG(d) AS d FROM y GROUP BY a)
        SELECT t1.*, t2. *
        FROM t1
        JOIN t2 ON t1.a = t2.a;
        ```

    -   **Form a chain structure**

        Additionally, the relations within a WITH clause can chain:

        ```
        WITH
          x AS (SELECT a FROM t),
          y AS (SELECT a AS b FROM x),
          z AS (SELECT b AS c FROM y)
        SELECT c FROM z;
        ```

-   **GROUP BY clause**
    -   **Basic functions**

        The `GROUP BY` clause divides the output of a `SELECT` statement into groups of rows containing matching values. A simple `GROUP BY` clause may contain any expression composed of input columns or it may be an ordinal number selecting an output column by position \(starting at one\).

        The following queries are equivalent \(position for the `nationkey` column is two\).

        ```
        --- Using the ordinal number
        SELECT count(*), nationkey FROM customer GROUP BY 2;
        --- Using the input column name
        SELECT count(*), nationkey FROM customer GROUP BY nationkey;
        ```

        `GROUP BY` clauses can group output by input column names not appearing in the output of a select statement, for example:

        ```
        --- The mktsegment column has not been specified in the SELECT list.
        --- The result set does not contain content of the mktsegment column.
        SELECT count(*) FROM customer GROUP BY mktsegment;
         _col0
        -------
         29968
         30142
         30189
         29949
         29752
        (5 rows)
        ```

        **Note:** When a `GROUP BY` BY clause is used in a `SELECT` statement, all output expressions must be either aggregate functions or columns present in the `GROUP BY` BY clause.

    -   **Complex grouping operations**

        Presto supports the following three complex aggregation syntaxes, which allows users to perform analysis that requires aggregation on multiple sets of columns in a single query:

        -   **GROUPING SETS**

            `CUBE` `ROLLUP`

            The shipping table is a data table with five columns, which are shown as follows:

            ```
            SELECT * FROM shipping;
             origin_state | origin_zip | destination_state | destination_zip | package_weight
            --------------+------------+-------------------+-----------------+----------------
             California   |      94131 | New Jersey        |            8648 |             13
             California   |      94131 | New Jersey        |            8540 |             42
             New Jersey   |       7081 | Connecticut       |            6708 |            225
             California   |      90210 | Connecticut       |            6927 |           1337
             California   |      94131 | Colorado          |           80302 |              5
             New York     |      10002 | New Jersey        |            8540 |              3
            (6 rows)
            ```

            Now we want to retrieve the following grouping results using a single query statement:

            -   Group by origin\_state, and get the total package\_weight.
            -   Group by origin\_state and origin\_zip, and get the total package\_weight.
            -   Group by destination\_state, and get the total package\_weight.
            `GROUPING SETS` allows users to retrieve the result set of the above three groups with a single query statement, as shown below:

            ```
            SELECT origin_state, origin_zip, destination_state, sum(package_weight)
            FROM shipping
            GROUP BY GROUPING SETS (
                (origin_state),
                (origin_state, origin_zip),
                (destination_state));
             origin_state | origin_zip | destination_state | _col0
            --------------+------------+-------------------+-------
             New Jersey   | NULL       | NULL              |   225
             California   | NULL       | NULL              |  1397
             New York     | NULL       | NULL              |     3
             California   |      90210 | NULL              |  1337
             California   |      94131 | NULL              |    60
             New Jersey   |       7081 | NULL              |   225
             New York     |      10002 | NULL              |     3
             NULL         | NULL       | Colorado          |     5
             NULL         | NULL       | New Jersey        |    58
             NULL         | NULL       | Connecticut       |  1562
            (10 rows)
            ```

            The preceding query may be considered logically equivalent to a `UNION ALL` of multiple `GROUP BY` queries:

            ```
            SELECT origin_state, NULL, NULL, sum(package_weight)
            FROM shipping GROUP BY origin_state
            UNION ALL
            SELECT origin_state, origin_zip, NULL, sum(package_weight)
            FROM shipping GROUP BY origin_state, origin_zip
            UNION ALL
            SELECT NULL, NULL, destination_state, sum(package_weight)
            FROM shipping GROUP BY destination_state;
            ```

            However, the query with the complex grouping syntax \(such as `GROUPING SETS`\) only reads from the underlying data source once, while the query with the `UNION ALL` reads the underlying data three times. This is why queries with a `UNION ALL` may produce inconsistent results when the data source is not deterministic.

        -   **CUBE**

            The **CUBE** operator generates all possible grouping sets for a given set of columns. For example, the query:

            ```
            SELECT origin_state, destination_state, sum(package_weight)
            FROM shipping
            Group by cube (glas_state, destiny _ State );
             origin_state | destination_state | _col0
            --------------+-------------------+-------
             California   | New Jersey        |    55
             California   | Colorado          |     5
             New York     | New Jersey        |     3
             New Jersey   | Connecticut       |   225
             California   | Connecticut       |  1337
             California   | NULL              |  1397
             New York     | NULL              |     3
             New Jersey   | NULL              |   225
             NULL         | New Jersey        |    58
             NULL         | Connecticut       |  1562
             NULL         | Colorado          |     5
             NULL         | NULL              |  1625
            (12 rows)
            ```

            is equivalent to:

            ```
            SELECT origin_state, destination_state, sum(package_weight)
            FROM shipping
            GROUP BY GROUPING SETS (
                (origin_state, destination_state),
                (origin_state),
                (destination_state),
                ());
            ```

        -   **ROLLUP**

            The **ROLLUP** operator generates all possible subtotals for a given set of columns. For example, the query:

            ```
            SELECT origin_state, origin_zip, sum(package_weight)
            FROM shipping
            GROUP BY ROLLUP (origin_state, origin_zip);
             origin_state | origin_zip | _col2
            --------------+------------+-------
             California   |      94131 |    60
             California   |      90210 |  1337
             New Jersey   |       7081 |   225
             New York     |      10002 |     3
             California   | NULL       |  1397
             New York     | NULL       |     3
             New Jersey   | NULL       |   225
             NULL         | NULL       |  1625
            (8 rows)
            ```

            is equivalent to:

            ```
            SELECT origin_state, origin_zip, sum(package_weight)
            FROM shipping
            GROUP BY GROUPING SETS ((origin_state, origin_zip), (origin_state), ());
            ```

        -   **Combining multiple grouping expressions**

            The following three statements are equivalent:

            ```
            SELECT origin_state, destination_state, origin_zip, sum(package_weight)
            FROM shipping
            GROUP BY
                GROUPING SETS ((origin_state, destination_state)),
                ROLLUP (origin_zip);
            ```

            ```
            SELECT origin_state, destination_state, origin_zip, sum(package_weight)
            FROM shipping
            GROUP BY
                GROUPING SETS ((origin_state, destination_state)),
                GROUPING SETS ((origin_zip), ());
            ```

            ```
            SELECT origin_state, destination_state, origin_zip, sum(package_weight)
            FROM shipping
            GROUP BY GROUPING SETS (
                (origin_state, destination_state, origin_zip),
                (origin_state, destination_state));
            ```

            Output results are as follows:

            ```
            origin_state | destination_state | origin_zip | _col3
            --------------+-------------------+------------+-------
             New York     | New Jersey        |      10002 |     3
             California   | New Jersey        |      94131 |    55
             New Jersey   | Connecticut       |       7081 |   225
             California   | Connecticut       |      90210 |  1337
             California   | Colorado          |      94131 |     5
             New York     | New Jersey        | NULL       |     3
             New Jersey   | Connecticut       | NULL       |   225
             California   | Colorado          | NULL       |     5
             California   | Connecticut       | NULL       |  1337
             California   | New Jersey        | NULL       |    55
            (10 rows)
            ```

            In a `GROUP BY` clause, the `ALL` and `DISTINCT` quantifiers determine whether duplicate grouping sets each produce distinct output rows. For example, the query:

            ```
            SELECT origin_state, destination_state, origin_zip, sum(package_weight)
            FROM shipping
            GROUP BY ALL
                CUBE (origin_state, destination_state),
                ROLLUP (origin_state, origin_zip);
            ```

            is equivalent to

            ```
            SELECT origin_state, destination_state, origin_zip, sum(package_weight)
            FROM shipping
            GROUP BY GROUPING SETS (
                (origin_state, destination_state, origin_zip),
                (origin_state, origin_zip),
                (origin_state, destination_state, origin_zip),
                (origin_state, origin_zip),
                (origin_state, destination_state),
                (origin_state),
                (origin_state, destination_state),
                (origin_state),
                (origin_state, destination_state),
                (origin_state),
                (destination_state),
                ());
            ```

            Multiple duplicate grouping sets are available. However, if the query uses the `DISTINCT` quantifier, only unique grouping sets are generated.

            ```
            SELECT origin_state, destination_state, origin_zip, sum(package_weight)
            FROM shipping
            GROUP BY DISTINCT
                CUBE (origin_state, destination_state),
                ROLLUP (origin_state, origin_zip);
            ```

            is equivalent to

            ```
            SELECT origin_state, destination_state, origin_zip, sum(package_weight)
            FROM shipping
            GROUP BY GROUPING SETS (
                (origin_state, destination_state, origin_zip),
                (origin_state, origin_zip),
                (origin_state, destination_state),
                (origin_state),
                (destination_state),
                ());
            ```

            **Note:** The default set quantifier for `GROUP BY` BY is `ALL`.

    -   **GROUPING operation**

        Presto provides a `grouping` operation that returns a bit set converted to decimal, indicating which columns are present in a grouping. The semantics is demonstrated as follows:

        ```
        grouping(col1, ..., colN) -> bigint
        ```

        `grouping` is used in conjunction with `GROUPING SETS`, `ROLLUP`, `CUBE` or `GROUP BY`. `grouping` columns must match exactly the columns referenced in the corresponding `GROUPING SETS`, `ROLLUP`, `CUBE` or `GROUP BY` clause.

        ```
        SELECT origin_state, origin_zip, destination_state, sum(package_weight),
               grouping(origin_state, origin_zip, destination_state)
        FROM shipping
        GROUP BY GROUPING SETS (
                (origin_state),
                (origin_state, origin_zip),
                (destination_state));
        origin_state | origin_zip | destination_state | _col3 | _col4
        --------------+------------+-------------------+-------+-------
        California   | NULL       | NULL              |  1397 |     3   --- 011
        New Jersey   | NULL       | NULL              |   225 |     3   --- 011
        New York     | NULL       | NULL              |     3 |     3   --- 011
        California   |      94131 | NULL              |    60 |     1   --- 001
        New Jersey   |       7081 | NULL              |   225 |     1   --- 001
        California   |      90210 | NULL              |  1337 |     1   --- 001
        New York     |      10002 | NULL              |     3 |     1   --- 001
        NULL         | NULL       | New Jersey        |    58 |     6   --- 100
        NULL         | NULL       | Connecticut       |  1562 |     6   --- 100
        NULL         | NULL       | Colorado          |     5 |     6   --- 100
        (10 rows)
        ```

        As shown in the preceding table, bits are assigned to the argument columns with the rightmost column being the least significant bit. For a given `grouping`, a bit is set to 0 if the corresponding column is included in the grouping and to 1 otherwise.

-   **HAVING clause**

    The `HAVING` clause is used in conjunction with aggregate functions and the `GROUP BY` clause to control which groups are selected. A `HAVING` clause will be executed after completion of grouping and aggregation, to eliminate groups that do not satisfy the given conditions.

    The following example selects user groups with an account balance greater than 5700000:

    ```
    SELECT count(*), mktsegment, nationkey,
           CAST(sum(acctbal) AS bigint) AS totalbal
    FROM customer
    GROUP BY mktsegment, nationkey
    HAVING sum(acctbal) > 5700000
    ORDER BY totalbal DESC;
    ```

    The output is as follows:

    ```
    _col0 | mktsegment | nationkey | totalbal
    -------+------------+-----------+----------
      1272 | AUTOMOBILE |        19 |  5856939
      1253 | FURNITURE  |        14 |  5794887
      1248 | FURNITURE  |         9 |  5784628
      1243 | FURNITURE  |        12 |  5757371
      1231 | HOUSEHOLD  |         3 |  5753216
      1251 | MACHINERY  |         2 |  5719140
      1247 | FURNITURE  |         8 |  5701952
    (7 rows)
    ```

-   **Set operations**

    Presto supports three set operations, namely `UNION`, `INTERSECT`, and `EXCEPT`. These clauses are used to combine the results of more than one query statement into a single result set. The usage is as follows:

    ```
    query UNION [ALL | DISTINCT] query
    query INTERSECT [DISTINCT] query
    query EXCEPT [DISTINCT] query
    ```

    The argument ALL or DISTINCT controls which rows are included in the final result set, and the default is DISTINCT.

    -   ALL: may return duplicated rows;
    -   parmnamepar DISTINCTparmname : eliminates duplicated rows.
    The ALL argument is not supported for `INTERSECT` or `EXCEPT`.

    The above three set operations are processed left to right, and `INTERSECT` has the highest priority. That means `A UNION B INTERSECT C EXCEPT D` is the same as `A UNION (B INTERSECT C) EXCEPT D`.

-   **UNION**

    `UNION` combines two query result sets, and uses the ALL and DISTINCT arguments to control whether or not to remove duplicates.

    -   Example one

        ```
        SELECT 13
        UNION
        Select 42;
         _col0
        -------
            13
            42
        (2 rows)
        ```

    -   Example two

        ```
        SELECT 13
        UNION
        SELECT * FROM (VALUES 42, 13);
         _col0
        -------
            13
            42
        (2 rows)
        ```

    -   Example three:

        ```
        SELECT 13
        UNION ALL
        SELECT * FROM (VALUES 42, 13);
         _col0
        -------
            13
            42
            13
        (3 rows)
        ```

-   **INTERSECT**

    `INTERSECT` returns only the rows that are in both query result sets.

    Examples

    ```
    SELECT * FROM (VALUES 13, 42)
    INTERSECT
    SELECT 13;
     _col0
    -------
       13
    (1 row)
    ```

-   **EXCEPT**

    `EXCEPT` returns the rows that are in the result set of the first query, but not the second.

    ```
    SELECT * FROM (VALUES 13, 42)
    EXCEPT
    SELECT 13;
     _col0
    -------
       42
    (1 row)
    ```

-   **ORDER BY clause**

    The `ORDER BY` clause is used to sort a result set. The semantics is demonstrated as follows:

    ```
    ORDER BY expression [ ASC | DESC ] [ NULLS { FIRST | LAST } ] [, ...]
    ```

    Where:

    -   Each expression may be composed of output columns or it may be an ordinal number selecting an output column by position \(starting at one\).
    -   The `ORDER BY` clause is the last step of a query after any `GROUP BY` or `HAVING` clause;
    -   NULLS \{ FIRST | LAST \} is used to control the sorting method of the NULL value \(regardless of ASC or DESC\), and the default null ordering is LAST.
-   **LIMIT clause**

    The `LIMIT` clause restricts the number of rows in the result set. `LIMIT ALL` is the same as omitting the `LIMIT` clause.

    Examples

    ```
    In this example, because the query lacks an ORDER BY, exactly which rows are returned is arbitrary.
    SELECT orderdate FROM orders LIMIT 5;
     orderdate
    -------------
     1996-04-14
     1992-01-15
     1995-02-01
     1995-11-12
     1992-04-26
    (5 rows)
    ```

-   **TABLESAMPLE**

    Presto provides two sampling methods, namely BERNOULLI and SYSTEM. However, neither of the two methods allow deterministic bounds on the number of rows returned.

    -   BERNOULLI:

        Each row is selected to be in the table sample with a probability of the sample percentage. When a table is sampled using the Bernoulli method, all physical blocks of the table are scanned and certain rows are skipped based on a comparison between the sample percentage and a random value calculated at runtime.

        The probability of a row being included in the result is independent from any other row. This does not reduce the time required to read the sampled table from disk. It may have an impact on the total query time if the sampled output is processed further.

    -   SYSTEM

        This sampling method divides the table into logical segments of data and samples the table at this granularity. This sampling method either selects all the rows from a particular segment of data or skips it \(based on a comparison between the sample percentage and a random value calculated at runtime\).

        The rows selected in a system sampling is dependent on which connector is used. For example, when used with Hive, it is dependent on how the data is laid out on HDFS. This method does not guarantee independent sampling probabilities.

    Examples

    ```
    --- Using BERNOULLI sampling
    SELECT *
    FROM users TABLESAMPLE BERNOULLI (50);
    --- Using system sampling
    SELECT *
    FROM users TABLESAMPLE SYSTEM (75);
    Using sampling with joins:
    --- Using sampling with JOIN
    SELECT o.*, i. *
    FROM orders o TABLESAMPLE SYSTEM (10)
    JOIN lineitem i TABLESAMPLE BERNOULLI (40)
      ON o.orderkey = i.orderkey;
    ```

-   **UNNEST**

    `UNNEST` can be used to expand an ARRAY or MAP into a relation. Arrays are expanded into a single column, and maps are expanded into two columns \(key, value\). `UNNEST` can also be used with multiple arrays and maps, in which case they are expanded into multiple columns, with as many rows as the highest cardinality argument \(the other columns are padded with nulls\). `UNNEST` can optionally have a `WITH ORDINALITY`clause, in which case an additional ordinal column is added to the end. `UNNEST` is normally used with a `JOIN` and can reference columns from relations on the left side of the join.

    -   Example one

        ```
        --- Using a single column
        SELECT student, score
        FROM tests
        CROSS JOIN UNNEST(scores) AS t (score);
        ```

    -   Example two

        ```
        --- Using multiple columns
        SELECT numbers, animals, n, a
        FROM (
          VALUES
            (ARRAY[2, 5], ARRAY['dog', 'cat', 'bird']),
            (ARRAY[7, 8, 9], ARRAY['cow', 'pig'])
        ) AS x (numbers, animals)
        CROSS JOIN UNNEST(numbers, animals) AS t (n, a);
          numbers  |     animals      |  n   |  a
        -----------+------------------+------+------
         [2, 5]    | [dog, cat, bird] |    2 | dog
         [2, 5]    | [dog, cat, bird] |    5 | cat
         [2, 5]    | [dog, cat, bird] | NULL | bird
         [7, 8, 9] | [cow, pig]       |    7 | cow
         [7, 8, 9] | [cow, pig]       |    8 | pig
         [7, 8, 9] | [cow, pig]       |    9 | NULL
        (6 rows)
        ```

    -   Example three:

        ```
        --- Using a WITH ORDINALITY clause
        SELECT numbers, n, a
        FROM (
          VALUES
            (ARRAY[2, 5]),
            (ARRAY[7, 8, 9])
        ) AS x (numbers)
        CROSS JOIN UNNEST(numbers) WITH ORDINALITY AS t (n, a);
          numbers  | n | a
        -----------+---+---
         [2, 5]    | 2 | 1
         [2, 5]    | 5 | 2
         [7, 8, 9] | 7 | 1
         [7, 8, 9] | 8 | 2
         [7, 8, 9] | 9 | 3
        (5 rows)
        ```

    -   **Joins**

        Joins allow you to combine data from multiple relations. A `CROSS JOIN` returns the [Cartesian product](https://en.wikipedia.org/wiki/Cartesian_product) of two relations \(all combinations\). `CROSS JOIN` can either be specified using

        -   the explicit `CROSS JOIN` syntax, or
        -   by specifying multiple relations in the `FROM` clause.
        Both of the following queries are equivalent:

        ```
        --- using the explicit **CROSS JOIN** syntax
        SELECT *
        FROM nation
        CROSS JOIN region;
        --- specifying multiple relations in the **FROM** clause
        VALUES
        FROM nation, region;
        ```

        Examples: The nation table contains 25 rows and the region table contains 5 rows, so a cross join between the two tables produces 125 rows:

        ```
        SELECT n.name AS nation, r.name AS region
        FROM nation AS n
        CROSS JOIN region AS r
        ORDER BY 1, 2;
             nation     |   region
        ----------------+-------------
         ALGERIA        | AFRICA
         ALGERIA        | AMERICA
         ALGERIA        | ASIA
         ALGERIA        | EUROPE
         ALGERIA        | MIDDLE EAST
         ARGENTINA      | AFRICA
         ARGENTINA      | AMERICA
        ...
        (125 rows)
        ```

        When two relations in a join have columns with the same name, the column references must be qualified using the relation name \(or alias\).

        ```
        --- Correct
        SELECT nation.name, region.name
        FROM nation
        CROSS JOIN region;
        --- Correct
        SELECT n.name, r.name
        FROM nation AS n
        CROSS JOIN region AS r;
        --- Correct
        SELECT n.name, r.name
        FROM nation n
        CROSS JOIN region r;
        --- Wrong, it will raise the "Column 'name' is ambiguous" error
        SELECT name
        FROM nation
        CROSS JOIN region;
        ```

    -   **Subquery**

        A subquery is an expression which is composed of a query. The subquery is correlated when it refers to columns outside of the subquery. Presto has limited support for correlated subqueries.

        -   **EXISTS**

            The `EXISTS` predicate determines if a subquery returns any rows. If subquery returns any rows, the WHERE expression is TRUE, and FALSE if otherwise.

            Examples

            ```
            SELECT name
            FROM nation
            WHERE EXISTS (SELECT * FROM region WHERE region.regionkey = nation.regionkey);
            ```

        -   **IN**

            The IN predicate determines if any columns specified by `WHERE` are included in the result set produced by the subquery. If yes, it returns results, and does not return results if otherwise. The subquery must produce exactly one column.

            Examples

            ```
            SELECT name
            FROM nation
            WHERE regionkey IN (SELECT regionkey FROM region);
            ```

        -   **Scalar subquery**

            A scalar subquery is a non-correlated subquery that returns zero or one row. The subquery cannot produce more than one row. The returned value is NULL if the subquery produces no rows.

            Examples

            ```
            SELECT name
            FROM nation
            WH ERregionkey = (SELECT max(regionkey) FROM regio;);
            ```


## SET SESSION {#section_cqh_l4g_1fb .section}

-   **Synopsis**

    ```
    SET SESSION name = expression
    SET SESSION catalog.name = expression
    ```

-   **Description**

    Sets a session property value.

-   **Examples**

    ```
    SET SESSION optimize_hash_generation = true;
    SET SESSION hive.optimized_reader_enabled = true;
    ```


## SHOW CATALOGS {#section_c3y_l4g_1fb .section}

-   **Synopsis**

    ```
    SHOW CATALOGS [ LIKE pattern ]
    ```

-   **Description**

    Lists the available catalogs. The `LIKE` clause can be used to filter the catalog names.

-   **Examples**

    ```
    SHOW CATALOGS;
    ```


## SHOW COLUMNS {#section_bq1_m4g_1fb .section}

-   **Synopsis**

    ```
    SHOW COLUMNS FROM table
    ```

-   **Description**

    Lists the columns in a given table along with their data type and other attributes.

-   **Examples**

    ```
    SHOW COLUMNS FROM orders;
    ```


## SHOW CREATE TABLE {#section_nnc_m4g_1fb .section}

-   **Synopsis**

    ```
    SHOW CREATE TABLE table_name
    ```

-   **Description**

    Shows the SQL statement that creates the specified table.

-   **Examples**

    ```
    SHOW CREATE TABLE sf1.orders;
    -----------------------------------------
     CREATE TABLE tpch.sf1.orders (
        orderkey bigint,
        orderstatus varchar,
        totalprice double,
        orderdate varchar
     )
     WITH (
        format = 'ORC',
        partitioned_by = ARRAY['orderdate']
     )
    (1 row)
    ```


## SHOW CREATE VIEW {#section_yn2_m4g_1fb .section}

-   **Synopsis**

    ```
    SHOW CREATE VIEW view_name
    ```

-   **Description**

    Shows the SQL statement that creates the specified view.

-   **Examples**

    ```
    SHOW CREATE VIEW view1;
    ```


## SHOW FUNCTIONS {#section_a2f_tpg_1fb .section}

-   **Synopsis**

    ```
    SHOW FUNCTIONS
    ```

-   **Description**

    List all the functions available for use in queries.

-   **Examples**

    ```
    SHOW FUNCTIONS
    ```


## SHOW GRANTS {#section_xk3_tpg_1fb .section}

-   **Synopsis**

    ```
    SHOW GRANTS [ ON [ TABLE ] table_name ]
    ```

-   **Description**

    Lists the grants for the current user on the specified table in the current catalog.

-   **Examples**

    ```
    --- List the grants for the current user on table orders
    SHOW GRANTS ON TABLE orders;
    --- List the grants for the current user on all the tables in all schemas of the current catalog
    SHOW GRANTS;
    ```

-   **Limitations**

    Some connectors have no support for `SHOW GRANTS`.


## SHOW SCHEMAS {#section_tny_dqg_1fb .section}

-   **Synopsis**

    ```
    SHOW SCHEMAS [ FROM catalog ] [ LIKE pattern ]
    ```

-   **Description**

    Lists all schemas in the specified catalog, or in the current catalog if no catalog has been specified. The `LIKE` clause can be used to filter the schema names.

-   **Examples**

    ```
    SHOW SCHEMAS;
    ```


## SHOW SESSION {#section_slk_lqg_1fb .section}

-   **Synopsis**

    ```
    SHOW SESSION
    ```

-   **Description**

    Lists the current session properties.

-   **Examples**

    ```
    SHOW SESSION
    ```


## SHOW TABLES; {#section_zlv_hsg_1fb .section}

-   **Synopsis**

    ```
    SHOW TABLES [ FROM schema ] [ LIKE pattern ]
    ```

-   **Description**

    Lists all tables in the specified schema, or in the current schema if no schema has been specified. The `LIKE` clause can be used to filter the table name.

-   **Examples**

    ```
    SHOW TABLES;
    ```


## START TRANSACTION {#section_nbw_lsg_1fb .section}

-   **Synopsis**

    ```
    START TRANSACTION [ mode [, ...] ]
    where **mode** is one of:
    ISOLATION LEVEL { READ UNCOMMITTED | READ COMMITTED | REPEATABLE READ | SERIALIZABLE }
    READ { ONLY | WRITE }
    ```

-   **Description**

    Starts a new transaction for the current session.

-   **Examples**

    ```
    START TRANSACTION;
    START TRANSACTION ISOLATION LEVEL REPEATABLE READ;
    START TRANSACTION READ WRITE;
    START TRANSACTION ISOLATION LEVEL READ COMMITTED, READ ONLY;
    START TRANSACTION READ WRITE, ISOLATION LEVEL SERIALIZABLE;
    ```


## USE {#section_kqx_4sg_1fb .section}

-   **Synopsis**

    ```
    USE catalog.schema
    USE schema
    ```

-   **Description**

    Updates the session to use the specified catalog and schema. If a catalog is not specified, the schema is resolved relative to the current catalog.

-   **Examples**

    ```
    USE hive.finance;
    USE information_schema;
    ```


## VALUES {#section_q5b_rsg_1fb .section}

-   **Synopsis**

    ```
    VALUES row [, ...]
    where **row** is a single expression or
    ( column_expression [, ...] )
    ```

-   **Description**

    Defines a literal inline table.

    -   `VALUE` can be used anywhere a query can be used. For example, behind the `FROM` clause of a `SELECT`, in an `INSERT`, or even at the top level.
    -   `VALUE` creates an anonymous table without column names by default. The table and columns can be named using an `AS` clause.
-   **Examples**

    ```
    --- Return a table with one column and three rows
    VALUES 1, 2, 3
    --- Return a table with two columns and three rows
    VALUES
        (1, 'a'),
        (2, 'b'),
        (3, 'c')
    --- Using in a query statement:
    SELECT * FROM (
        VALUES
            (1, 'a'),
            (2, 'b'),
            (3, 'c')
    ) AS t (id, name)
    --- Create a table
    CREATE TABLE example AS
    SELECT * FROM (
        VALUES
            (1, 'a'),
            (2, 'b'),
            (3, 'c')
    ) AS t (id, name)
    ```


