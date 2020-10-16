# Excel特征配置

本文为您介绍如何通过Excel方式生成配置文件。

## 命令

Excel模板如下：

-   multi\_tower：[multi\_tower\_template.xls](https://pai-vision-data-hz.oss-cn-zhangjiakou.aliyuncs.com/easy-rec/excel/multi_tower_template.xls)
-   deepfm：[deepfm\_template.xls](https://pai-vision-data-hz.oss-cn-zhangjiakou.aliyuncs.com/easy-rec/excel/deepfm_template.xls)
-   wide\_and\_deep：同deepfm。

```
# bash
pip install pandas
wget https://pai-vision-data-hz.oss-cn-zhangjiakou.aliyuncs.com/easy-rec/releases/scripts/create_config_from_excel.py
python scripts/create_config_from_excel.py --model_type multi_tower --excel_path multi_tower_template.xls --output_path multi_tower.config
```

|参数|说明|
|--|--|
|`model_type`|必须和Excel模板匹配。|
|`excel_path`|输出Config文件。|
|`output_path`|输入输出文件，train\_input\_path TRAIN\_INPUT\_PATH和eval\_input\_path EVAL\_INPUT\_PATH。默认数据文件（CSV）列（Column）分割符号是`,` ；列（Column）内部里面字符分割符号是`|`。

您可以自定义分隔符：`--column_separator $'|' --incol_separator $','`。 |

## 配置说明

模板包含features、global、group、types和basic\_types共五个页签。

-   features描述特征配置。

    ```
    | **name** | **data_type** | **type** | **user_item_other** | **global** | **hash_bucket_size** | **embedding_dim** | **default_value** | **weights** | **boundaries** | **query** |
    | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
    | label | double | label | label |  |  |  |  |  |  |  |
    | uid |  string  | category | user | uid |  |  |  |  |  |  |
    | own_room |  bigint  | dense | user |  |  |  |  |  | 10,20,30 |  |
    | cate_idx |  string  | tags | user | cate |  |  |  | cate_wgt |  |  |
    | cate_wgt |  string  | weights | user |  |  |  |  |  |  |  |
    | **...** |  |  |  |  |  |  |  |  |  |  |
    ```

    |参数|说明|
    |--|--|
    |name|输入的列名。|
    |data\_type|输入的数据类型，包括STRING，BIGINT和DOUBLE。|
    |type|特征类型，引用[types](#li_types)中的types列：    -   label：要预测的列
    -   category：离散值特征
    -   dense：连续值特征
    -   tags：标签型特征
    -   weights：tags对应的weight
    -   indexes：一串数字，使用in column separator分割。例如：1,2,4,5。
    -   notneed：不需要的，可以忽略的 |
    |group|分组设置，引用[group](#li_group)sheet列：    -   multi\_tower
        -   user：user tower
        -   item：item tower
        -   user\_item：user\_item tower
        -   label：label tower
    -   deepfm
        -   wide：特征仅用在wide部分
        -   deep：特征仅用在deep和fm部分
        -   wide\_and\_deep：特征用在wide, deep, fm部分，默认选wide\_and\_deep |
    |global|引用[global](#li_global)里面的name列。    -   hash\_bucket\_size：hash\_bucket桶的大小
    -   embedding\_dim：embedding的大小
    -   default\_value：缺失值填充
    -   weights：如果type是tags，则可以指定weights。weights和tags必须要有相同的列
    -   boundaries：连续值离散化的区间。例如：10,20,30，将会离散成区间\(-inf, 10\), \[10, 20\), \[20, 30\), \[30, inf\)。

**说明：** 配置boundaries时，通常也需要配置embedding\_dim。

    -   query：当前未使用，拟用作DIN的target，通常是item\_id。

**说明：**

        -   features必须和CSV文件的列是一一对应的，顺序必须要一致。
        -   features的数目和输入标或者文件的列的数目必须是一致的
        -   如果某些列不需要，可以设置type为notneed。 |

-   global：描述share embedding里面share embedding的信息。

    其中hash\_bucket\_size embedding\_dim default\_value会覆盖features表里面对应的信息。

    ```
    | **name** | **type** | **hash_bucket_size** | **embedding_dim** | **default_value** |
    | --- | --- | --- | --- | --- |
    | cate | category | 1000 | 10 | 0 |
    | uid | category | 100000 | 10 | 0 |
    | **...** |  |  |  |  |
    ```

-   group
    -   - multi\_tower模型中的分组设置

        ```
        | **group** |
        | --- |
        | user |
        | item |
        | user_item |
        | label |
        ```

    -   - deep\_fm模型中的分组设置

        ```
        | **group** |
        | --- |
        | wide_and_deep |
        | wide |
        | deep |
        | label |
        ```

-   types：描述数据类型。

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

-   basic\_types：描述输入表的数据类型， 包括STRING，BIGINT和DOUBLE。

    ```
    | **basic_types** |
    | --- |
    | string |
    | bigint |
    | double |
    ```


