---
layout:     post
title:      "AWS Data Analytics Exam Preparation"
date:       2021-12-11 12:00:00
author:     "Jason Feng"
header-img: "img/landscape-336542_1920.jpg"
excerpt_separator: <!--more-->
tags:
    - AWS
    - Data Analytics Certification
---
My study notes for the AWS Data Analytics Certification.
<!--more-->

- You can 10 consumer application. You can issue up to 5 GetRecords API calls per second, so it'll take 2 seconds for each consuming application before they can issue their next call. Each shard can support up to 5 transactions per second for reads, up to a maximum total data read rate of 2 MB per second and up to 1,000 records per second for writes, up to a maximum total data write rate of 1 MB per second (including partition keys). The total capacity of the stream is the sum of the capacities of its shards.

- No matter how many consumers you have, in enhanced fan out mode, each consumer will receive 2MB per second of throughput and have an average latency of 70ms.

- Because DynamodB table is under provisioned, checkpointing does not happen fast enough and results in a lower throughput for your KCL based application. Make sure to increase the RCU / WCU

- you get about 3k reads per second per prefix, so using the device-id will help having many prefixes and parallelize your writes.

- Kinesis Data Analytics provisions capacity in the form of Kinesis Processing Units (KPU). A single KPU provides you with the memory (4 GB) and corresponding computing and networking. The default limit for KPUs for your application is eight.

- Kinesis, DynamoDB, Logstash / Beats, and Elasticsearch's native API's offer means to import data into Amazon ES.

- Amazon ES created daily snapshots to S3 by default, and you can create them more often if you wish.

- Using columnar formats such as ORC and Parquet can reduce costs 30-90%, while improving performance at the same time.

- Presto running on Amazon EMR gives you more flexibility in how you configure and run queries, including the ability to federate to other data sources if needed. you may have a workflow in which you need to join data between different systems like MySQL, Amazon Redshift, Apache Cassandra, and Hive.

- Presto is a fast SQL query engine designed for interactive analytic queries over large datasets from multiple sources. It supports both non-relational sources, such as the Hadoop Distributed File System (HDFS), Amazon S3, and HBase, and relational data sources such as MySQL, PostgreSQL, and Amazon Redshift. Presto can query data where it’s stored, without needing to move data into a separate analytics system. Query execution runs in parallel over a pure memory-based architecture, with most results returning in seconds.

-To prevent some of the problems that arise when you have duplicate data or receive out-of-order data, you need to pay attention to two ingestion tool characteristics: delivery (de-duping) functionality and guaranteed ordering. However, if you have a requirement to de-duplicate incoming data, keep in mind that most ingestion tools categorize data delivery using these three phrases: at-least-once, at-most-once, and exactly-once.

- Operational data stores (often referred to as OLTP data stores) consist mostly of transactional data and are built for quick updates. These storage systems are designed for high volume, low latency access, and are often regular, repeatable workloads. 

Compared to analytic data stores, operational data stores are usually:

Data stored in a row-based format
Smaller compute size
Low latency 
High throughput
High concurrency
High change velocity
Usually a good fit for caching
Mission-critical, HA, DR, data protection 

- Analytic data stores consist of two different types of data stores: OLAP and Decision Support Systems (DSS). OLAP systems provide a more responsive framework for real-time feedback and ad-hoc queries. DSS systems, such as data lakes and data warehouses, are useful for long-running query aggregations and projections, where latency is less important. 

Compared to operational data stores, analytic data stores are usually:

Data commonly stored in a columnar format 
Datasets are large and use partitioning
Large compute size 
Regularly performs complex joins and aggregations
Bulk loading or trickle inserts
Low change velocity

- DynamoDB with provisioned capability mode, you specify the number of reads (RCUs) and writes (WCUs) per second that you expect your application to require. 1 RCU represents one strongly consistent read or 2 eventually consistent reads per second for an item up to 4 KB in size. 1 WCU represents one write per second for an item up to 1 KB in size.

- Global secondary indexes (GSI) allow the partition key and optional sort key to differ from those on the base table. A GSI is considered "global" because queries on the index can span all the data in the base table, across all partitions. GSIs provide the most query flexibility. 

- Local secondary indexes (LSI) use the same partition key as the base table but allow you to specify a different sort key when required.  LSI is "local" in the sense that the data in LSIs is organized by the same partition key as the base table, but with a different sort key.

