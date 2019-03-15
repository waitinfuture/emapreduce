# SQL语句（旧文档，不发布） {#concept_k1k_mhl_z2b .concept}

本文介绍Presto使用中一些常见的SQL语句以及相关示例。

## ALTER SCHEMA {#section_fk2_2cf_1fb .section}

-   概要

    ```
    ALTER SCHEMA name RENAME TO new_name
    ```

-   描述

    重命名SCHEMA。

-   示例

    ```
    ALTER SCHEMA web RENAME TO traffic -- 将Schema 'web'重命名为'traffic'
    ```


## ALTER TABLE {#section_sjf_ncf_1fb .section}

-   概要

    ```
    ALTER TABLE name RENAME TO new_name
    ALTER TABLE name ADD COLUMN column_name data_type
    ALTER TABLE name DROP COLUMN column_name
    ALTER TABLE name RENAME COLUMN column_name TO new_column_name
    ```

-   描述

    更新表格定义。

-   示例

    ```
    ALTER TABLE users RENAME TO people; --- 重命名
    ALTER TABLE users ADD COLUMN zip varchar; --- 添加列
    ALTER TABLE users DROP COLUMN zip; --- 删除列
    ALTER TABLE users RENAME COLUMN id TO user_id; --- 重命名列
    ```


## CALL {#section_ph3_ncf_1fb .section}

-   概要

    ```
    CALL procedure_name ( [ name => ] expression [, ...] )
    ```

-   描述

    调用存储过程。存储过程可以由连接器提供，用于数据操作或管理任务。如果底层存储系统具有自己的存储过程框架，如PostgreSQL，则需要通过连接器提供的存储过程来访问这些存储系统自己的存储过程,而不能使用**CALL**直接访问。

-   示例

    ```
    CALL test(123, 'apple'); --- 调用存储过程并传参（参数位置）
    CALL test(name => 'apple', id => 123); --- 调用存储过程并传参（命名参数）
    CALL catalog.schema.test(); --- 调用权限定名称的存储过程
    ```


## COMMIT {#section_bq5_ddf_1fb .section}

-   概要

    ```
    COMMIT [WORK]
    ```

-   描述

    提交当前事务。

-   示例

    ```
    COMMIT;
    COMMIT WORK;
    ```


## CREATE SCHEMA {#section_pkx_ddf_1fb .section}

-   概要

    ```
    CREATE SCHEMA [ IF NOT EXISTS ] schema_name
    [ WITH ( property_name = expression [, ...] ) ]
    ```

-   描述

    创建新的SCHEMA。Schema是管理数据库表、视图和其他数据库对象的容器。

    -   如果SCHEMA已经存在，使用 `IF NOT EXISTS` 子句可以避免抛错。
    -   使用 `WITH` 子句可以在创建时为SCHEMA设置属性。可以通过下列查询语句获取所有支持的属性列表：

        ```
        SELECT * FROM system.metadata.schema_properties;
        ```

-   示例

    ```
    CREATE SCHEMA web;
    CREATE SCHEMA hive.sales;
    CREATE SCHEMA IF NOT EXISTS traffic;
    ```


## CREATE TABLE {#section_sgd_rdf_1fb .section}

-   概要

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

-   描述

    创建空表。可以使用`CREATE TABLE AS`从已有数据集中创建表。

    -   使用`IF NOT EXISTS`子句可以避免在表存在时抛出异常。
    -   使用`WITH`子句可以在创建表格时，为表格设置属性。支持的属性列表可以通过下列语句获取：

```
SELECT * FROM system.metadata.table_properties;
```

    -   使用`LIKE`子句可以引用已经存在的表的列定义。支持使用多个`LIKE`子句。
    -   使用`INCLUDING PROPERTIES`可以在创建表时，引用已有表的属性。如果同时还使用了`WITH`子句，`WITH`子句中指定的属性会覆盖`INCLUDING PROPERTIES`引入的同名属性。默认为`EXCLUDING PROPERTIES`。
-   示例

    ```
    --- 创建表orders
    CREATE TABLE orders (
      orderkey bigint,
      orderstatus varchar,
      totalprice double,
      orderdate date
    )
    WITH (format = 'ORC')
    --- 创建表orders，带备注信息
    CREATE TABLE IF NOT EXISTS orders (
      orderkey bigint,
      orderstatus varchar,
      totalprice double COMMENT 'Price in cents.',
      orderdate date
    )
    COMMENT 'A table to keep track of orders.'
    --- 创建表bigger_orders， 其中部分列的定义引用自表orders
    CREATE TABLE bigger_orders (
      another_orderkey bigint,
      LIKE orders,
      another_orderdate date
    )
    ```


## CREATE TABLE AS {#section_csn_22f_1fb .section}

-   概要

    ```
    CREATE TABLE [ IF NOT EXISTS ] table_name [ ( column_alias, ... ) ]
    [ COMMENT table_comment ]
    [ WITH ( property_name = expression [, ...] ) ]
    AS query
    [ WITH [ NO ] DATA ]
    ```

