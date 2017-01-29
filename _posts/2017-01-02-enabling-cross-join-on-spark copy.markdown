---
layout: post
title:  "Enabling Cross Join on Spark"
date:   2017-01-02
tag: apache spark
blog: true
---

I was working on a quick Python script to check whether the transformation and calculation done on a particular dataset is, in fact, correct. One of the first things I did was join the original and the transformed dataset. The script would be run on Apache Spark. This is one of the SQL queries I was using:

```python
df_count = spark.sql("SELECT id, COUNT(hit_rules) as `hit_rules_count` FROM rules LEFT OUTER JOIN transactions ON INSTR(hit_rules, id) > 0 GROUP BY id ORDER BY id")
```

The script, however, returned an error: 

```
Require explicit CROSS join for cartesian products.
```

It took me quite a while to get it, but then I realized that when you're joining both tables with a condition that doesn't use `=`, it will require cartesian products between the two tables. This is because basically when you're joining two tables, you need something to join it with (hence the need of the `=` operator). When you're not using `=` as the condition, the query will instead make a cartesian product of the two tables before proceeding to eliminate the rows that don't fulfill the condition. Depending on the size of your tables, there's a huge chance that the query would take forever to run. I guess that's why Spark requires an explicit declaration of `CROSS` join--it's like saying, "yo, you'll end up with X millions of rows so you better know what you're doing!".

Unfortunately I had to make do with what I had due to the nature of the dataset itself--modifying the original dataset wasn't an option, indexing also wasn't an option and I had no other choice but to use `INSTR` (or its equally evil sibling `%LIKE%`, which is a story for another time!). Transforming the dataset was possible, but then again the purpose of the script was to evaluate the transformed dataset--therefore, I'd have to keep the whole transformation thing to a minimum. So, `CROSS` join it is.

All right, back to Spark! When I ran my script, Spark detected that there was a cartesian product and yet I wasn't explicitly declaring a `CROSS` join, which eventually triggered the error. One way to solve this error is to either explicitly declare the `CROSS` join on the SQL query. Another way is to set `spark.sql.crossJoin.enabled` to `true`, thus disabling the check and allowing Spark to run the query without having `CROSS` join declared in the query:

```python
# Enabling cross join to allow cartesian products without explicit declaration
conf = SparkConf()
conf.set("spark.sql.crossJoin.enabled", "true")

# Initiating SparkSession with the configuration
spark = SparkSession \
    .builder.config(conf=conf) \
    .appName("Audit Transactions").getOrCreate()

```

