# MERGE INTO

This topic describes how to use the MERGE INTO statement in Spark Streaming SQL.

## Syntax

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

## Example

-   Define a source table and a target table.

    ```
    -- source table
    source_table
        : id int,         --primary key
        | name string,
        | opType string   --data operation type
    
    
    -- target table
    target_table
        : id int,         --primary key
        | name string
    ```

-   Merge data into a Delta table.

    ```
    MERGE INTO target_table t
    USING source_table s
    ON s.id = t.id
    WHEN MATCHED AND s.opType = 'delete' THEN DELETE
    WHEN MATCHED AND s.opType = 'update' THEN UPDATE SET id = s.id, name = s.name
    WHEN NOT MATCHED AND s.opType = 'insert' THEN INSERT (key, value) VALUES (key, value)
    ```

-   Merge data into a Kudu table.

    ```
    MERGE INTO target_table t
    USING source_table s
    WHEN s.opType = 'delete' THEN DELETE
    WHEN s.opType = 'update' THEN UPDATE SET *
    WHEN s.opType = 'insert' THEN INSERT *
    ```


## Description

Take note of the following rules for the MERGE INTO statement:

-   You can use a maximum of three clauses.
-   When you use the WHEN NOT MATCHED clause, you can specify a maximum of two WHEN clauses, and you must place the WHEN NOT MATCHED clause at the end of the statement.

Take note of the following rules for clauses in the MERGE INTO statement:

-   WHEN MATCHED:
    -   You can use only one UPDATE or DELETE operation.
    -   You can specify an optional condition in each WHEN MATCHED clause. If you use two WHEN MATCHED clauses, the first one must contain a condition.
    -   WHEN MATCHED clauses are executed in sequence. A data entry is updated or deleted based on only one WHEN MATCHED clause.
    -   You can use `UPDATE SET *` to indicate `UPDATE SET column1 = source.column1 [, column2 = source.column2 ...]`. If the number of fields in the source table is different from that in the target table, a parsing exception is thrown.
-   WHEN NOT MATCHED:
    -   You can use only INSERT operations and can specify an optional condition.
    -   You can use `INSERT *` to indicate `INSERT (column1 [, column2 ...]) VALUES (source.value1 [, source.value2 ...])`. If the number of fields in the source table is different from that in the target table, a parsing exception is thrown.

If the target storage system supports the UPSERT operation, you can use a variant of MERGE INTO to update or delete data. Take note of the following points on the variants of MERGE INTO:

-   You can use a maximum of three WHEN clauses.
-   `mergeCondition`: You do not need to configure a merge condition.
-   You can omit the `MATCHED` and `NOT MATCHED` keywords.