-   描述

    创建新表，表中的数据来自 `SELECT` 子句。

    -   使用 `IF NOT EXISTS` 子句可以避免在表存在时抛出异常；
    -   使用 `WITH` 子句可以在创建表格时，为表格设置属性。支持的属性列表可以通过下列语句获取：

        ```
        SELECT * FROM system.metadata.table_properties;
        ```

-   示例

    ```
    --- 从表orders中选择两个列来创建新的表
    CREATE TABLE orders_column_aliased (order_date, total_price)
    AS
    SELECT orderdate, totalprice
    FROM orders
    --- 创建新表，使用聚合函数
    CREATE TABLE orders_by_date
    COMMENT 'Summary of orders by date'
    WITH (format = 'ORC')
    AS
    SELECT orderdate, sum(totalprice) AS price
    FROM orders
    GROUP BY orderdate
    --- 创建新表，使用**IF NOT EXISTS**子句
    CREATE TABLE IF NOT EXISTS orders_by_date AS
    SELECT orderdate, sum(totalprice) AS price
    FROM orders
    GROUP BY orderdate
    --- 创建新表，schema和表nation一样，但是没有数据
    CREATE TABLE empty_nation AS
    SELECT *
    FROM nation
    WITH NO DATA
    ```


## CREATE VIEW {#section_ipq_22f_1fb .section}

-   概要

    ```
    CREATE [ OR REPLACE ] VIEW view_name AS query
    ```

-   描述

    创建视图。视图是一个逻辑表，不包含实际数据，可以在查询中被引用。每次查询引用视图时，定义视图的查询语句都会被执行一次。

    创建视图时使用 `OR REPLACE` 可以避免在视图存在时抛出异常。

-   示例

    ```
    --- 创建一个简单的视图
    CREATE VIEW test AS
    SELECT orderkey, orderstatus, totalprice / 2 AS half
    FROM orders
    --- 创建视图，使用聚合函数
    CREATE VIEW orders_by_date AS
    SELECT orderdate, sum(totalprice) AS price
    FROM orders
    GROUP BY orderdate
    --- 创建视图，如果视图已经存在，就替换它
    CREATE OR REPLACE VIEW test AS
    SELECT orderkey, orderstatus, totalprice / 4 AS quarter
    FROM orders
    ```


## DEALLOCATE PREPARE {#section_i1p_s2f_1fb .section}

-   概要

    ```
    DEALLOCATE PREPARE statement_name
    ```

-   描述

    从会话中删除一个命名Statement。

-   示例

    ```
    --- 释放名为my_query的查询
    DEALLOCATE PREPARE my_query;
    ```


## DELETE {#section_dqp_s2f_1fb .section}

-   概要

    ```
    DELETE FROM table_name [ WHERE condition ]
    ```

-   描述

    删除表中与 `WHERE` 子句匹配的行，如果没有指定 `WHERE` 子句，将删除所有行。

-   示例

    ```
    --- 删除匹配行
    DELETE FROM lineitem WHERE shipmode = 'AIR';
    --- 删除匹配行
    DELETE FROM lineitem
    WHERE orderkey IN (SELECT orderkey FROM orders WHERE priority = 'LOW');
    --- 清空表
    DELETE FROM orders;
    ```

-   局限

    部分连接器没有可能不支持 `DELETE`。


## DESCRIBE {#section_tjq_bff_1fb .section}

-   概要

    ```
    DESCRIBE table_name
    ```

