---
layout:     post
title:      "Spark source code snippets"
date:       2019-05-12 12:00:00
author:     "Jason Feng"
header-img: "img/post-bg-2015.jpg"
mathjax: true
excerpt_separator: <!--more-->
tags:
    - Spark
    - PySpark
    - Spark SQL
---

I put the source code snippets from the book __Spark: The Definitive Guide__ into one piece. They cover most of the operations and common functions for DataFrames and Spark SQL in our daily life when writing Spark code.

<!--more-->

Apache Spark is arguably one of the most popular big data processing frameworks. [Spark: The Definitive Guide](http://shop.oreilly.com/product/0636920034957.do) is written by its creator Matei Zaharia. This book is a must-have comprehensive guide for anyone who wants to learn Spark.

The source code snippets include sections as:

- DataFrame operations, such as filtering rows, adding columns, sorting rows, etc.
- Work with different data types, string manipulation, regexp, timestamp, etc.
- Perform aggregations, such as grouping, window functions
- Join multiple DataFrames with different types: inner join, outer join, left/right join, cross join
- Read and write with different data sources, such as csv, JSON, Parquet, SQL Databases  

It can be found [here](https://github.com/q15928/python-snippets/blob/master/pyspark/pyspark-def-guide.py).