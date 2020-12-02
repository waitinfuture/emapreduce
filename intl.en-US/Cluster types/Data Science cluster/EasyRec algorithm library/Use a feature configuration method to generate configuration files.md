# Use a feature configuration method to generate configuration files

This topic describes how to use a feature configuration method to generate configuration files. Common feature classes include the ID class, continuous value class, tag class, and sequence class.

## ID class

In features of the ID class, `user_id` and `itemid` have many values. Hash operations are performed to map ID values to a small-sized bucket. Examples:

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

Assume that features of the ID class, such as workday and level, have few values.

-   Values can be written into `vocab_list`. Example:

    ```
    feature_configs: {
      input_names: "workday"
      feature_type: IdFeature
      vocab_list: ["Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday", "Sunday"]
      embedding_dim: 8
    }
    ```

-   Values can be mapped to a hash bucket without the need to configure `vocab_list`.

    ```
    feature_configs: {
      input_names: "workday"
      feature_type: IdFeature
      hash_bucket_size: 70
      embedding_dim: 10
    }
    ```

    **Note:** To prevent a hash conflict, set hash\_bucket\_size to a value that is five to ten times the value of embedding\_dim.


## Continuous value class

Discretized intervals can be manually configured for features of the continuous value class. Example:

```
feature_configs: {
  input_names: "ctr"
  feature_type: RawFeature
  boundaries: [0.1, 0.2, 0.3, 0.4, 0.5, 0.6, 0.7, 0.8, 0.9, 1.0]
  embedding_dim: 8
}
```

## Tag class

Features of the tag class are specified in the format of XX\|XX\|XX. For example, tag features of an article are Entertainment\|Funny\|Popular.

You must specify hash\_bucket\_size for each value in the tag class. Example:

```
feature_configs: {
  input_names: "article_tag"
  feature_type: TagFeature
  embedding_dim: 16
  hash_bucket_size: 1000
}
```

## Sequence class

Features of the sequence class are specified in the format of XX\|XX\|XX. For example, sequence features of user behavior are item\_id1\|item\_id2\|item\_id3.

You must specify hash\_bucket\_size for each value in the sequence class. Example:

```
feature_configs: {
  input_names: "play_sequence"
  feature_type: SequenceFeature
  embedding_dim: 64
  hash_bucket_size: 100000
}
```