-   描述

    获取表格定义信息，等同与[SHOW COLUMNS](#)。

-   示例

    ```
    DESCRIBE orders;
    ```


## DESCRIBE INPUT {#section_sxs_bff_1fb .section}

-   概要

    ```
    DESCRIBE INPUT statement_name
    ```

-   描述

    列出一个预编译查询中各个参数的位置及其类型。

-   示例

    ```
    --- 创建一个预编译查询'my_select1'
    PREPARE my_select1 FROM
    SELECT ? FROM nation WHERE regionkey = ? AND name < ?;
    --- 获取该预编译查询的描述信息
    DESCRIBE INPUT my_select1;
    ```

    查询结果：

    ```
    Position | Type
    --------------------
            0 | unknown
            1 | bigint
            2 | varchar
    (3 rows)
    ```


## DESCRIBE OUTPUT {#section_t2v_bff_1fb .section}

-   概要

    ```
    DESCRIBE OUTPUT statement_name
    ```

-   描述

    列出输出结果的所有列信息，包括列名（或别名）、Catalog、Schema、表名、类型、类型大小（单位字节）和一个标识位，说明该列是否为别名。

-   示例
    -   示例1

        准备一个预编译查询：

        ```
        PREPARE my_select1 FROM
        SELECT * FROM nation;
        ```

        执行 `DESCRIBE OUTPUT`，输出：

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

    -   示例2

        ```
        PREPARE my_select2 FROM
        SELECT count(*) as my_count, 1+2 FROM nation
        ```

        执行 `DESCRIBE OUTPUT`，输出：

        ```
        DESCRIBE OUTPUT my_select2;
         Column Name | Catalog | Schema | Table |  Type  | Type Size | Aliased
        -------------+---------+--------+-------+--------+-----------+---------
         my_count    |         |        |       | bigint |         8 | true
         _col1       |         |        |       | bigint |         8 | false
        (2 rows)
        ```

    -   示例3

        ```
        PREPARE my_create FROM
        CREATE TABLE foo AS SELECT * FROM nation;
        ```

        执行 `DESCRIBE OUTPUT`，输出：

        ```
        DESCRIBE OUTPUT my_create;
         Column Name | Catalog | Schema | Table |  Type  | Type Size | Aliased
        -------------+---------+--------+-------+--------+-----------+---------
         rows        |         |        |       | bigint |         8 | false
        (1 row)
        ```


## DROP SCHEMA {#section_twp_n4f_1fb .section}

-   概要

    ```
    DROP SCHEMA [ IF EXISTS ] schema_name
    ```

-   描述

    删除Schema。

    -   Schema必须为空。
    -   使用 `IF EXISTS` 来避免删除不存在的Schema时报错。
-   示例

    ```
    DROP SCHEMA web;
    DROP TABLE IF EXISTS sales;
    ```


## DROP TABLE {#section_brs_n4f_1fb .section}

-   概要

    ```
    DROP TABLE [ IF EXISTS ] table_name
    ```

-   描述

    删除数据表。使用**IF EXISTS**来避免删除不存在的表时报错。

-   示例

    ```
    DROP TABLE orders_by_date;
    DROP TABLE IF EXISTS orders_by_date;
    ```


## DROP VIEW {#section_wdt_n4f_1fb .section}

-   概要

    ```
    DROP VIEW [ IF EXISTS ] view_name
    ```

-   描述

    删除视图。使用**IF EXISTS**来避免删除不存在的视图时报错。

-   示例

    ```
    DROP VIEW orders_by_date;
    DROP VIEW IF EXISTS orders_by_date;
    ```


## EXECUTE {#section_zkg_v4f_1fb .section}

-   概要

    ```
    EXECUTE statement_name [ USING parameter1 [ , parameter2, ... ] ]
    ```

-   描述

    执行预编译查询， 使用**USING**子句按位置传参。

-   示例
    -   示例1

        ```
        PREPARE my_select1 FROM
        SELECT name FROM nation;
        --- 执行预编译查询
        EXECUTE my_select1;
        ```

    -   示例2

        ```
        PREPARE my_select2 FROM
        SELECT name FROM nation WHERE regionkey = ? and nationkey < ?;
        --- 执行预编译查询
        EXECUTE my_select2 USING 1, 3; 
        --- 上述语句等价与执行如下查询：
        SELECT name FROM nation WHERE regionkey = 1 AND nationkey < 3;
        ```


## EXPLAIN {#section_apj_v4f_1fb .section}

-   概要

    ```
    EXPLAIN [ ( option [, ...] ) ] statement
    这里Option可是时如下几个:
        FORMAT { TEXT | GRAPHVIZ }
        TYPE { LOGICAL | DISTRIBUTED | VALIDATE }
    ```

-   描述

    根据选项不同，可以实现如下几个功能：

    -   显示查询语句的逻辑计划。
    -   显示查询语句的分布式执行计划。
    -   验证一个查询语句的合法性。
    使用 `TYPE DISTRIBUTED` 选项来显示计划分片。每个计划分片都在单个或多个Presto节点上执行。分片之间的间隔表示Presto节点间的数据交换。分片类型说明分片是如何被Presto节点执行的，以及数据是如何在分片间分布的。 分片类型如下：

    -   SINGLE：分片在单个节点上执行。
    -   HASH：分片在固定个数的节点上执行，输入数据在这些节点上通过hash函数分布。
    -   ROUND\_ROBIN：分片在固定个数的节点上执行，输入数据在这些节点上通过ROUND\_ROBIN的模式分布。
    -   BROADCAST：分片在固定个数的节点上执行，处理数据通过广播的方式分发到所有节点。
    -   SOURCE：分片在存储数据的节点上执行。
-   示例
    -   示例1

        逻辑计划：

        ```
        presto:tiny> EXPLAIN SELECT regionkey, count(*) FROM nation GROUP BY 1;
                                                        Query Plan
        ----------------------------------------------------------------------------------------------------------
         - Output[regionkey, _col1] => [regionkey:bigint, count:bigint]
                 _col1 := count
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

    -   示例2

        分布式计划：

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

    -   示例3

        验证：

        ```
        presto:tiny> EXPLAIN (TYPE VALIDATE) SELECT regionkey, count(*) FROM nation GROUP BY 1;
         Valid
        -------
         true
        ```


## EXPLAIN ANALYZE {#section_lrl_v4f_1fb .section}

-   概要

    ```
    EXPLAIN ANALYZE [VERBOSE] statement
    ```

-   描述

    执行Statement，并且显示该次查询实际的分布式执行计划以及每个执行过程的成本。启用 `VERBOSE` 选项可以获取更多详细信息和底层的统计量。

-   示例

    在下面的示例中，你可以看到每个Stage消耗的CPU时间和该Stage中每个计划节点的相对成本。需要注意的是，相对成本使用实际时间来计量，因此，无法显示其与CPU时间的相关性。对于每个计划节点，还会显示一些附加的统计信息，这些统计信息有助于发现一次查询中的数据异常情况（如倾斜，hash冲突等）。

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

    如果开启了 `VERBOSE` 选项，运行操作符会返回更多信息：

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
                     Rows per driver: std.dev.: 0.00
                     Size of partition: std.dev.: 0.00
                     count := count("clerk")
     ...
    ```


## GRANT {#section_z3r_2qf_1fb .section}

-   概要

    ```
    GRANT ( privilege [, ...] | ( ALL PRIVILEGES ) )
    ON [ TABLE ] table_name TO ( grantee | PUBLIC )
    [ WITH GRANT OPTION ]
    ```

-   描述

    授予指定的权限指定的受让对象。

    -   `ALL PRIVILEGES` 包括 DELETE, INSERT 和 SELECT 权限。
    -   `PUBLIC`表示所有用户。
    -   使用`WITH GRANT OPTION`允许权限受让对象给其他用户赋予相同的权限。
-   示例

    ```
    GRANT INSERT, SELECT ON orders TO alice; --- 给用户alice赋权
    GRANT SELECT ON nation TO alice WITH GRANT OPTION; --- 给用户alice赋权，同时，alice还具有赋予其他用户**SELECT**权限的权限，
    GRANT SELECT ON orders TO PUBLIC; --- 开放表order的**SELECT**权限给所有人
    ```

-   局限

    某些连接器可能不支持 `GRANT`。


## INSERT {#section_c25_2qf_1fb .section}

-   概要

    ```
    INSERT INTO table_name [ ( column [, ... ] ) ] query
    ```

-   描述

    插入数据。如果指定了列名列表,该列表必须精确匹配 `query` 的结果集。没有出现在列名列表中的列将使用`null`进行填充。

-   示例

    ```
    INSERT INTO orders SELECT * FROM new_orders; --- 将select结果插入表orders中
    INSERT INTO cities VALUES (1, 'San Francisco'); --- 插入一行数据
    INSERT INTO cities VALUES (2, 'San Jose'), (3, 'Oakland'); --- 插入多行数据
    INSERT INTO nation (nationkey, name, regionkey, comment) VALUES (26, 'POLAND', 3, 'no comment'); --- 插入一行数据
    INSERT INTO nation (nationkey, name, regionkey) VALUES (26, 'POLAND', 3); --- 插入一行数据（只包括部分列）
    ```


## PREPARE {#section_tlw_2qf_1fb .section}

-   概要

    ```
    PREPARE statement_name FROM statement
    ```

-   描述

    创建一个预处理语句，以备后续使用。预处理语句就是一组保存在会话中的命名查询语句集合，可以在这些语句中定义参数，在实际执行时填充实际的值。定义的参数使用?占位。

-   示例

    ```
    --- 不带参数的预处理查询语句
    PREPARE my_select1 FROM
    SELECT * FROM nation;
    --- 带参数的预处理查询语句
    PREPARE my_select2 FROM
    SELECT name FROM nation WHERE regionkey = ? AND nationkey < ?;
    --- 不带参数的预处理插入语句
    PREPARE my_insert FROM
    INSERT INTO cities VALUES (1, 'San Francisco');
    ```


## RESET SESSION {#section_rky_2qf_1fb .section}

-   概要

    ```
    RESET SESSION name
    RESET SESSION catalog.name
    ```

-   描述

    重置会话，使用默认属性。

-   示例

    ```
    RESET SESSION optimize_hash_generation;
    RESET SESSION hive.optimized_reader_enabled;
    ```


## REVOKE {#section_nsv_fqf_1fb .section}

-   概要

    ```
    REVOKE [ GRANT OPTION FOR ]
    ( privilege [, ...] | ALL PRIVILEGES )
    ON [ TABLE ] table_name FROM ( grantee | PUBLIC )
    ```

-   描述

    撤回指定用户的指定权限。

    -   使用`ALL PRIVILEGE`可以撤销`SELECT`，`INSERT`和`DELETE`权限。
    -   使用`PUBLIC`表示撤销对象为`PUBLIC`角色，用户通过其他角色获得的权限不会被收回。
    -   使用`GRANT OPTION FOR`将进一步收回用户使用`GRANT`进行赋权的权限。
    -   语句中`grantee`既可以是单个用户也可以是角色。
-   示例

    ```
    --- 撤回用户alice对表orders进行INSET和SELECT的权限
    REVOKE INSERT, SELECT ON orders FROM alice;
    --- 撤回所有人对表nation进行SELECT的权限，
    --- 同时，撤回所有人赋予其他人对该表进行SELECT操作权限的权限
    REVOKE GRANT OPTION FOR SELECT ON nation FROM PUBLIC;
    --- 撤回用户alice在表test上的所有权限
    REVOKE ALL PRIVILEGES ON test FROM alice;
    ```

-   局限

    有些连接器不支持`REVOKE`语句。


## ROLLBACK {#section_jsl_lrf_1fb .section}

-   概要

    ```
    ROLLBACK [ WORK ] 
    ```

-   描述

    回滚当前事物。

-   示例

    ```
    ROLLBACK;
    ROLLBACK WORK;
    ```


## SELECT {#section_a2s_qrf_1fb .section}

-   概要

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

    其中，`from_item`有如下两种形式：

    ```
    table_name [ [ AS ] alias [ ( column_alias [, ...] ) ] ]
    ```

    ```
    from_item join_type from_item [ ON join_condition | USING ( join_column [, ...] ) ]
    ```

    这里的 `join_type` 可以是如下之一：

    -   \[ INNER \] JOIN
    -   LEFT \[ OUTER \] JOIN
    -   RIGHT \[ OUTER \] JOIN
    -   FULL \[ OUTER \] JOIN
    -   CROSS JOIN
    而 `grouping_element` 可以是如下之一：

    -   \(\)
    -   expression
    -   GROUPING SETS \( \( column \[, …\] \) \[, …\] \)
    -   CUBE \( column \[, …\] \)
    -   ROLLUP \( column \[, …\] \)
-   描述

    检索0到多张数据表，获取结果集。

-   WITH子句
    -   基本功能

        WITH子句可以定义一组命名关系，这些关系可以在查询中被使用，从而扁平化嵌套查询或简化子查询。如下两个查询语句是等价的：

        ```
        --- 不使用WITH子句
        SELECT a, b
        FROM (
          SELECT a, MAX(b) AS b FROM t GROUP BY a
        ) AS x;
        --- 使用WITH子句，查询语句看起来更加明了
        WITH x AS (SELECT a, MAX(b) AS b FROM t GROUP BY a)
        SELECT a, b FROM x;
        ```

    -   定义多个子查询

        WITH 子句中可以定义多个子查询：

        ```
        WITH
          t1 AS (SELECT a, MAX(b) AS b FROM x GROUP BY a),
          t2 AS (SELECT a, AVG(d) AS d FROM y GROUP BY a)
        SELECT t1.*, t2.*
        FROM t1
        JOIN t2 ON t1.a = t2.a;
        ```

    -   组成链式结构

        WITH 子句中的关系还可以组成链式结构：

        ```
        WITH
          x AS (SELECT a FROM t),
          y AS (SELECT a AS b FROM x),
          z AS (SELECT b AS c FROM y)
        SELECT c FROM z;
        ```

-   GROUP BY子句
    -   基本功能

        使用 `GROUP BY` 子句可以对 `SELECT` 结果进行分组。`GROUP BY`子句支持任意的表达式，既可以使用列名，也可以使用序号（从1开始）。

        下列示例中的查询语句是等价的（列 `nationkey` 的位置为2）。

        ```
        --- 使用列序号
        SELECT count(*), nationkey FROM customer GROUP BY 2;
        --- 使用列名
        SELECT count(*), nationkey FROM customer GROUP BY nationkey;
        ```

        没有在输出列表中指定的列也可以用于 `GROUP BY` 子句，如下所示：

        ```
        --- 列mktsegment没有在SELECT列表中指定，
        --- 结果集中不包括mktsegment列的内容。
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

        **说明：** 在`SELECT`语句中使用`GROUP BY`子句时，其输出表达式只能是聚合函数或者是`GROUP BY`子句中使用的列。

    -   复杂分组操作

        Presto支持如下3中复杂的聚合语法，可以在一个查询中实现多个列集合的聚合分析：

        -   GROUPING SETS（分组集）

            `GROUPING SETS`可以在一条查询语句中完成多个列的分组聚合。没有在分组列表中的列使用`NULL`进行填充。

            表shipping是一个包含5个列的数据表，如下所示：

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

            现在希望在一个查询中获取如下几个分组结果：

            -   按origin\_state进行分组，获取package\_weight的总和。
            -   按origin\_state和origin\_zip分组，获取package\_weight的总和。
            -   按destination\_state分组，获取package\_weight的总和。
            使用`GROUPING SETS`可以在一条语句中获取上述3个分组的结果集，如下所示：

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

            上述查询逻辑也可以通过`UNION ALL`多个`GROUP BY`查询实现：

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

            但是，使用`GROUPING SETS`语法往往能获得更好的性能，因为，该语法在执行时，只会读取一次基表数据，而使用上述`UNION ALL`方式，会读取3次，因此，如果在查询期间基表数据有变化，使用`UNION ALL`的方式容易出现不一致的结果。

        -   CUBE（多维立方）

            使用CUBE可以获得给定列列表所有可能的分组结果。如下所示：

            ```
            SELECT origin_state, destination_state, sum(package_weight)
            FROM shipping
            GROUP BY CUBE (origin_state, destination_state);
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

            该查询等价于如下语句：

            ```
            SELECT origin_state, destination_state, sum(package_weight)
            FROM shipping
            GROUP BY GROUPING SETS (
                (origin_state, destination_state),
                (origin_state),
                (destination_state),
                ());
            ```

        -   ROLLUP（汇总）

            使用ROLLUP可以获得给定列集和的小记结果。如下所示：

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

            上述示例等价与如下语句：

            ```
            SELECT origin_state, origin_zip, sum(package_weight)
            FROM shipping
            GROUP BY GROUPING SETS ((origin_state, origin_zip), (origin_state), ());
            ```

        -   综合使用

            下列3个语句是等价的：

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

            输出结果如下：

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

            在`GROUP BY`子句中，可以使用`ALL`和`DISTINCT`修饰符，用来说明是否可以生成重复的统计维度。如下所示：

            ```
            SELECT origin_state, destination_state, origin_zip, sum(package_weight)
            FROM shipping
            GROUP BY ALL
                CUBE (origin_state, destination_state),
                ROLLUP (origin_state, origin_zip);
            ```

            等价于：

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

            其中有多个维度是重复的。如果使用`DISTINCT`，则只会输出不重复的维度。

            ```
            SELECT origin_state, destination_state, origin_zip, sum(package_weight)
            FROM shipping
            GROUP BY DISTINCT
                CUBE (origin_state, destination_state),
                ROLLUP (origin_state, origin_zip);
            ```

            等价于：

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

            **说明：** `GROUP BY`默认使用的修饰符为`ALL`。

    -   GROUPING函数

        Presto提供了一个`grouping`函数，用于生成一个标记数，该数中的每一个比特位表示对应的列是否出现在该分组条件中。语法如下：

        ```
        grouping(col1, ..., colN) -> bigint
        ```

        `grouping`通常与`GROUPING SETS`，`ROLLUP`，`CUBE`或`GROUP BY`一起使用。`grouping`中的列必须与`GROUPING SETS`，`ROLLUP`，`CUBE`或`GROUP BY`中指定的列一一对应。

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

        如上所示，`grouping`函数返回的是右对齐的比特标记数，0 表示列存在，1 表示列不存在。

-   HAVING子句

    `HAVING`子句用来控制查询中分组的选择，与聚合函数和`GROUP BY`子句一起使用。`HAVING`子句的功能会在分组和聚合计算完成后进行，过滤掉不满足条件的分组。

    下面的示例将账户结余大于5700000的用户选出来。

    ```
    SELECT count(*), mktsegment, nationkey,
           CAST(sum(acctbal) AS bigint) AS totalbal
    FROM customer
    GROUP BY mktsegment, nationkey
    HAVING sum(acctbal) > 5700000
    ORDER BY totalbal DESC;
    ```

    输出如下：

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

-   集合运算

    Presto支持`UNION`、`INTERSECT`和`EXCEPT`3种集合运算。这些子句用来将多个查询语句的结果合并成一个总的结果集。使用方法如下所示：

    ```
    query UNION [ALL | DISTINCT] query
    query INTERSECT [DISTINCT] query
    query EXCEPT [DISTINCT] query
    ```

    参数ALL和DISTINCT来控制最终哪些行会出现在结果集中，默认设为DISTINCT。

    -   ALL， 有可能返回重复的行。
    -   DISTINCT，对结果集进行去重。
    `INTERSECT`和`EXCEPT`不支持ALL选项。

    上述3个集合运算可以组合使用，运算从左到右进行，`INTERSECT`的优先级最高，因此，组合运算`A UNION B INTERSECT C EXCEPT D`实际的顺序是`A UNION (B INTERSECT C) EXCEPT D`。

-   UNION

    `UNION`对两个查询的结果集做并集运算，通过ALL和DISTINCT来控制是否剔除重复项。

    -   示例1

        ```
        SELECT 13
        UNION
        SELECT 42;
         _col0
        -------
            13
            42
        (2 rows)
        ```

    -   示例2

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

    -   示例3

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

    `INTERSECT`对两个查询的结果集做交集运算。

    示例：

    ```
    SELECT * FROM (VALUES 13, 42)
    INTERSECT
    SELECT 13;
     _col0
    -------
       13
    (1 rows)
    ```

-   EXCEPT

    `EXCEPT`求两个查询结果集的补集。

    ```
    SELECT * FROM (VALUES 13, 42)
    EXCEPT
    SELECT 13;
     _col0
    -------
       42
    (1 rows)
    ```

-   ORDER BY子句

    在查询中可以使用`ORDER BY`子句对查询结果进行排序。语法如下：

    ```
    ORDER BY expression [ ASC | DESC ] [ NULLS { FIRST | LAST } ] [, ...]
    ```

    其中：

    -   expression由列名或列位置序号（从1开始）组成。
    -   `ORDER BY`在`GROUP BY`和`HAVING`之后执行。
    -   NULLS \{ FIRST | LAST \}可以控制NULL值的排序方式（与ASC/DESC无关），默认为LAST。
-   LIMIT子句

    `LIMIT`子句用来控制结果集的规模（即行数）。使用`LIMIT ALL`等价于不使用`LIMIT`子句。

    示例：

    ```
    --- 本例中，因为没有使用ORDER BY子句，返回结果是随机的
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

-   TABLESAMPLE

    Presto提供了两种采样方式，BERNOULLI和SYSTEM，但是无论哪种采样方式， 都无法确定的给出采样结果集的行数。

    -   BERNOULLI：

        本方法基于样本百分比概率进行采样。当使用Bernoulli方法对表格进行采样时，执行器会扫描表格的所有物理块，并且基于样本百分比与运行时计算的随机值之间的比较结果跳过某些行。

        每一行被选中对概率是独立对，与其他行无关，但是，这不会减少从磁盘读取采样表所需的时间。如果还需对采样结果做进一步处理，可能会对总查询时间产生影响。

    -   SYSTEM：

        本方法将表格分成数据的逻辑段，并以此粒度对表格进行采样。这种抽样方法要么从一个特定的数据段中选择所有的行，要么跳过它（根据样本百分比和在运行时计算的一个随机值之间的比较）。

        采样结果与使用哪种连接器有关。例如，与Hive一起使用时，取决于数据在HDFS上的分布。这种方法不保证独立的抽样概率。

    示例：

    ```
    --- 使用BERNOULLI采样
    SELECT *
    FROM users TABLESAMPLE BERNOULLI (50);
    --- 使用SYSTEM采样
    SELECT *
    FROM users TABLESAMPLE SYSTEM (75);
    Using sampling with joins:
    --- 采样过程中使用JOIN
    SELECT o.*, i.*
    FROM orders o TABLESAMPLE SYSTEM (10)
    JOIN lineitem i TABLESAMPLE BERNOULLI (40)
      ON o.orderkey = i.orderkey;
    ```

-   UNNEST

    `UNNEST`可以将 ARRAY 和 MAP 类型的变量展开成表。其中 ARRAY 展开为单列的表， MAP 展开为双列的表（键，值）。`UNNEST`可以一次展开多个 ARRAY 和 MAP 类型的变量，在这种情况下，它们被扩展为多个列，行数等于输入参数列表中最大展开行数（其他列用空值填充）。`UNNEST`可以有一个`WITH ORDINALITY`子句，此时，查询结果中会附加一个序数列。`UNNEST`通常与`JOIN`一起使用，并可以引用join左侧的关系的列。

    -   示例1

        ```
        --- 使用一个列
        SELECT student, score
        FROM tests
        CROSS JOIN UNNEST(scores) AS t (score);
        ```

    -   示例2

        ```
        --- 使用多个列
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

    -   示例3

        ```
        --- 使用 WITH ORDINALITY 子句
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

        Joins可以将多个关系的数据合并到一起。`CROSS JOIN` 返回两个关系到[笛卡尔积](https://en.wikipedia.org/wiki/Cartesian_product)（所有排列组合的集合）。`CROSS JOIN`可以通过如下两种方式来指定：

        -   显式的使用`CROSS JOIN`语法。
        -   在`FROM`子句中指定多个关系。
        下列两个SQL语句是等价的：

        ```
        --- 显式的使用**CROSS JOIN**语法
        SELECT *
        FROM nation
        CROSS JOIN region;
        --- 在**FROM**子句中指定多个关系
        SELECT *
        FROM nation, region;
        ```

        示例：表nation包括25行记录，表region包括5行，对这两个表执行cross join操作，其结果集为125行记录。

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

        如果参与join的表中包含相同名称的列，则需要用表名（或别名）加以修饰，示例如下：

        ```
        --- 正确
        SELECT nation.name, region.name
        FROM nation
        CROSS JOIN region;
        --- 正确
        SELECT n.name, r.name
        FROM nation AS n
        CROSS JOIN region AS r;
        --- 正确
        SELECT n.name, r.name
        FROM nation n
        CROSS JOIN region r;
        --- 错误，会抛出"Column 'name' is ambiguous"
        SELECT name
        FROM nation
        CROSS JOIN region;
        ```

    -   子查询

        子查询是由一个查询组成的表达式。当子查询引用了子查询之外的列时，子查询与外部查询是相关的。Presto对相关子查询的支持比较有限。

        -   EXISTS

            谓语`EXISTS`用于确定子查询是否返回所有行，如果子查询有返回值，WHERE表达式为TRUE，否则为FALSE。

            示例：

            ```
            SELECT name
            FROM nation
            WHERE EXISTS (SELECT * FROM region WHERE region.regionkey = nation.regionkey);
            ```

        -   IN

            如果`WHERE`指定的列在子查询结果集中存在，则返回结果，否则不返回。子查询只能返回一个列。

            示例：

            ```
            SELECT name
            FROM nation
            WHERE regionkey IN (SELECT regionkey FROM region);
            ```

        -   标量子查询

            标量子查询是一个非相关子查询，返回0～1行结果。该类子查询最多只能返回一行数据，如果子查询结果集为空，则返回 null。

            示例：

            ```
            SELECT name
            FROM nation
            WH ERregionkey = (SELECT max(regionkey) FROM regio;);
            ```


## SET SESSION {#section_cqh_l4g_1fb .section}

-   概要

    ```
    SET SESSION name = expression
    SET SESSION catalog.name = expression
    ```

-   描述

    设置会话属性。

-   示例

    ```
    SET SESSION optimize_hash_generation = true;
    SET SESSION hive.optimized_reader_enabled = true;
    ```


## SHOW CATALOGS {#section_c3y_l4g_1fb .section}

-   概要

    ```
    SHOW CATALOGS [ LIKE pattern ]
    ```

-   描述

    获取可用的Catalog清单。可以使用`LIKE`子句过滤catalog名称。

-   示例

    ```
    SHOW CATALOGS;
    ```


## SHOW COLUMNS {#section_bq1_m4g_1fb .section}

-   概要

    ```
    SHOW COLUMNS FROM table
    ```

-   描述

    获取给定表格所有的列及其属性。

-   示例

    ```
    SHOW COLUMNS FROM orders;
    ```


## SHOW CREATE TABLE {#section_nnc_m4g_1fb .section}

-   概要

    ```
    SHOW CREATE TABLE table_name
    ```

-   描述

    显示定义给定表格的SQL语句。

-   示例

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

-   概要

    ```
    SHOW CREATE VIEW view_name
    ```

-   描述

    显示定义给定视图的SQL语句。

-   示例

    ```
    SHOW CREATE VIEW view1;
    ```


## SHOW FUNCTIONS {#section_a2f_tpg_1fb .section}

-   概要

    ```
    SHOW FUNCTIONS
    ```

-   描述

    列出所有可用于用于查询的函数。

-   示例

    ```
    SHOW FUNCTIONS
    ```


## SHOW GRANTS {#section_xk3_tpg_1fb .section}

-   概要

    ```
    SHOW GRANTS [ ON [ TABLE ] table_name ]
    ```

-   描述

    显示权限列表。

-   示例

    ```
    --- 获取当前用户在表orders上的权限
    SHOW GRANTS ON TABLE orders;
    --- 获取当前用户在当前catalog中的权限
    SHOW GRANTS;
    ```

-   局限

    有些连接器不支持`SHOW GRANTS`操作。


## SHOW SCHEMAS {#section_tny_dqg_1fb .section}

-   概要

    ```
    SHOW SCHEMAS [ FROM catalog ] [ LIKE pattern ]
    ```

-   描述

    列出给定catalog下的所有schema，catalog不指定则表示当前catalog。使用`LIKE`子句可以过滤schema名称。

-   示例

    ```
    SHOW SCHEMAS;
    ```


## SHOW SESSION {#section_slk_lqg_1fb .section}

-   概要

    ```
    SHOW SESSION
    ```

-   描述

    显示当前回话的属性列表。

-   示例

    ```
    SHOW SESSION
    ```


## SHOW TABLES {#section_zlv_hsg_1fb .section}

-   概要

    ```
    SHOW TABLES [ FROM schema ] [ LIKE pattern ]
    ```

-   描述

    列出给定Schema下的所有表格，schema不指定则表示当前schema。使用`LIKE`子句可以过滤表名。

-   示例

    ```
    SHOW TABLES;
    ```


## START TRANSACTION {#section_nbw_lsg_1fb .section}

-   概要

    ```
    START TRANSACTION [ mode [, ...] ]
    其中**mode**可以从如下几个选项中选择：
    ISOLATION LEVEL { READ UNCOMMITTED | READ COMMITTED | REPEATABLE READ | SERIALIZABLE }
    READ { ONLY | WRITE }
    ```

-   描述

    在当前会话中启动一个新的事务。

-   示例

    ```
    START TRANSACTION;
    START TRANSACTION ISOLATION LEVEL REPEATABLE READ;
    START TRANSACTION READ WRITE;
    START TRANSACTION ISOLATION LEVEL READ COMMITTED, READ ONLY;
    START TRANSACTION READ WRITE, ISOLATION LEVEL SERIALIZABLE;
    ```


## USE {#section_kqx_4sg_1fb .section}

-   概要

    ```
    USE catalog.schema
    USE schema
    ```

-   描述

    更新当前回话，使用指定的Catalog和Schema。如果Catalog没有指定，则使用当前Catalog下的Schema。

-   示例

    ```
    USE hive.finance;
    USE information_schema;
    ```


## VALUES {#section_q5b_rsg_1fb .section}

-   概要

    ```
    VALUES row [, ...]
    其中， **row**是一个表达式，或者如下形式的表达式列表：
    ( column_expression [, ...] )
    ```

-   描述

    定义一张字面内联表。

    -   任何可以使用查询语句的地方都可以使用`VALUE`， 如放在`SELECT`语句的`FROM`后面，放在`INSERT`里，甚至放在最顶层。
    -   `VALUE`默认创建一张匿名表，并且没有列名。表名和列名可以通过`AS`进行命名。
-   示例

    ```
    --- 返回一个表对象，包含1列，3行数据
    VALUES 1, 2, 3
    --- 返回一个表对象，包含2列，3行数据
    VALUES
        (1, 'a'),
        (2, 'b'),
        (3, 'c')
    --- 在查询语句中使用
    SELECT * FROM (
        VALUES
            (1, 'a'),
            (2, 'b'),
            (3, 'c')
    ) AS t (id, name)
    --- 创建一个表
    CREATE TABLE example AS
    SELECT * FROM (
        VALUES
            (1, 'a'),
            (2, 'b'),
            (3, 'c')
    ) AS t (id, name)
    ```


