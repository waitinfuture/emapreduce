# Spark + TableStore {#concept_fyq_12h_hfb .concept}

本章节将介绍如何在Spark中消费TableStore中的数据。

## Spark接入TableStore {#section_m5v_b2h_hfb .section}

-   准备一张数据表

创建一张表pet，其中name为主键。

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

-   下面这个例子示例了如何在Spark中消费TableStore中的数据。

    ```
    private static RangeRowQueryCriteria fetchCriteria() {
        RangeRowQueryCriteria res = new RangeRowQueryCriteria("pet");
        res.setMaxVersions(1);
        List<PrimaryKeyColumn> lower = new ArrayList<PrimaryKeyColumn>();
        List<PrimaryKeyColumn> upper = new ArrayList<PrimaryKeyColumn>();
        lower.add(new PrimaryKeyColumn("name", PrimaryKeyValue.INF_MIN));
        upper.add(new PrimaryKeyColumn("name", PrimaryKeyValue.INF_MAX));
        res.setInclusiveStartPrimaryKey(new PrimaryKey(lower));
        res.setExclusiveEndPrimaryKey(new PrimaryKey(upper));
        return res;
    }
    public static void main(String[] args) {
        SparkConf sparkConf = new SparkConf().setAppName("RowCounter");
        JavaSparkContext sc = new JavaSparkContext(sparkConf);
        Configuration hadoopConf = new Configuration();
        JavaSparkContext sc = null;
        try {
            sc = new JavaSparkContext(sparkConf);
            Configuration hadoopConf = new Configuration();
            TableStore.setCredential(
                    hadoopConf,
                    new Credential(accessKeyId, accessKeySecret, securityToken));
            Endpoint ep = new Endpoint(endpoint, instance);
            TableStore.setEndpoint(hadoopConf, ep);
            TableStoreInputFormat.addCriteria(hadoopConf, fetchCriteria());
            JavaPairRDD<PrimaryKeyWritable, RowWritable> rdd = sc.newAPIHadoopRDD(
                    hadoopConf, TableStoreInputFormat.class,
                    PrimaryKeyWritable.class, RowWritable.class);
            System.out.println(
                new Formatter().format("TOTAL: %d", rdd.count()).toString());
        } finally {
            if (sc != null) {
                sc.close();
            }
        }
    }
    ```


## 附录 {#section_gxr_l2h_hfb .section}

完整示例代码请参考：

-   [Spark接入TableStore](https://github.com/aliyun/aliyun-emapreduce-sdk/blob/master/examples/src/main/java/com/aliyun/openservices/tablestore/spark/RowCounter.java)

