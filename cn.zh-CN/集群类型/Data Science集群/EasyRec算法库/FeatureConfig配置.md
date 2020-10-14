# FeatureConfig配置

本文为您介绍如何通过特征配置方式生成配置文件。常用的特征有ID类、连续值类、Tag类和Sequense类。

## ID类

ID类特征中，`user_id`和`itemid`取值数较多，通常做Hash处理，映射ID值到相对较小的桶中。如下例所示。

```
feature_configs: {
  input_names: "uid"
  feature_type: IdFeature
  embedding_dim: 64
  hash_bucket_size: 100000
}
feature_configs: {
  input_names: "iid"
  feature_type: IdFeature
  embedding_dim: 64
  hash_bucket_size: 100000
}
```

workday和level等ID类特征取值数较少时：

-   取值可以直接写在`vocab_list`中，如下例所示。

    ```
    feature_configs: {
      input_names: "workday"
      feature_type: IdFeature
      vocab_list: ["星期一", "星期二", "星期三", "星期四", "星期五", "星期六", "星期日"]
      embedding_dim: 8
    }
    ```

-   取值也可以直接映射到Hash桶中，无需配置`vocab_list`。

    ```
    feature_configs: {
      input_names: "workday"
      feature_type: IdFeature
      hash_bucket_size: 70
      embedding_dim: 10
    }
    ```

    **说明：** 为防止Hash冲突，通常设置Hash桶数（hash\_bucket\_size）为取值数（embedding\_dim）的5~10倍。


## 连续值类

连续值类特征可以在Config中手动配置离散化的区间，如下例所示。

```
feature_configs: {
  input_names: "ctr"
  feature_type: RawFeature
  boundaries: [0.1, 0.2, 0.3, 0.4, 0.5, 0.6, 0.7, 0.8, 0.9, 1.0]
  embedding_dim: 8
}
```

## Tag类

Tag类特征格式一般为“XX\|XX\|XX”，例如文章标签特征为“娱乐\|搞笑\|热门”。

对于Tag中的每个值，同样需要配置hash\_bucket\_size。如下例所示。

```
feature_configs: {
  input_names: "article_tag"
  feature_type: TagFeature
  embedding_dim: 16
  hash_bucket_size: 1000
}
```

## Sequense类

Sequense类特征格式通常为“XX\|XX\|XX”，例如用户行为序列特征为“item\_id1\|item\_id2\|item\_id3”。

对于序列中的每个值，同样需要配置hash\_bucket\_size，如下例所示。

```
feature_configs: {
  input_names: "play_sequence"
  feature_type: SequenceFeature
  embedding_dim: 64
  hash_bucket_size: 100000
}
```

