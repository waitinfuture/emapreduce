# Query bank employee information {#concept_xgd_gtk_z2b .concept}

## 1. Create a temporary table {#section_fb4_ltk_z2b .section}

```
%spark
import org.apache.commons.io.IOUtils
import java.net.URL
import java.nio.charset.Charset
// Zeppelin creates and injects sc (SparkContext) and sqlContext (HiveContext or SqlContext)
// So you don't need create them manually
// load bank data
val bankText = sc.parallelize(
    IOUtils.toString(
        new URL("http://emr-sample-projects.oss-cn-hangzhou.aliyuncs.com/bank.csv"),
        Charset.forName("utf8")).split("\n"))
case class Bank(age: Integer, job: String, marital: String, education: String, balance: Integer)
val bank = bankText.map(s => s.split(";")).filter(s => s(0) ! = "\"age\"").map(
    s => Bank(s(0).toInt, 
            s(1).replaceAll("\"", ""),
            s(2).replaceAll("\"", ""),
            s(3).replaceAll("\"", ""),
            s(5).replaceAll("\"", "").toInt
        )
).toDF()
bank.registerTempTable("bank")
```

## 2. Query the table structure {#section_gbt_4tk_z2b .section}

```
%sql
desc bank
```

## 3. Query the number of employees in each age group under 30 {#section_h23_ttk_z2b .section}

```
%sql select age, count(1) value from bank where age < 30 group by age order by age
```

## 4. Query the information of employees younger than or equal to 20 {#section_xl5_5tk_z2b .section}

```
%sql select * from bank where age <= 20
```