- Each Amazon Redshift cluster has one leader node, two or more compute nodes, and each compute node has a number of slices. Each node slice is an independent partition of data. The slices can perform operations in parallel to complete operations significantly faster than systems that do not use slices. 

- When you load data into a table, Amazon Redshift distributes the table's rows to the compute nodes and slices according to the distribution style that you chose when you created the table. The four different distribution styles are: key, even, all, auto.
Some suggestions for best approach follow:

    - Distribute the fact table and one dimension table on their common columns.
    - Choose the largest dimension based on the size of the filtered dataset.
    - Choose a column with high cardinality in the filtered result set.
    - Change some dimension tables to use ALL distribution.


- When you create a table, you can define one or more of its columns as sort keys. Don’t use an interleaved sort key on columns with monotonically increasing attributes, such as identity columns, dates, or timestamps. A compound sort key is more efficient when query predicates use a prefix, which is a subset of the sort key columns in order. 

- If no compression is specified in a CREATE TABLE or ALTER TABLE statement, Amazon Redshift automatically assigns compression encoding as follows:

Columns that are defined as BOOLEAN, REAL, or DOUBLE PRECISION data types are assigned RAW compression.

Columns that are defined as SMALLINT, INTEGER, BIGINT, DECIMAL, DATE, TIMESTAMP, or TIMESTAMPTZ data types are assigned AZ64 compression.

Columns that are defined as CHAR or VARCHAR data types are assigned LZO compression.

Best practices for optimizing data size: 

Split your load data files so that the files are about equal size, between 1 MB and 1 GB after compression. For optimum parallelism, the ideal size is between 1 MB and 125 MB after compression. 
The number of files should be a multiple of the number of slices in your cluster.
When loading data, we strongly recommend that you individually compress your load files using gzip, lzop, bzip2, or Zstandard when you have large datasets.

- A classifier recognizes the format of your data and generates a schema. It returns a certainty number between 0.0 and 1.0. If the classifier recognizes the format of the data, it generates a schema. If a classifier returns certainty=1.0 during processing, it indicates that it's 100 percent certain that it can create the correct schema. AWS Glue then uses the output of that classifier. If no classifier returns certainty=1.0, AWS Glue uses the output of the classifier that has the highest certainty. If no classifier returns a certainty greater than 0.0, AWS Glue returns the default classification string of UNKNOWN.

- An AWS Glue resource policy can only be used to manage permissions for Data Catalog resources. You can't attach it to any other AWS Glue resources such as jobs, triggers, development endpoints, crawlers, or classifiers. A resource policy is attached to a catalog, which is a virtual container for all the kinds of Data Catalog resources mentioned previously. Each AWS account owns a single catalog in an AWS Region whose catalog ID is the same as the AWS account ID. A catalog cannot be deleted or modified.

- Identity-based resources are attached to an IAM entity and indicate what the user or group is permitted to do. Resource-based permissions are attached to a resource and indicate what a specified user (or group of users) is permitted to do with it.

- Possible reasons for Amazon Kinesis Data Streams Producer Application is Writing at a Slower Rate Than Expected: Service Limits Exceeded or Producer Optimization

- Configure a Kinesis Data Firehose stream to load data into an Amazon Elasticsearch Service (Amazon ES) cluster. Analyze the files using Amazon ES. Kinesis Data Firehose can transform data prior to storage using an AWS Lambda function. Amazon ES can perform near-real-time analytics on streaming data.

- Linux Unified Key Setup, or LUKS, is a disk encryption specification that can help encrypt sensitive data at rest in EMR. Other options are EBS encryption, open-source HDFS encryption

- Use the COPY and NOLOAD commands to check the integrity of all of the data without loading it into Redshift.

- Best practice for designing and using partition keys effectively is using a partition key that uniquely identifies each item in a DynamoDB table. Since the student ID number uniquely identifies the student, then a composite key is not required.

- Use the CreateIngestion API call to create and start a new SPICE ingestion on the dataset. Automate the transformation process and use this API call to refresh the dashboard after the transformations have completed. You can use the CreateIngestion command to create and start a new SPICE ingestion on the dataset which will refresh the dashboard with the most up-to-date data. 

- When using the COPY command to move data from an S3 bucket, two things are required: an IAM role for accessing S3 resources and ensuring that data is either auto-committed or there's an explicit COMMIT at the end of your COPY command to save the changes uploaded from S3.

