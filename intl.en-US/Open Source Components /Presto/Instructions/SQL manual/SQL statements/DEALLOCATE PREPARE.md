# DEALLOCATE PREPARE {#concept_gzh_1s3_ygb .concept}

## Overview {#section_p5k_ll3_ygb .section}

```
DEALLOCATE PREPARE statement_name
```

## Description {#section_fym_ml3_ygb .section}

Removes a named statement from a session.

## Examples {#section_phk_nl3_ygb .section}

```
--- Release the query named my_query
DEALLOCATE PREPARE my_query;
```

