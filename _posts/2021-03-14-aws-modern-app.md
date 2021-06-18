---
layout:     post
title:      "Building Modern Python Applications on AWS - week six"
date:       2021-03-14 12:00:00
author:     "Jason Feng"
header-img: "img/tunnel-4427609_1920.jpg"
excerpt_separator: <!--more-->
tags:
    - AWS
    - CloudFront
    - Lambda
---
This is week six course notes of [Building Modern Python Applications on AWS](https://www.coursera.org/learn/building-modern-python-applications-on-aws/home/welcome) on Coursera.
<!--more-->
### AWS Global Infrastructure: Edge Locations
AWS Edge Locations are sites that Amazon CloudFront uses to cache copies of your content for faster delivery to users around the world. By deploying content to the edge locations, the latency in your application can be reduced for end users since they are accessing the resources at a location that is physically closer to them than the region you hosted your resources in originally. 

Read more about Amazon CloudFront and Edge Locations here: [https://aws.amazon.com/cloudfront/](https://aws.amazon.com/cloudfront/)  

### Amazon API Gateway Response Caching
You can enable API caching in Amazon API Gateway to cache your endpoint's responses. With caching, you can reduce the number of calls made to your endpoint and also improve the latency of requests to your API.

When you enable caching for a stage, API Gateway caches responses from your endpoint for a specified time-to-live (TTL) period, in seconds. API Gateway then responds to the request by looking up the endpoint response from the cache instead of making a request to your endpoint. The default TTL value for API caching is 300 seconds. The maximum TTL value is 3600 seconds. TTL=0 means caching is disabled.

To read more about Response Caching click here: [https://docs.aws.amazon.com/apigateway/latest/developerguide/api-gateway-caching.html](https://docs.aws.amazon.com/apigateway/latest/developerguide/api-gateway-caching.html)

### AWS Lambda @ Edge
Lambda@Edge is a feature of Amazon CloudFront that lets you run code closer to users of your application, which improves performance and reduces latency. With Lambda@Edge, you don't have to provision or manage infrastructure in multiple locations around the world. You pay only for the compute time you consume - there is no charge when your code is not running.

Read about use cases of Lambda@Edge here:  [https://aws.amazon.com/lambda/edge/#Website_Security_and_Privacy](https://aws.amazon.com/lambda/edge/#Website_Security_and_Privacy)

Read more general information about Lambda@Edge here: [https://docs.aws.amazon.com/lambda/latest/dg/lambda-edge.html](https://docs.aws.amazon.com/lambda/latest/dg/lambda-edge.html)

### AWS Lambda Layers
You can configure your Lambda function to pull in additional code and content in the form of layers. A layer is a ZIP archive that contains libraries, a custom runtime, or other dependencies. With layers, you can use libraries in your function without needing to include them in your deployment package.

Layers let you keep your deployment package small, which makes development easier.

Read more about layers here: [https://docs.aws.amazon.com/lambda/latest/dg/configuration-layers.html](https://docs.aws.amazon.com/lambda/latest/dg/configuration-layers.html)

### AWS Lambda Performance and Pricing
Following best practices with your AWS Lambda functions can help provide a more streamline and cost-efficient utilization of this component within your workflows. Make sure to keep the Lambda pricing calculator bookmarked to help provide estimations about how changes in your function build and utilization might affect the cost of your service usage.

Find the pricing calculator here: [https://s3.amazonaws.com/lambda-tools/pricing-calculator.html](https://s3.amazonaws.com/lambda-tools/pricing-calculator.html)

### AWS Lambda Power Tuning
The efficiency and cost of your lambda function often times relies on the amount of CPU and memory you have given you function. The more power you give a function, the more it costs to run. That being said, it can often be cheaper to run a function with more power. The reason for this is that your code runs faster with more CPU and memory available, so it can be a good exercise to do Lambda Power Tuning to find the best settings for your lambda function. 

AWS Lambda Power Tuning is an AWS Step Functions state machine that helps you optimize your Lambda functions in a data-driven way. 

You can provide any Lambda function as input and the state machine will run it with multiple power configurations (from 128MB to 3GB), analyze execution logs and suggest you the best configuration to minimize cost or maximize performance.

The state machine will generate a dynamic visualization of average cost and speed for each power configuration.

Find the AWS Lambda Power Tuning project here: [https://serverlessrepo.aws.amazon.com/applications/arn:aws:serverlessrepo:us-east-1:451282441545:applications~aws-lambda-power-tuning](https://serverlessrepo.aws.amazon.com/applications/arn:aws:serverlessrepo:us-east-1:451282441545:applications~aws-lambda-power-tuning)

### AWS Lambda Best Practices
To read a list of AWS Lambda Best Practices in detail click here: [https://docs.aws.amazon.com/lambda/latest/dg/best-practices.html](https://docs.aws.amazon.com/lambda/latest/dg/best-practices.html)

### Amazon API Gateway Proxy for AWS Services
One API Gateway integration type is the AWS integration type. This allows you to proxy other AWS services with API Gateway.

Each method you setup for a reason using an AWS integration type would expose one API or AWS service action. Because the AWS API will need the data in a specific format that your clients likely won’t be using, you must configure both the integration request and integration response and set up necessary data mappings from the method request to the integration request, and from the integration response to the method response. This will transform the request and response to adhere to the specifications needed for the AWS API.

Read more about API Gateway integration types here: [https://docs.aws.amazon.com/apigateway/latest/developerguide/api-gateway-api-integration-types.html](https://docs.aws.amazon.com/apigateway/latest/developerguide/api-gateway-api-integration-types.html)

### Amazon API Gateway HTTP APIs
HTTP APIs are designed for low-latency, cost-effective AWS Lambda proxy and HTTP proxy APIs. HTTP APIs support OIDC and OAuth 2.0 authorization, and come with built-in support for CORS and automatic deployments. Previous-generation REST APIs currently offer more features, and full control over API requests and responses.

Read about how to choose between REST and HTTP APIs here: [https://docs.aws.amazon.com/apigateway/latest/developerguide/http-api-vs-rest.html](https://docs.aws.amazon.com/apigateway/latest/developerguide/http-api-vs-rest.html)