- In redshift, inspect the SVV_TABLE_INFO table's unsorted_rows and vacuum_sort_benefit to determine the number of unsorted rows and performance benefit from sorting them.

- You can fix the processing of the multiple files by using the grouping feature in AWS Glue. Grouping is automatically enabled when you use dynamic frames and when the input dataset has a large number of files (more than 50,000). Grouping allows you to coalesce multiple files together into a group, and it allows a task to process the entire group instead of a single file. As a result, the Spark driver stores significantly less state in memory to track fewer tasks. It can prevent the "java.lang.OutOfMemoryError: Java heap space" error.

- You can use a JDBC (Java Database Connectivity) connection to connect Athena to business intelligence tools and other applications, such as SQL Workbench. ODBC (Open Database Connectivity) is also supported in Amazon Athena.

- Amazon QuickSight Enterprise edition supports both AWS Directory Service for Microsoft Active Directory and Active Directory Connector. AD Connector is a directory gateway with which you can redirect directory requests to your on-premises Microsoft Active Directory without caching any information in the cloud.

- Amazon Redshift doesn't support a single merge statement (update or insert, also known as an upsert) to insert and update data from a single data source. However, you can effectively perform a merge operation. To do so, load your data into a staging table and then join the staging table with your target table for an UPDATE statement and an INSERT statement.

- A good rule of thumb is to try to keep a shard size between 10–50 GiB. Large shards can make it difficult for Elasticsearch to recover from failure, but because each shard uses some amount of CPU and memory, having too many small shards can cause performance issues and out of memory errors. In other words, shards should be small enough that the underlying Amazon ES instance can handle them, but not so small that they place needless strain on the hardware.

- You can also ingest data into your Amazon Elasticsearch domain using Amazon Kinesis Firehose, AWS IoT, or Amazon CloudWatch Logs.

- Using the Amazon EMR version 5.7.0 or later, you can set up a read-replica cluster, which allows you to maintain read-only copies of data in Amazon S3. In the event that the primary cluster becomes unavailable, you can access the data from the read-replica cluster to perform read operations simultaneously.

- Glacier Select can only do uncompressed CSV files. Amazon S3 Select works on objects stored in CSV, JSON, or Apache Parquet format. It also works with objects compressed with GZIP or BZIP2 (for CSV and JSON objects only) and server-side encrypted objects. 

- Enable S3 Transfer Acceleration and point the application's S3 endpoint to the bucketname.s3-accelerate.amazonaws.com endpoint domain name.

- With AUTO distribution, Amazon Redshift assigns an optimal distribution style based on the size of the table data. For example, Amazon Redshift initially assigns ALL distribution to a small table, then changes to EVEN distribution when the table grows larger. When a table is changed from ALL to EVEN distribution, storage utilization might change slightly. The change in distribution occurs in the background, in a few seconds.

- Data Pipeline is the best tool available to extract all of the records from a DynamoDB table as quickly as possible without writing any code.

- Since there is a requirement for the data to have an unlimited retention period, then MSK is the only option that will work. With MSK, you can provide custom configurations that allow for an unlimited retention period by setting the following configuration log.retention.ms to -1.

- Using VPC endpoints to S3 will allow QuickSight private access to the S3 data, and using Active Directory Connector will allow corporate network users to connect to QuickSight.

- You have multiple options to encrypt data at rest in your EMR clusters. EMR by default uses the EMR file system (EMRFS) to read from and write data to Amazon S3. To encrypt data in Amazon S3, you can specify one of the following options:

SSE-S3: Amazon S3 manages the encryption keys for you
SSE-KMS: You use an AWS Key Management Service (AWS KMS) customer master key (CMK) to encrypt your data server-side on Amazon S3. Be sure to use policies that allow access by Amazon EMR.
CSE-KMS/CSE-C: Amazon S3 encryption and decryption takes place client-side on your Amazon EMR cluster. You can use keys provided by AWS KMS (CSE-KMS) or use a custom Java class that provides the master key (CSE-C).

- Cognito supports sign-in with social identity providers - such as Facebook, Google, and Amazon - and enterprise identity providers via SAML 2.0, and is generally used as an authentication mechanism for web and mobile applications built on AWS. In 2018 AWS announced support for authenticating to Kibana using Amazon Cognito.

- Kinesis Firehose can only deliver data to a single target,

