# Example of bank employee information inquiry {#concept_xgd_gtk_z2b .concept}

## Section one: Create a temporary table {#section_fb4_ltk_z2b .section}

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

## Section two: Query the table structure {#section_gbt_4tk_z2b .section}

```
%sql
desc bank
```

## Section three: Query the number of employees of each age group below 30 {#section_h23_ttk_z2b .section}

```
%sql select age, count(1) value from bank where age < 30 group by age order by age
```

## Section 4: Query the information of employees at the age less than or equal to 20 {#section_xl5_5tk_z2b .section}

```
%sql select * from bank where age <= 20
```

