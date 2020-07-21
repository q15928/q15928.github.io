---
layout:     post
title:      "Streaming Process with AWS Kinesis"
date:       2020-06-27 12:00:00
author:     "Jason Feng"
header-img: "img/tunnel-4427609_1920.jpg"
excerpt_separator: <!--more-->
tags:
    - AWS
    - Kinesis
    - Streaming Processing
---
Notes from the workshops of building a streaming data platform on AWS.
<!--more-->

Speed is definitely a key characteristic businesses are looking to have for a competitive edge in today’s world. Consumers increasingly want experiences that are timely, targeted, and tailored to their specific needs, whether they are applying for a loan, investing, checking health alerts, online shopping , tracking home security alerts, or monitoring systems.

Amazon Kinesis makes it easy to collect, process, and analyze real-time, streaming data so you can get timely insights and react quickly to new information. It is fully managed, scalable, real-time streaming data process platfrom. It provides the following services:

### Kinesis Data Streams
This is similar to Apache Kafka, the message bus that producers and consumers are decoupled to generate the data and to read the data. By default, the messages are retained for 24 hours. The core concept here is topic and shard. Topic is the name of a specific stream of data. Shard defines the capacity of reading and writing to the stream concurrently. Number of shards is required to manually configured to suite the throughput of the streaming data. This is similar to partition in Apache Kafka. 

### Kinesis Data Firehose
It can deliever the stream data to a certain AWS services like S3, RedShift, Elastic Search and Splunk. The source data can be Kinesis Data Streams. The more common use case is Lambda function is invoked once new data arrives in KDS. The data is processed by the Lambda function and then sent to Kinesis Data Firehose which is configured with a specific detination. KDF can specify the file format as well, for example, save as parquet files in S3 buckets.

### Kinesis Data Analytics
It provides the easy way to analyze streaming data, gain actionable insights, and respond to your business and customer needs in real time. We can create Kinesis Data Analytics for SQL Applications, write SQL code and do schema transformation, or even do complex data analysis using inbuilt Machine learning feature.

We can also deploy Apache Flink application (supported Java or Scala) in Kinesis Data Analytics to process the streaming data. KDA will manage the underlying infrastruture and cluster used by Apache Flink, including provisioning compute resources, parallel processing, auto-scaling, and application backups. We just need to pay what we use.

### Kinesis Video Steams
To be updated.

![](/img/2020-06-27-aws-kinesis-arch.png)
### Reference
- [Building a streaming analytics application on AWS Labs](https://kinesis-immersion-day1.s3.amazonaws.com/immersionday/README.html)
- [Amazon Kinesis Replay](https://github.com/aws-samples/amazon-kinesis-replay)
- [episode 1](https://anz-resources.awscloud.com/anz-webinars-on-demand-developer/episode-1-real-time-data-processing-core-concepts-and-create-your-first-data-stream-3)
- [episode 2](https://anz-resources.awscloud.com/anz-webinars-on-demand-developer/episode-2-stream-data-processing-with-kinesis-data-firehose-and-aws-lambda-3)
- [episode 3](https://anz-resources.awscloud.com/anz-webinars-on-demand-developer/episode-3-serverless-stream-processing-with-sql-3)
- [episode 4](https://anz-resources.awscloud.com/anz-webinars-on-demand-developer/episode-4-building-end-to-end-stream-processing-pipeline-3)
- [AWS ANZ Webinars](https://anz-resources.awscloud.com/anz-webinars-on-demand)