---
title: "Spark is still a safe port when compared to DuckDB and Polars"
date: 2023-06-17T17:09:29+03:00
tags: ["data engineering", "spark", "duckdb", "polars"]
description: "The author faced obstacles and encountered performance issues with DuckDB and Polars, while Spark proved to be a reliable and efficient solution. The experience highlights the importance of having the necessary skills to work with these technologies and implies that Spark just works when it comes to data processing tasks."
draft: false
---

{{< alert "circle-info" >}}
**Disclaimer:** Any opinions expressed are solely my own.
{{< /alert >}}

<br />

## Introduction
There are many data processing technologies available today compared to the past. Some of them work as distributed systems, while others work as standalone solutions. There is no silver bullet, as all of these technologies cover specialized problems. In this post, we will discuss a basic data processing job that can be performed using **Spark, DuckDB, and Polars**.

## Problem
The goal of the basic data processing job is to perform the following steps:

- Read a **JSON** file.
- Write a **Parquet** file.

Thus, the job involves converting a JSON file to a Parquet file. Naturally, our data is complex, unpredictable, and heavily contaminated, just like real-world data.

## Proposed System

### DuckDB

```sql
CREATE TABLE d AS SELECT * FROM 'd.json';
COPY d to 'd.parquet' (FORMAT parquet);
```

Unfortunately, this process takes more than 30 minutes 🤷.

### Polars

```python
import polars as pl
df = pl.read_ndjson("d.json")
```

```python
   1028     source = normalise_filepath(source)
   1030 self = cls.__new__(cls)
-> 1031 self._df = PyDataFrame.read_ndjson(source)
   1032 return self
RuntimeError: BindingsError: "expected list/array in JSON value, got str"
```

Once again, there are strange errors for very basic commands.

### Spark

```bash
./bin/spark-shell --master "local[8]"
```

```scala
val df = spark.read.json(path)
df.coalesce(1).write.parquet(outputPath)
```

The task completes within a few minutes.

## Conclusion
I needed to perform some basic data processing jobs quickly, but unfortunately, I faced some obstacles... I'm not sure, maybe DuckDB needs some optimizations for complex data structures, and Polars requires some more time. This short story shows me that no matter what, Spark still ***just works*** if you have the necessary skills.

---

<script async data-uid="46fa8c47ab" src="https://mert-kavi.ck.page/46fa8c47ab/index.js"></script>