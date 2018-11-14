# Process Table Store data in MR {#concept_hzz_rvn_hfb .concept}

This topic describes how to process Table Store data in MR.

## Connect MR to Table Store {#section_d42_txn_hfb .section}

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

-   The following example describes how to process Table Store data in MR.

    ```
    public class RowCounter {
        public static class RowCounterMapper
          extends Mapper<PrimaryKeyWritable, RowWritable, Text, LongWritable> {
            private final static Text agg = new Text("TOTAL");
            private final static LongWritable one = new LongWritable(1);
            @Override public void map(PrimaryKeyWritable key, RowWritable value, 
                Context context) throws IOException, InterruptedException {
                context.write(agg, one);
            }
        }
        public static class IntSumReducer
          extends Reducer<Text,LongWritable,Text,LongWritable> {
            @Override public void reduce(Text key, Iterable<LongWritable> values, 
                Context context) throws IOException, InterruptedException {
                long sum = 0;
                for (LongWritable val : values) {
                    sum += val.get();
                }
                context.write(key, new LongWritable(sum));
            }
        }
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
        public static void main(String[] args) throws Exception {
            Configuration conf = new Configuration();
            job.setJarByClass(RowCounter.class);
            job.setMapperClass(RowCounterMapper.class);
            job.setCombinerClass(IntSumReducer.class);
            job.setReducerClass(IntSumReducer.class);
            job.setOutputKeyClass(Text.class);
            job.setOutputValueClass(LongWritable.class);
            job.setInputFormatClass(TableStoreInputFormat.class);
            TableStore.setCredential(job, accessKeyId, accessKeySecret, securityToken);
            TableStore.setEndpoint(job, endpoint, instance);
            TableStoreInputFormat.addCriteria(job, fetchCriteria());
            FileOutputFormat.setOutputPath(job, new Path(outputPath));
            System.exit(job.waitForCompletion(true) ? 0 : 1);
        }
    }
    ```

-   The following example describes how to use MR to write data to Table Store.

    ```
    public static class OwnerMapper
      extends Mapper<PrimaryKeyWritable, RowWritable, Text, MapWritable> {
        @Override public void map(PrimaryKeyWritable key, RowWritable row, 
            Context context) throws IOException, InterruptedException {
            PrimaryKeyColumn pet = key.getPrimaryKey().getPrimaryKeyColumn("name");
            Column owner = row.getRow().getLatestColumn("owner");
            Column species = row.getRow().getLatestColumn("species");
            MapWritable m = new MapWritable();
            m.put(new Text(pet.getValue().asString()),
                new Text(species.getValue().asString()));
            context.write(new Text(owner.getValue().asString()), m);
        }
    }
    public static class IntoTableReducer
      extends Reducer<Text,MapWritable,Text,BatchWriteWritable> {
        @Override public void reduce(Text owner, Iterable<MapWritable> pets, 
            Context context) throws IOException, InterruptedException {
            List<PrimaryKeyColumn> pkeyCols = new ArrayList<PrimaryKeyColumn>();
            pkeyCols.add(new PrimaryKeyColumn("owner",
                    PrimaryKeyValue.fromString(owner.toString())));
            PrimaryKey pkey = new PrimaryKey(pkeyCols);
            List<Column> attrs = new ArrayList<Column>();
            for(MapWritable petMap: pets) {
                for(Map.Entry<Writable, Writable> pet: petMap.entrySet()) {
                    Text name = (Text) pet.getKey();
                    Text species = (Text) pet.getValue();
                    attrs.add(new Column(name.toString(),
                            ColumnValue.fromString(species.toString())));
                }
            }
            RowPutChange putRow = new RowPutChange(outputTable, pkey)
                .addColumns(attrs);
            BatchWriteWritable batch = new BatchWriteWritable();
            batch.addRowChange(putRow);
            context.write(owner, batch);
        }
    }
    public static void main(String[] args) throws Exception {
        Configuration conf = new Configuration();
        Job job = Job.getInstance(conf, TableStoreOutputFormatExample.class.getName());
        job.setMapperClass(OwnerMapper.class);
        job.setReducerClass(IntoTableReducer.class);
        job.setMapOutputKeyClass(Text.class);
        job.setMapOutputValueClass(MapWritable.class);
        job.setInputFormatClass(TableStoreInputFormat.class);
        job.setOutputFormatClass(TableStoreOutputFormat.class);
        TableStore.setCredential(job, accessKeyId, accessKeySecret, securityToken);
        TableStore.setEndpoint(job, endpoint, instance);
        TableStoreInputFormat.addCriteria(job, ...) ;
        TableStoreOutputFormat.setOutputTable(job, outputTable);
        System.exit(job.waitForCompletion(true) ? 0 : 1);
    }
    ```


## Appendix {#section_gtd_rxn_hfb .section}

For the complete sample code, see:

-   [MR reads data from Table Store](https://github.com/aliyun/aliyun-emapreduce-sdk/blob/master/examples/src/main/java/com/aliyun/openservices/tablestore/hadoop/RowCounter.java)
-   [MR writes data to Table Store](https://github.com/aliyun/aliyun-emapreduce-sdk/blob/master/examples/src/main/java/com/aliyun/openservices/tablestore/hadoop/TableStoreOutputFormatExample.java)

