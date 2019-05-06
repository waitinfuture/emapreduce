# START TRANSACTION {#concept_v2l_vhm_zgb .concept}

## Overview {#section_p5k_ll3_ygb .section}

```
START TRANSACTION [ mode [, ...] ]

`mode` provides the following options:

ISOLATION LEVEL { READ UNCOMMITTED | READ COMMITTED | REPEATABLE READ | SERIALIZABLE }
READ { ONLY | WRITE }
```

## Description {#section_fym_ml3_ygb .section}

Starts a new transaction for the current session.

## Examples {#section_phk_nl3_ygb .section}

```
START TRANSACTION;
START TRANSACTION ISOLATION LEVEL REPEATABLE READ;
START TRANSACTION READ WRITE;
START TRANSACTION ISOLATION LEVEL READ COMMITTED, READ ONLY;
START TRANSACTION READ WRITE, ISOLATION LEVEL SERIALIZABLE;
```

