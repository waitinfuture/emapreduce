# Process Table Store data in Hive {#concept_sqb_xrn_hfb .concept}

This topic describes how to process Table Store data in Hive.

## Connect Hive to Table Store {#section_zzs_fsn_hfb .section}

-   Prepare a data table

    Create a table named pet and set the name field as the primary key.

    |name|owner|species|sex|birth|death|
    |----|-----|-------|---|-----|-----|
    |Fluffy|Harold|cat|f|1993-02-04| |
    |Claws|Gwen|cat|m|1994-03-17| |
    |Buffy|Harold|dog|f|1989-05-13| |
    |Fang|Benny|dog|m|1990-08-27| |
    |Bowser|Diane|dog|m|1979-08-31|1995-07-29|
    |Chirpy|Gwen|bird|f|1998-09-11| |
    |Whistler|Gwen|bird| |1997-12-09| |
    |Slim|Benny|snake|m|1996-04-29| |
    |Puffball|Diane|hamster|f|1999-03-30| |

-   The following example describes how to process Table Store data in Hive.
    1.  Command line

        ```
        $ HADOOP_HOME=YourHadoopDir HADOOP_CLASSPATH=emr-tablestore-<version>.jar:tablestore-4.1.0-jar-with-dependencies.jar:joda-time-2.9.4.jar bin/hive
        ```

    2.  Create an external table

        ```
        CREATE EXTERNAL TABLE pet
          (name STRING, owner STRING, species STRING, sex STRING, birth STRING, death STRING)
          STORED BY 'com.aliyun.openservices.tablestore.hive.TableStoreStorageHandler'
          WITH SERDEPROPERTIES(
            "tablestore.columns.mapping"="name,owner,species,sex,birth,death")
          TBLPROPERTIES (
            "tablestore.endpoint"="YourEndpoint",
            "tablestore.access_key_id"="YourAccessKeyId",
            "tablestore.access_key_secret"="YourAccessKeySecret",
            "tablestore.table.name"="pet");
        ```

    3.  Insert data to the external table

        ```
        INSERT INTO pet VALUES("Fluffy", "Harold", "cat", "f", "1993-02-04", null);
        INSERT INTO pet VALUES("Claws", "Gwen", "cat", "m", "1994-03-17", null);
        INSERT INTO pet VALUES("Buffy", "Harold", "dog", "f", "1989-05-13", null);
        INSERT INTO pet VALUES("Fang", "Benny", "dog", "m", "1990-08-27", null);
        INSERT INTO pet VALUES("Bowser", "Diane", "dog", "m", "1979-08-31", "1995-07-29");
        INSERT INTO pet VALUES("Chirpy", "Gwen", "bird", "f", "1998-09-11", null);
        INSERT INTO pet VALUES("Whistler", "Gwen", "bird", null, "1997-12-09", null);
        INSERT INTO pet VALUES("Slim", "Benny", "snake", "m", "1996-04-29", null);
        INSERT INTO pet VALUES("Puffball", "Diane", "hamster", "f", "1999-03-30", null);
        ```

    4.  Query data

        ```
        > SELECT * FROM pet;
        Bowser  Diane   dog     m       1979-08-31      1995-07-29
        Buffy   Harold  dog     f       1989-05-13      NULL
        Chirpy  Gwen    bird    f       1998-09-11      NULL
        Claws   Gwen    cat     m       1994-03-17      NULL
        Fang    Benny   dog     m       1990-08-27      NULL
        Fluffy  Harold  cat     f       1993-02-04      NULL
        Puffball        Diane   hamster f       1999-03-30      NULL
        Slim    Benny   snake   m       1996-04-29      NULL
        Whistler        Gwen    bird    NULL    1997-12-09      NULL
        > SELECT * FROM pet WHERE birth > "1995-01-01";
        Chirpy  Gwen    bird    f       1998-09-11      NULL
        Puffball        Diane   hamster f       1999-03-30      NULL
        Slim    Benny   snake   m       1996-04-29      NULL
        Whistler        Gwen    bird    NULL    1997-12-09      NULL
        ```


## Data type conversion {#section_gn4_5tn_hfb .section}

| |TINYINT|SMALLINT|INT|BIGINT|FLOAT|DOUBLE|BOOLEAN|STRING|BINARY|
|--|-------|--------|---|------|-----|------|-------|------|------|
|INTEGER|Supported \(with loss of precision\)|Supported \(with loss of precision\)|Supported \(with loss of precision\)|Supported|Supported \(with loss of precision\)|Supported \(with loss of precision\)| | | |
|DOUBLE|Supported \(with loss of precision\)|Supported \(with loss of precision\)|Supported \(with loss of precision\)|Supported \(with loss of precision\)|Supported \(with loss of precision\)|Supported| | | |
|BOOLEAN| | | | | | |Supported| | |
|STRING| | | | | | | |Supported| |
|BINARY| | | | | | | | |Supported|

## Appendix {#section_ezh_dvn_hfb .section}

For the complete sample code, see

-   [Process Table Store data in Hive](https://github.com/aliyun/aliyun-emapreduce-sdk/blob/master/examples/src/main/java/com/aliyun/openservices/tablestore/pet.sql)

