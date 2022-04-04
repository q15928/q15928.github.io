---
layout:     post
title:      "Construct next business date with Spark"
date:       2022-01-15 12:00:00
author:     "Jason Feng"
header-img: "img/landscape-336542_1920.jpg"
excerpt_separator: <!--more-->
tags:
    - Spark
    - Data Analytics
---
Here is how to use Pyspark to generate a dataframe showing the date and the next business date given the country and optional state.
<!--more-->
### Key points
- Use `sequence` and `explode` functions to generate the calendar date.
- Use *holidays* package to create the dataframe of holidays given the country and optional state.
- Filter out weekends and use `left_anti` to exclude public holidays.
- Apply window functions to derive next business date.

### Show me the code
```python
from pyspark.sql import SparkSession
import pyspark.sql.functions as F
from pyspark.sql.window import Window
from holidays.utils import country_holidays


spark = SparkSession.builder.appName('next-buz-date').master('local[*]').getOrCreate()


def get_next_buz_date(start_date, end_date, country, state=None):
    raw_df = spark.createDataFrame(
        [(start_date, end_date)], ('start_date', 'end_date')
    )
    weekday_df = raw_df.withColumn(
        'start_date',
        F.to_date('start_date')
    ).withColumn(
        'end_date',
        F.to_date('end_date')
    ).withColumn(
        'date',
        F.expr('sequence(start_date, end_date, INTERVAL 1 DAY)')
    ).withColumn(
        'date',
        F.explode('date')
    ).drop(
        'start_date', 'end_date'
    ).filter(
        ~F.dayofweek('date').isin([1, 7])
    ).withColumn(
        'year', 
        F.year('date')
    )
    year_list = [r[0] for r in weekday_df.select('year').distinct().collect()]
    holiday_list = [(h,) for h in country_holidays(country, state, year_list)]
    holiday_df = spark.createDataFrame(holiday_list, 'date: date')
    win_spec = Window.orderBy('date').rowsBetween(Window.currentRow+1, Window.currentRow+1)
    buz_day_df = weekday_df.join(
        holiday_df, 'date', 'left_anti'
    ).select(
        'date'
    ).withColumn(
        'next_buz_date', 
        F.first('date').over(win_spec)
    ).withColumn(
        'country',
        F.lit(country)
    ).withColumn(
        'state',
        F.lit(state)
    )
    return buz_day_df

```

### Link to colab
Here is the [link](https://colab.research.google.com/drive/1raQDUtntPnxEvzpCq0lz6Ev7rwD6gKR2?usp=sharing)
