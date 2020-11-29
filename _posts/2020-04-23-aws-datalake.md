---
layout:     post
title:      "Data Lake, how to dig?"
date:       2020-04-23 12:00:00
author:     "Jason Feng"
header-img: "img/landscape-336542_1920.jpg"
excerpt_separator: <!--more-->
tags:
    - AWS
    - Data Lake
---
There is an introductory article of AWS services which can help to build data lake in WeChat. I translated to English as I found it quite intuitive. 
<!--more-->

Long time ago, when data was limited, people could remembered them without noting down, or just put knots in the ropes.
![](/img/2020-04-23-aws-datalake-1.png)
Then database was invented for the sake of more efficient recording and processing. The core of database was to query, insert, delete and update records for online transactions. For example, you spend with your bank card, the backend database is required to record this transaction in real time, and update the balance of your account.
![](/img/2020-04-23-aws-datalake-2.png)

As the time goes by, the amount of data are kept increasing. The database is not only needed to cater for online transaction processing, it is required to provide online analytical processing. However, the traditional database can only support high frequent and fast IO requests. It is hard to run complex queries to analyze large amount of data.
![](/img/2020-04-23-aws-datalake-3.png)

Hence, people came up data processing based on the database. More precisely, data are extracted, transformed and loaded (ETL) into another system which is called data warehouse. The main purpose of data warehouse is used for business intellegence (BI), dashboard, data analytics and insights, etc.
![](/img/2020-04-23-aws-datalake-4.png)

Quick recap, **database is used for OLTP, which captures, stores, and processes data from transactions in real time. data warehouse is used for OLAP, which uses complex queries to analyze large amount of historical data from OLTP systems.**
![](/img/2020-04-23-aws-datalake-5.png)
![](/img/2020-04-23-aws-datalake-6.png)

Data in both OLTP and OLAP systems are mainly structured data. Together with these two systems, most companies can deal with the real time transactional process and analytical process.

However, people is never satisfied with status quo. The types of data become more and more. For example, semi-structured data such as csv, XML, JSON; un-structured data such as text, mail, image, video. Companies want to derive values from these **Big Data**. They want to store them and utilize them properly.
![](/img/2020-04-23-aws-datalake-7.png)

How can we deal with such complicated data with various formats? How about putting all of them into a lake!!
![](/img/2020-04-23-aws-datalake-8.png)
This is the prototype of Data Lake which serves as a central repository of all kinds of heterogeneous data. 

Why not Data River? We need to keep the data. We can't let go the data along with the flow.

Why not Data Pool? Like a swimming pool? It is not big enough for the growing data.

Why not Data Sea? We need a boundary for the data. They can't flow out to other sea freely.

Then it comes to the challenges of data lake.
1. Where to dig the lake? The storage aspect of data lake.
2. How to get the water flow in? The ETL process of data lake.
3. How to serve for good with the lake? The analytics of data lake. 

With various AWS services, data lake can be built with much less effort.

### Storage
First and foremost, data lake is an architect of storage of data. So it is natually to think of S3 as the foundation of data lake. Companies can quickly dig up a just-fit "lake" which is scalable and shrinkable, only paid for the amount of water (data).
![](/img/2020-04-23-aws-datalake-10.png)

### ETL
Then it is the process to get all the heterogeneous data into the lake. AWS Glue comes into spot for this part. It has a component called Crawler which can discover the metadata (schema) of the data. Glue ETL jobs run Apache Spark to process and load the data either via our customised code or visual ETL code generation. Furthermore, Glue is a serverless service which we don't need to manage the underlying cluster.
![](/img/2020-04-23-aws-datalake-11.png)

### Analytics
Athena lets us to run standard SQL queries over the data stored in S3. It is also serverless and we can run the query and get the data insights within a short time no matter it is hundreds of MB or GB of data. It is billed by the amount of data which the query scans.
![](/img/2020-04-23-aws-datalake-12.png)

### Other services
There are other services to meet the different requirements. EMR is for big data processing, Kinesis is for streaming data processing, QickSight is for data visualisation, etc..
![](/img/2020-04-23-aws-datalake-13.png)

#### References
- Original article - [数据湖这个大坑，是怎么挖的？](https://mp.weixin.qq.com/s?__biz=MzA4ODMwMDcxMQ==&mid=2650902149&idx=2&sn=c4d8d50ab2e5067245de3990c6f96f13&chksm=8bd9634dbcaeea5b9acfd96b6caefc3ab69ef480c285e87ebadaaac66c436f5a9a8b3b5411a3&mpshare=1&scene=24&srcid=0704sXIhsUSJmsHGgzFs8EZY&sharer_sharetime=1593820431922&sharer_shareid=511a36666c7bd1945174e668895435c5&ascene=14&devicetype=android-27&version=27001141&nettype=WIFI&abtest_cookie=AAACAA%3D%3D&lang=en&exportkey=AkquQQlag8Jnw72c71yAj8I%3D&pass_ticket=0Gtc5JiGOLDgl2t7jRfJ9RJOjKZww%2BVMo9I6ysAaoVhqVCgXC7kJic7d7EbEV47%2F&wx_header=1)
- [Data Lake vs Data Warehouse](https://luminousmen.com/post/data-lake-vs-data-warehouse)

*Image by [Free-Photos](https://pixabay.com/photos/?utm_source=link-attribution&amp;utm_medium=referral&amp;utm_campaign=image&amp;utm_content=336542) from [Pixabay](https://pixabay.com/?utm_source=link-attribution&amp;utm_medium=referral&amp;utm_campaign=image&amp;utm_content=336542)*