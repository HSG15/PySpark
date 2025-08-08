# 🚀 PySpark Functions Cheat Sheet

A quick reference for essential **PySpark DataFrame functions** — from column operations to performance tuning. Perfect for interview prep or as a quick lookup while coding.

---

## 🔧 Column Operations

| Function | Definition | Example |
|----------|------------|---------|
| `withColumn()` | Add a new column or update an existing one. | `df.withColumn("age_plus1", df.age + 1)` |
| `drop()` | Remove unwanted columns. | `df.drop("address")` |
| `alias()` | Give a column or DataFrame a temporary name. | `df.select(df.name.alias("full_name"))` |
| `cast()` | Change a column’s data type. | `df.withColumn("age_str", df.age.cast("string"))` |
| `selectExpr()` | Run SQL-like expressions directly. | `df.selectExpr("name", "age + 5 as age_plus5")` |

---

## 🔍 Filtering & Conditions

| Function | Definition | Example |
|----------|------------|---------|
| `filter()` / `where()` | Keep rows matching a condition. | `df.filter(df.age > 30)` |
| `isin()` | Check if column value is in a list. | `df.filter(df.city.isin("Delhi", "Mumbai"))` |
| `when().otherwise()` | If-else for columns. | `df.withColumn("senior", when(df.age > 60, "Yes").otherwise("No"))` |
| `isNull()` / `isNotNull()` | Check for missing values. | `df.filter(df.email.isNull())` |

---

## 📊 Aggregation & Grouping

| Function | Definition | Example |
|----------|------------|---------|
| `groupBy().agg()` | Group and apply aggregations. | `df.groupBy("city").agg(avg("salary"))` |
| `count(), sum(), avg(), max(), min()` | Built-in aggregations. | `df.groupBy("dept").sum("salary")` |
| `distinct()` | Keep only unique rows. | `df.distinct()` |
| `dropDuplicates()` | Remove duplicates by column subset. | `df.dropDuplicates(["name"])` |

---

## 📆 Date & Time Functions

| Function | Definition | Example |
|----------|------------|---------|
| `current_date()`, `current_timestamp()` | Get today’s date / date-time. | `df.withColumn("today", current_date())` |
| `datediff()` | Days between two dates. | `df.withColumn("days", datediff(df.end, df.start))` |
| `add_months()` | Add months to a date. | `df.withColumn("new_date", add_months(df.start, 2))` |
| `months_between()` | Months between two dates. | `df.withColumn("months", months_between(df.end, df.start))` |
| `date_format()` | Change date display format. | `df.withColumn("formatted", date_format(df.start, "yyyy-MM"))` |
| `to_date()`, `to_timestamp()` | Convert strings to date/timestamp. | `df.withColumn("dt", to_date(df.date_str, "dd-MM-yyyy"))` |

---

## 🧠 Window Functions

| Function | Definition | Example |
|----------|------------|---------|
| `rank()` | Rank rows (skips numbers for ties). | `rank().over(Window.partitionBy("dept").orderBy("salary"))` |
| `dense_rank()` | Rank rows (no gaps). | `dense_rank().over(Window.partitionBy("dept").orderBy("salary"))` |
| `row_number()` | Unique row number per group. | `row_number().over(Window.partitionBy("dept").orderBy("salary"))` |
| `lag()`, `lead()` | Previous / next row value. | `lag("salary", 1).over(win)` |
| `partitionBy().orderBy()` | Define grouping & sorting for window. | `Window.partitionBy("dept").orderBy("salary")` |

---

## 🧪 Data Cleaning & Transformation

| Function | Definition | Example |
|----------|------------|---------|
| `replace()` | Replace values. | `df.replace({"NY": "New York"}, "city")` |
| `fillna()` | Fill missing values. | `df.fillna({"salary": 0})` |
| `dropna()` | Drop rows with nulls. | `df.dropna()` |
| `explode()` | Expand array elements into rows. | `df.select(explode(df.skills))` |
| `split()` | Split string into array. | `df.withColumn("parts", split(df.name, " "))` |
| `concat()` | Combine columns. | `df.withColumn("full", concat(df.fname, df.lname))` |
| `substring()` | Extract part of string. | `df.withColumn("year", substring(df.date, 1, 4))` |

---

## 🔁 Joins & Unions

| Function | Definition | Example |
|----------|------------|---------|
| `join()` | Merge DataFrames on keys. | `df1.join(df2, "id", "inner")` |
| `union()` | Combine DataFrames with same schema. | `df1.union(df2)` |
| `unionByName()` | Combine by matching column names. | `df1.unionByName(df2)` |
| `broadcast()` | Speed up joins with small DataFrame. | `df1.join(broadcast(df2), "id")` |

---

## 🗂 Schema & Metadata

| Function | Definition | Example |
|----------|------------|---------|
| `printSchema()` | Show DataFrame schema tree. | `df.printSchema()` |
| `schema.fields` | List column names/types. | `[f.name for f in df.schema.fields]` |
| `dtypes` | Column names with types. | `df.dtypes` |

---

## ⚡ Performance & Partitioning

| Function | Definition | Example |
|----------|------------|---------|
| `cache()` | Store DataFrame in memory for reuse. | `df.cache()` |
| `persist()` | Store with custom storage level. | `df.persist(StorageLevel.DISK_ONLY)` |
| `repartition()` | Increase/change number of partitions (full shuffle). | `df.repartition(8)` |
| `coalesce()` | Reduce partitions without full shuffle. | `df.coalesce(2)` |
| `rdd.getNumPartitions()` | Show current partitions count. | `df.rdd.getNumPartitions()` |

---

## 💡 Quick Tips
- **Use `cache()`/`persist()`** when the DataFrame will be reused multiple times.  
- **`repartition()`** is best when increasing or balancing partitions.  
- **`coalesce()`** is best when reducing partitions after filtering.  
- For joins, **broadcast small tables** to avoid shuffles.


