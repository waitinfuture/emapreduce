# MERGE INTO

本文为您介绍如何在Spark SQL流式处理中使用MERGE INTO语句。

## 语法

```
mergeInto
    : MERGE INTO target=tableIdentifier tableAlias
      USING (source=tableIdentifier (timeTravel)? | '(' subquery = query ')') tableAlias
      mergeCondition?
      matchedClauses*
      notMatchedClause?

mergeCondition
    : ON condition=booleanExpression

matchedClauses
    : deleteClause
    | updateClause

notMatchedClause
    : insertClause

deleteClause
    : WHEN MATCHED (AND deleteCond=booleanExpression)? THEN deleteAction
    | WHEN deleteCond=booleanExpression THEN deleteAction

updateClause
    : WHEN MATCHED (AND updateCond=booleanExpression)? THEN updateAction
    | WHEN updateCond=booleanExpression THEN updateAction

insertClause
    : WHEN NOT MATCHED (AND insertCond=booleanExpression)? THEN insertAction
    | WHEN insertCond=booleanExpression THEN insertAction
```

## 示例

-   源表和目标表定义。

    ```
    -- source table
    source_table
        : id int,         --主键
        | name string,
        | opType string   --数据操作类型
    
    
    -- target table
    target_table
        : id int,         --主键
        | name string
    ```

-   Merge Into Delta。

    ```
    MERGE INTO target_table t
    USING source_table s
    ON s.id = t.id
    WHEN MATCHED AND s.opType = 'delete' THEN DELETE
    WHEN MATCHED AND s.opType = 'update' THEN UPDATE SET id = s.id, name = s.name
    WHEN NOT MATCHED AND s.opType = 'insert' THEN INSERT (key, value) VALUES (key, value)
    ```

-   Merge Into Kudu。

    ```
    MERGE INTO target_table t
    USING source_table s
    WHEN s.opType = 'delete' THEN DELETE
    WHEN s.opType = 'update' THEN UPDATE SET *
    WHEN s.opType = 'insert' THEN INSERT *
    ```


## 说明

MERGE INTO语句需要遵循以下规则：

-   最多支持3个子句。
-   当支持WHEN NOT MATCHED子句时，最多支持2个WHEN子句，并且WHEN NOT MATCHED子句必须放在最后。

MERGE INTO的子句需要遵循以下规则：

-   WHEN MATCHED子句：
    -   最多有一个UPDATE或者DELETE操作。
    -   每个子句可以有一个可选的condition。如果有两个WHEN MATCHED子句时，则第一个WHEN MATCHED必须写上condition。
    -   WHEN MATCHED子句有顺序关系，且每条数据只能执行一个WHEN MATCHED子句。
    -   支持`UPDATE SET *`写法，表示`UPDATE SET column1 = source.column1 [, column2 = source.column2 ...]`。如果源表和目标表的字段个数不一致，则抛出解析失败异常。
-   WHEN NOT MATCHED子句：
    -   仅能使用INSERT操作，且可以有一个可选的condition。
    -   支持`INSERT *`写法，表示`INSERT (column1 [, column2 ...]) VALUES (source.value1 [, source.value2 ...])`。如果源表和目标表的字段个数不一致，则抛出解析失败异常。

对于支持UPSERT操作的目标存储系统，可以基于MERGE的变种来实现。支持情况如下：

-   最多支持3个WHEN子句。
-   `mergeCondition`：无需配置Merge条件。
-   可以省略`MATCHED`和`NOT MATCHED`关键字，直接根据condition操作。

