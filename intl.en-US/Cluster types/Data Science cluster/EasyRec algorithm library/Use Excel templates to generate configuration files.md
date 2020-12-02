# Use Excel templates to generate configuration files

This topic describes how to use Excel templates to generate configuration files.

## Command

Excel templates:

-   MultiTower: [multi\_tower\_template.xls](https://pai-vision-data-hz.oss-cn-zhangjiakou.aliyuncs.com/easy-rec/excel/multi_tower_template.xls)
-   DeepFM: [deepfm\_template.xls](https://pai-vision-data-hz.oss-cn-zhangjiakou.aliyuncs.com/easy-rec/excel/deepfm_template.xls)
-   wide\_and\_deep: similar to DeepFM.

```
# bash
pip install pandas
wget https://pai-vision-data-hz.oss-cn-zhangjiakou.aliyuncs.com/easy-rec/releases/scripts/create_config_from_excel.py
python scripts/create_config_from_excel.py --model_type multi_tower --excel_path multi_tower_template.xls --output_path multi_tower.config
```

|Parameter|Description|
|---------|-----------|
|`model_type`|This parameter must match an Excel template.|
|`excel_path`|The path used to store a configuration file.|
|`output_path`|The path used to store input and output files. Valid values: train\_input\_path TRAIN\_INPUT\_PATH and eval\_input\_path EVAL\_INPUT\_PATH.By default, columns in a CSV data file are separated by commas \(,\). Characters in a column are separated by vertical bars \(\|\).

You can also customize a separator in the format of `--column_separator $'|' --incol_separator $','`. |

## Configuration description

A template contains five sheets: features, global, group, types, and basic\_types.

-   features: describes the feature configurations.

    ```
    | **name** | **data_type** | **type** | **user_item_other** | **global** | **hash_bucket_size** | **embedding_dim** | **default_value** | **weights** | **boundaries** | **query** |
    | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
    | label | double | label | label |  |  |  |  |  |  |  |
    | uid |  string  | category | user | uid |  |  |  |  |  |  |
    | own_room |  bigint  | dense | user |  |  |  |  |  | 10,20,30 |  |
    | cate_idx |  string  | tags | user | cate |  |  |  | cate_wgt |  |  |
    | cate_wgt |  string  | weights | user |  |  |  |  |  |  |  |
    | **... ** |  |  |  |  |  |  |  |  |  |  |
    ```

    |Parameter|Description|
    |---------|-----------|
    |name|The column name.|
    |data\_type|The data type. Valid values: STRING, BIGINT, and DOUBLE.|
    |type|The feature type. Values in the types column of the [types](#li_types) sheet are referenced:    -   label: prediction column
    -   category: discrete value feature
    -   dense: continuous value feature
    -   tags: tag-type feature
    -   weights: the weight of tags
    -   indexes: a string of digits which are separated by an in-column separator. Example: 1,2,4,5.
    -   notneed: not required. You can ignore this value. |
    |group|Group settings. Values in the sheet column of the [group](#li_group) sheet are referenced:    -   MultiTower
        -   user: user tower
        -   item: item tower
        -   user\_item: user\_item tower
        -   label: label tower
    -   DeepFM
        -   wide: Features are only used in the wide section.
        -   deep: Features are only used in the deep and fm sections.
        -   wide\_and\_deep: Features are used in the wide, deep, and fm sections. wide\_and\_deep is selected by default. |
    |global|Values in the name column of the [global](#li_global) sheet are referenced.    -   hash\_bucket\_size: the size of the hash bucket.
    -   embedding\_dim: the embedding size.
    -   default\_value: missing data imputation.
    -   weights: You can specify this parameter if type is tags. weights and tags must have the same column.
    -   boundaries: the intervals within which continuous values are discretized. For example, continuous values 10,20,30 are discretized into the following intervals: \(-inf, 10\), \[10, 20\), \[20, 30\), \[30, inf\).

**Note:** To specify boundaries, you must also specify embedding\_dim.

    -   query: not used currently. It serves as the target of DIN. Generally, item\_id is used.

**Note:**

        -   Columns in the features sheet must map the columns in the CSV file in the same sequence.
        -   The number of columns in the features sheet must be consistent with the number of columns in the input table or file.
        -   If some columns are not required, set type to notneed. |

-   global: describes share embedding information.

    The settings of hash\_bucket\_size, embedding\_dim, and default\_value will overwrite those in the features sheet.

    ```
    | **name** | **type** | **hash_bucket_size** | **embedding_dim** | **default_value** |
    | --- | --- | --- | --- | --- |
    | cate | category | 1000 | 10 | 0 |
    | uid | category | 100000 | 10 | 0 |
    | **... ** |  |  |  |  |
    ```

-   group
    -   - Group settings in the MultiTower model

        ```
        | **group** |
        | --- |
        | user |
        | item |
        | user_item |
        | label |
        ```

    -   - Group settings in the DeepFM model

        ```
        | **group** |
        | --- |
        | wide_and_deep |
        | wide |
        | deep |
        | label |
        ```

-   types: describes data types.

    ```
    | **types** |
    | --- |
    | label |
    | category |
    | tags |
    | weights |
    | dense |
    | indexes |
    | notneed |
    ```

-   basic\_types: describes the data types of an input table. Valid values: STRING, BIGINT, and DOUBLE.

    ```
    | **basic_types** |
    | --- |
    | string |
    | bigint |
    | double |
    ```