- Enhanced VPC Routing forces Redshift to use the VPC for all COPY and UNLOAD commands, which in turn will help us make sure that all that traffic appears on the VPC Flow logs.

- Amazon Kinesis Data Streams can only send data to Lambda, EC2, Kinesis Data Analytics, and Amazon EMR.

- Redshift, enable concurrency scaling for the Executive dashboard user group? Enable concurrency scaling in the workload management (WLM) queue.

- Redshift, there is only column-level access control. While in QuickSight there is row-level security(RLS)

- Kinesis Data Analytics, stagger window, sliding window, tumbling window

- By default, AWS Glue processes and writes out data in 100-second windows.

- Redshift, best approach to seperate storage and computation is to use RA3 nodes

- CDC files are loaded into Redshift. The loading is taking much longer than initial load. To speed up, run the COPY command with COMPUPDATE option off.

- Amazon AppFlow is a fully managed integration service that enables you to securely transfer data between Software-as-a-Service (SaaS) applications like Salesforce, SAP, Zendesk, Slack, and ServiceNow, and AWS services like Amazon S3 and Amazon Redshift, in just a few clicks. 

- the buffer interval in Amazon Elasticsearch Service only ranges from 60 seconds to 900 seconds.

- When automatic scaling is configured, Amazon EMR adds and removes instances based on Amazon CloudWatch metrics that you specify. Only an Instance Group has this feature. The following are two of the most common metrics used for automatic scaling in Amazon EMR:

- YarnMemoryAvailablePercentage: The percentage of remaining memory available to YARN.

- ContainerPendingRatio: The ratio of pending containers to containers allocated. You can use this metric to scale a cluster based on container-allocation behavior for varied loads, which is useful for performance tuning.

- S3DistCp does not support concatenation for Parquet files. Amazon recommends using PySpark instead.

- AWS Lambda supports Parallelization Factor, a feature that allows you to process one shard of a Kinesis or DynamoDB data stream with more than one Lambda invocation simultaneously.

- AWS Glue can crawl data in different AWS Regions. When you define an Amazon S3 data store to crawl, you can choose whether to crawl a path in your account or another account. Then you can use Athena to query the data has been crawled. Athena can't query S3 data in another region by default.

- You create Redshift Spectrum tables by defining the structure for your files and registering them as tables in an external data catalog.

- When EMRFS makes a request to Amazon S3, the underlying EC2 instances assume the corresponding IAM role for the request if it matches the basis for access. The IAM permissions attached to that role apply instead of the IAM permissions attached to the service role for cluster EC2 instances.

- To authorize Amazon QuickSight to access your Amazon S3 bucket, go to the QuickSight Console and locate the S3 bucket that you want to access from Amazon QuickSight by clicking Manage QuickSight -> Security & permissions.

- Redshift resize: elastic, classic, Snapshot and restore with classic resize. AWS recommends using elastic resize when possible. For example, you can perform a classic resize only if you are resizing to a single-node cluster. Or, perform a classic resize if you want your node slices to match the number of slices in your target node type. 

- Amazon Redshift logs information in the following log files:
Connection log — logs authentication attempts, and connections and disconnections.
User log — logs information about changes to database user definitions.
User activity log — logs each query before it is run on the database.

- Amazon Redshift workload management (WLM) enables users to flexibly manage priorities within workloads so that short, fast-running queries won't get stuck in queues behind long-running queries. You can use workload management (WLM) to define multiple query queues and to route queries to the appropriate queues at runtime.

- Amazon Kinesis Data Streams (KDS) You can register up to 20 consumers per data stream.

- AWS recommends using Athena workgroups to isolate queries for teams, applications, or different workloads.

- Redshift, You can use a manifest to ensure that the COPY command loads all of the required files, and only the required files, for a data load. 

- Block Public Access configuration is an account-level configuration that helps you centrally manage public network access to EMR clusters in a region.

- When you load all the data from a single large file, Amazon Redshift is forced to perform a serialized load, which is much slower. The COPY command loads the data in parallel from multiple files, dividing the workload among the nodes in your cluster. Split your load data files so that the files are about equal size, between 1 MB and 1 GB after compression. For optimum parallelism, the ideal size is between 1 MB and 125 MB after compression. The number of files should be a multiple of the number of slices in your cluster.

- There are two primary reasons why records may be delivered more than one time to your Amazon Kinesis Data Streams application: producer retries (network timeout) and consumer retries (worker terminated, merged or split shards).