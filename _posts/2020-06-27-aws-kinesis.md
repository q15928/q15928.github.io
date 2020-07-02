---
layout:     post
title:      "AWS Kinesis"
date:       2020-06-27 12:00:00
author:     "Jason Feng"
header-img: "img/tunnel-4427609_1920.jpg"
excerpt_separator: <!--more-->
tags:
    - AWS
    - Kinesis
    - Streaming Processing
---
Notes from workshops of building a streaming data platform on AWS.
<!--more-->

Speed is definitely a key characteristic businesses are looking to have for a competitive edge in today’s world. Consumers increasingly want experiences that are timely, targeted, and tailored to their specific needs, whether they are applying for a loan, investing, checking health alerts, online shopping , tracking home security alerts, or monitoring systems.

Amazon Kinesis makes it easy to collect, process, and analyze real-time, streaming data so you can get timely insights and react quickly to new information. It is fully managed, scalable, real-time streaming data process platfrom. It provides the following services:

- **Kinesis Data Streams**
This is similar to Apache Kafka, the message bus that producers and consumers are decoupled to generate the data and read the data. It 
- **Kinesis Data Firehose**
It can deliever the stream data to a certain AWS services like S3, Elastic Search and Splunk.
- **Kinesis Data Analytics**
It provides the easy way to analyze streaming data, gain actionable insights, and respond to your business and customer needs in real time. We can create Kinesis Data Analytics for SQL Applications, write SQL code and do schema transformation, or even do complex data analysis using inbuilt Machine learning feature
- **Kinesis Video Steams**


### Reference
- [Building a streaming analytics application on AWS Labs](https://kinesis-immersion-day1.s3.amazonaws.com/immersionday/README.html)
- [Amazon Kinesis Replay](https://github.com/aws-samples/amazon-kinesis-replay)