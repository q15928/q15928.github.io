---
layout:     post
title:      "A Simple Serverless App to Check Price"
date:       2022-11-18 12:00:00
author:     "Jason Feng"
header-img: "img/landscape-336542_1920.jpg"
excerpt_separator: <!--more-->
tags:
    - AWS
    - Serverless
---

I had been Googling the price for a gift and I was hoping the price would drop when it was close to Christmas. But I was not bothered to check the price everyday. Well, why not build a simple serverless app for this task? Ahh, just a few days later, I got a message that the price was dropped!
<!--more-->

![](/img/2022-12-10-serverless-app.png)
### AWS Services
- EventBridge Scheduler to invoke the app on a daily basis.
- Lambda function to check the price and send it to SNS topic.
- SNS to notify me via SMS.

### Steps
#### 1. Setup Cloud9
It needs to install Python 3.9 in Cloud9 which is the version used in Lambda. The simplest way is to install miniconda. Restart the shell after installation.
```shell
sudo wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh

bash Miniconda3-latest-Linux-x86_64.sh
```
#### 2. Deploy with SAM
SAM is pre-installed in Cloud9. Just change to the folder with the template and use SAM CLI to deploy the serverless app.
```shell
cd get-oppo-price
sam build
sam deploy --guided
```

### Source code
SAM template and source code is [here](https://github.com/q15928/aws-snippets/tree/master/get-oppo-price).
### Reference
- [Simple pattern that triggers a Lambda function every 5 minutes using Amazon EventBridge Scheduler](https://serverlessland.com/patterns/eventbridge-schedule-to-lambda)
- [Create a Lambda function that publishes to an SNS topic](https://serverlessland.com/patterns/lambda-sns)