# PREPARE {#concept_rrk_qck_zgb .concept}

## Overview {#section_p5k_ll3_ygb .section}

```
PREPARE statement_name FROM statement
```

## Description {#section_fym_ml3_ygb .section}

Creates a precompiled statement for subsequent executions. A precompiled statement is a set of query statements saved to a session. The statement can include arguments that are used to enter actual values during execution. Arguments are represented by `?` .

## Examples {#section_phk_nl3_ygb .section}

```
--- Prepare a query that does not include arguments
PREPARE my_select1 FROM
SELECT * FROM nation;

--- Prepare a query that includes arguments
PREPARE my_select2 FROM
SELECT name FROM nation WHERE regionkey = ? AND nationkey < ? ;

--- Prepare an INSERT statement that does not include arguments
PREPARE my_insert FROM
INSERT INTO cities VALUES (1, 'San Francisco');
```

