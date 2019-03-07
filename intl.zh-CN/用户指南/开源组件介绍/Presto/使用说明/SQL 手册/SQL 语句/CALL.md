# CALL {#concept_v2y_3m3_ygb .concept}

## 概要 {#section_p5k_ll3_ygb .section}

```
CALL procedure_name ( [ name => ] expression [, ...] )
```

## 描述 {#section_fym_ml3_ygb .section}

调用存储过程。存储过程可以由连接器提供，用于数据操作或管理任务。如果底层存储系统具有自己的存储过程框架，如PostgreSQL，则需要通过连接器提供的存储过程来访问这些存储系统自己的存储过程，而不能使用`CALL`直接访问。

## 示例 {#section_phk_nl3_ygb .section}

```
CALL test(123, 'apple'); --- 调用存储过程并传参（参数位置）
CALL test(name => 'apple', id => 123); --- 调用存储过程并传参（命名参数）
CALL catalog.schema.test(); --- 调用权限定名称的存储过程
```

