---
layout:     post
title:      "Manipulate Datetime and timestamp in PySpark"
date:       2019-09-10 12:00:00
author:     "Jason Feng"
header-img: "img/tunnel-4427609_1920.jpg"
excerpt_separator: <!--more-->
tags:
    - Time-Series
    - PySpark
---

Apache Spark is a general-purpose computational framework which is well known for big data processing. This post will show how to manipulate time-series related data with PySpark.
<!--more-->
### The Data
Let us make some synthetic stock market data and put them into a DataFrame.
```python
df = spark.createDataFrame([
    (18, "2017-03-09T10:27:18+00:00", "GOOG"),
    (17, "2017-03-10T15:27:18+00:00", "GOOG"),
    (22, "2017-03-13T12:27:18+00:00", "GOOG"),
    (13, "2017-03-15T12:27:18+00:00", "GOOG"),
    (19, "2017-03-15T02:27:18+00:00", "GOOG"),
    (25, "2017-03-18T11:27:18+00:00", "GOOG")],
    ["Close", "timestampGMT", "Symbol"])
```

### Datetime functions in PySpark
`pyspark.sql.functions` module provides a rich set of functions to handle and manipulate datetime/timestamp related data.

#### Convert timestamp string to Unix time
[Unix Epoch](https://www.epochconverter.com/) time is widely used especially for internal storage and computing.

The format arguement is following the pattern letters of the Java class *[java.text.SimpleDateFormat](https://docs.oracle.com/javase/8/docs/api/java/text/SimpleDateFormat.html)*.
```python
df = df.withColumn('unix_time', F.unix_timestamp(F.col('timestampGMT'), "yyyy-MM-dd'T'HH:mm:ssX"))
df.show(truncate=False)                                                           
+-----+-------------------------+------+----------+
|Close|timestampGMT             |Symbol|unix_time |
+-----+-------------------------+------+----------+
|18   |2017-03-09T10:27:18+00:00|GOOG  |1489055238|
|17   |2017-03-10T15:27:18+00:00|GOOG  |1489159638|
|22   |2017-03-13T12:27:18+00:00|GOOG  |1489408038|
|13   |2017-03-15T12:27:18+00:00|GOOG  |1489580838|
|19   |2017-03-15T02:27:18+00:00|GOOG  |1489544838|
|25   |2017-03-18T11:27:18+00:00|GOOG  |1489836438|
+-----+-------------------------+------+----------+
```

#### Get proper timestamp format
By default, Spark use the time zone setting of the system. You can change to your desired time zone with setting the configuration 
```python
df = df.withColumn('local_ts', F.date_format('timestampGMT', 'dd/MM/yyyy HH:mm:ss Z'))
```

#### Truncate to specific unit of time
If we want to aggregate the timestamp to a higher level, for example, get the average value of every day. We can get the date value and then group by the date to calculate the average value.
```python
df = df.withColumn('local_date', F.date_trunc('day', 'timestampGMT'))
df_by_date = df.groupBy('Symbol', 'local_date').agg(F.avg('Close').alias('avg_close'))
df_by_date.show(truncate=False)                                                                                        
+------+-------------------+---------+
|Symbol|local_date         |avg_close|
+------+-------------------+---------+
|GOOG  |2017-03-09 00:00:00|18.0     |
|GOOG  |2017-03-11 00:00:00|17.0     |
|GOOG  |2017-03-13 00:00:00|22.0     |
|GOOG  |2017-03-15 00:00:00|16.0     |
|GOOG  |2017-03-18 00:00:00|25.0     |
+------+-------------------+---------+
```

### Compute moving average
We can use [window functions](https://spark.apache.org/docs/latest/api/python/pyspark.sql.html?highlight=dateformat#pyspark.sql.Window) in PySpark to calculate the moving average for each symbol. If we want to get the results based on date range, the trick is the window_frame_clause is only supported numeric expression. We need to use `unix_time` column which is long type before applying the window function with a litte helper lambda function.
```python
days = lambda d: d * 24 * 60 * 60

win_spec = Window.partitionBy('Symbol') \
    .orderBy('unix_time') \
    .rangeBetween(-days(7), 0)
col_mean_7d = F.avg('Close').over(win_spec)
df_mean = df.withColumn('mean_7d', col_mean_7d)
df_mean.show(truncate=False)                                                                                 
+-----+-------------------------+------+----------+-------------------------+-------------------+-------+
|Close|timestampGMT             |Symbol|unix_time |local_ts                 |local_date         |mean_7d|
+-----+-------------------------+------+----------+-------------------------+-------------------+-------+
|18   |2017-03-09T10:27:18+00:00|GOOG  |1489055238|09/03/2017 21:27:18 +1100|2017-03-09 00:00:00|18.0   |
|17   |2017-03-10T15:27:18+00:00|GOOG  |1489159638|11/03/2017 02:27:18 +1100|2017-03-11 00:00:00|17.5   |
|22   |2017-03-13T12:27:18+00:00|GOOG  |1489408038|13/03/2017 23:27:18 +1100|2017-03-13 00:00:00|19.0   |
|19   |2017-03-15T02:27:18+00:00|GOOG  |1489544838|15/03/2017 13:27:18 +1100|2017-03-15 00:00:00|19.0   |
|13   |2017-03-15T12:27:18+00:00|GOOG  |1489580838|15/03/2017 23:27:18 +1100|2017-03-15 00:00:00|17.8   |
|25   |2017-03-18T11:27:18+00:00|GOOG  |1489836438|18/03/2017 22:27:18 +1100|2017-03-18 00:00:00|19.75  |
+-----+-------------------------+------+----------+-------------------------+-------------------+-------+
```
If we just want to compute the moving average from the preceding *N* records to current record, we can use **rangeBetween** instead of **rowsBetween**.
```python
win_spec = Window.partitionBy('Symbol') \
    .orderBy('unix_time') \
    .rowsBetween(-4, 0)
col_mean_5r = F.avg('Close').over(win_spec)
df_mean = df.withColumn('mean_5r', col_mean_5r)
df_mean.show(truncate=False)
+-----+-------------------------+------+----------+-------------------------+-------------------+-------+
|Close|timestampGMT             |Symbol|unix_time |local_ts                 |local_date         |mean_5r|
+-----+-------------------------+------+----------+-------------------------+-------------------+-------+
|18   |2017-03-09T10:27:18+00:00|GOOG  |1489055238|09/03/2017 21:27:18 +1100|2017-03-09 00:00:00|18.0   |
|17   |2017-03-10T15:27:18+00:00|GOOG  |1489159638|11/03/2017 02:27:18 +1100|2017-03-11 00:00:00|17.5   |
|22   |2017-03-13T12:27:18+00:00|GOOG  |1489408038|13/03/2017 23:27:18 +1100|2017-03-13 00:00:00|19.0   |
|19   |2017-03-15T02:27:18+00:00|GOOG  |1489544838|15/03/2017 13:27:18 +1100|2017-03-15 00:00:00|19.0   |
|13   |2017-03-15T12:27:18+00:00|GOOG  |1489580838|15/03/2017 23:27:18 +1100|2017-03-15 00:00:00|17.8   |
|25   |2017-03-18T11:27:18+00:00|GOOG  |1489836438|18/03/2017 22:27:18 +1100|2017-03-18 00:00:00|19.2   |
+-----+-------------------------+------+----------+-------------------------+-------------------+-------+
```
Soure code can be found [here](https://github.com/q15928/python-snippets/blob/master/pyspark/time-series/pyspark_ts.py).

*Image by [Peter H](https://pixabay.com/users/Tama66-1032521/?utm_source=link-attribution&amp;utm_medium=referral&amp;utm_campaign=image&amp;utm_content=4427609) from [Pixabay](https://pixabay.com/?utm_source=link-attribution&amp;utm_medium=referral&amp;utm_campaign=image&amp;utm_content=4427609)*