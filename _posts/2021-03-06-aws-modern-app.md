---
layout:     post
title:      "Building Modern Python Applications on AWS - week three"
date:       2021-03-06 12:00:00
author:     "Jason Feng"
header-img: "img/tunnel-4427609_1920.jpg"
excerpt_separator: <!--more-->
tags:
    - AWS
    - Lambda
    - Python
---
This is week three course notes of [Building Modern Python Applications on AWS](https://www.coursera.org/learn/building-modern-python-applications-on-aws/home/welcome) on Coursera.
<!--more-->
### AWS Lambda
AWS Lambda is a compute service that lets you run code without provisioning or managing servers. AWS Lambda executes your code only when needed and scales automatically, from a few requests per day to thousands per second. You pay only for the compute time you consume - there is no charge when your code is not running. With AWS Lambda, you can run code for virtually any type of application or backend service - all with little to no administration in regards to environment provisioning and scaling.

Read the Developer Guide for AWS Lambda here: [https://docs.aws.amazon.com/lambda/latest/dg/getting-started.html](https://docs.aws.amazon.com/lambda/latest/dg/getting-started.html)

### The Lambda Execution Environment
When AWS Lambda executes your Lambda function, it provisions and manages the resources needed to run your Lambda function. When you create a Lambda function, you specify configuration information, such as the amount of memory and maximum execution time that you want to allow for your Lambda function. 

It takes time to set up an execution context and do the necessary "bootstrapping", which adds some latency each time the Lambda function is invoked. You typically see this latency when a Lambda function is invoked for the first time or after it has been updated because AWS Lambda tries to reuse the execution context for subsequent invocations of the Lambda function.

After a Lambda function is executed, AWS Lambda maintains the execution context for some time in anticipation of another Lambda function invocation. In effect, the service freezes the execution context after a Lambda function completes, and thaws the context for reuse, if AWS Lambda chooses to reuse the context when the Lambda function is invoked again. This execution context reuse approach has the following implications:

**Objects declared outside of the function's handler method remain initialized, providing additional optimization when the function is invoked again.** For example, if your Lambda function establishes a database connection, instead of reestablishing the connection, the original connection is used in subsequent invocations. We suggest adding logic in your code to check if a connection exists before creating one.

Each execution context provides 512 MB of additional disk space in the /tmp directory. The directory content remains when the execution context is frozen, providing transient cache that can be used for multiple invocations. You can add extra code to check if the cache has the data that you stored. For information on deployment limits, see AWS Lambda limits.

Background processes or callbacks initiated by your Lambda function that did not complete when the function ended resume if AWS Lambda chooses to reuse the execution context. You should make sure any background processes or callbacks in your code are complete before the code exits.

Read more about the Lambda execution context here: [https://docs.aws.amazon.com/lambda/latest/dg/runtimes-context.html](https://docs.aws.amazon.com/lambda/latest/dg/runtimes-context.html)

### Lambda Permissions
There are two types of permissions to be aware of when working with AWS Lambda. The first type is **Execution Permissions**. A lambda function’s execution permissions are controlled by an IAM Role. When you create a lambda function you associate an IAM Role with the function. This Role will contain policies that either allow or deny specific API calls to be made. In order for the code running in the lambda function to make calls to AWS APIs the execution role must contain the permissions for those API calls. Read more about execution permissions by clicking here: [https://docs.aws.amazon.com/lambda/latest/dg/lambda-intro-execution-role.html](https://docs.aws.amazon.com/lambda/latest/dg/lambda-intro-execution-role.html) 

The second type of permission to be aware of with AWS Lambda is **resource-based policies**. Resource-based policies are used to control access to invoking and managing a lambda function. Resource-based policies let you grant usage permission to other accounts on a per-resource basis. You also use a resource-based policy to allow an AWS service to invoke your function.

For Lambda functions, you can grant an account permission to invoke or manage a function. You can add multiple statements to grant access to multiple accounts, or let any account invoke your function. To read more about resource-based permissions click here: [https://docs.aws.amazon.com/lambda/latest/dg/access-control-resource-based.html](https://docs.aws.amazon.com/lambda/latest/dg/access-control-resource-based.html)

### Lambda Execution
When your code executes in Lambda, the service will spin up a micro-virtual machine that will download your code and all the dependencies required and then execute it. The technology used for those micro-VMs is called Firecracker which is an open source virtualization technology. AWS created this technology with Lambda in mind as well as for multi-tenant container which is being used in the AWS Fargate service. By open-sourcing it, we now allow anyone to contribute and expand the capabilities of this technology. It also allows anyone to use this to build their own version of Lambda on-premises if you really wanted to. You can find more information about this technology here:   

[https://firecracker-microvm.github.io/](https://firecracker-microvm.github.io/)

### AWS Lambda Push/Pull Model
There are multiple ways to invoke lambda functions. You can invoke the function directly by calling the invoke API as shown in the demos, or you can setup triggers to invoke the lambda function. 

Read about how other AWS Services integrate with AWS Lambda here: [https://docs.aws.amazon.com/lambda/latest/dg/lambda-services.html](https://docs.aws.amazon.com/lambda/latest/dg/lambda-services.html)

When setting up a trigger, there are two main models the keep in mind. First the is the push model. The push model, is where a trigger sends an event to lambda, which then invokes your lambda function. When the push model is used, the resource-based policy must allow the trigger to invoke the lambda function.

The second model is the pull model. Some AWS Services being configured as triggers for lambda functions do not push events to lambda, but instead are the source of events, and AWS Lambda pulls the event from the event source, and then invokes the lambda function. A common example of a trigger that follows this model is Amazon Simple Queue Service (SQS). SQS allows messages to build up in a queue, and then your code would process those messages. Instead of having SQS invoke the lambda every time it gets a message, the messages build up in the queue and you define an Event Source Mapping as the trigger for the lambda function.

AWS Lambda will pull the events from SQS to the lambda function. You can configure how many messages to pull and at what frequency. Your code does not need to handle polling the queue for messages, instead the lambda service does that for you and passes the messages to your code through it’s event.

Read more about Event Source Mappings here: [https://docs.aws.amazon.com/lambda/latest/dg/invocation-eventsourcemapping.html](https://docs.aws.amazon.com/lambda/latest/dg/invocation-eventsourcemapping.html)

### Asynchronous vs Synchronous Lambda Functions
When invoking a Lambda function via the CLI or the SDK, the type of invocation (synchronous or asynchronous) can be selected based on the API call used. When using an AWS service to trigger the Lambda function, the type of invocation is typically decided by the trigger configured. For example, S3 invokes Lambda asynchronously. 

However, there are certain services that allows the type of invocation to be selected. Amazon API Gateway is one of those. The default setting is for API Gateway to invoke Lambda synchronously. For most REST API operations, the client will want a response from the Lambda function so it makes sense to have Lambda executed synchronously during those times. In the case of a long-latency operation, it would make sense to invoke it asynchronously and use another mechanism to let the client know that the operation is completed. That’s why it’s possible to choose how to invoke Lambda in API Gateway. However, this option is not available when using the Lambda proxy feature of API Gateway. Lambda must be configured with a custom integration. Then, the method execution can be configured to invoke the Lambda function asynchronously using the ‘X-Amz-Invocation-Type’ header set to ‘Event’ in the Integration Request. Or the client can decide which type of invocation to use by setting that header in the request to API Gateway. More information can be found here:

[https://docs.aws.amazon.com/apigateway/latest/developerguide/set-up-lambda-integration-async.html](https://docs.aws.amazon.com/apigateway/latest/developerguide/set-up-lambda-integration-async.html)

### Security and Compliance with AWS Lambda
You must remember to apply the shared responsibility model when using AWS Lambda. The following documentation shows you how to configure Lambda to meet your security and compliance objectives: [https://docs.aws.amazon.com/lambda/latest/dg/lambda-security.html](https://docs.aws.amazon.com/lambda/latest/dg/lambda-security.html)

### Creating a Python Lambda Function
You can run Python code in AWS Lambda. Lambda provides runtimes for Python that execute your code to process events. Your code runs in an Amazon Linux environment that includes AWS credentials from an AWS Identity and Access Management (IAM) role that you manage.

Read more about Python Lambda Functions at: [https://docs.aws.amazon.com/lambda/latest/dg/lambda-python.html](https://docs.aws.amazon.com/lambda/latest/dg/lambda-python.html)

Your Lambda function's handler is the method in your function code that processes events. When your function is invoked, Lambda runs the handler method. When the handler exits or returns a response, it becomes available to handle another event. Read about handlers here: [https://docs.aws.amazon.com/lambda/latest/dg/python-handler.html](https://docs.aws.amazon.com/lambda/latest/dg/python-handler.html)

To create a Python Lambda Function you need to write your code and then package it up into a deployment package. A deployment package is a ZIP archive that contains your function code and dependencies. You can upload the package directly to Lambda, or you can use an Amazon S3 bucket, and then upload it to Lambda. If the deployment package is larger than 50 MB, you must use Amazon S3.

Read about how to create a deployment package for a Python Lambda Function at: [https://docs.aws.amazon.com/lambda/latest/dg/python-package.html](https://docs.aws.amazon.com/lambda/latest/dg/python-package.html)

Read about how to create an AWS Lambda function with the AWS Toolkit for Pycharm at: 

[https://docs.aws.amazon.com/toolkit-for-jetbrains/latest/userguide/create-new-lambda.html](https://docs.aws.amazon.com/toolkit-for-jetbrains/latest/userguide/create-new-lambda.html)

Read about running and debugging an AWS Lambda function with the AWS Toolkit for Pycharm at: [https://docs.aws.amazon.com/toolkit-for-jetbrains/latest/userguide/invoke-lambda.html](https://docs.aws.amazon.com/toolkit-for-jetbrains/latest/userguide/invoke-lambda.html)

### Versioning and Aliases
AWS Lambda versions and aliases can help to provide a lot of control when managing your function code. Make sure you understand their base concepts to best utilize these features.  

You can publish a new version of your AWS Lambda function when you create new or update existing functions. Each version of a lambda function gets it’s own unique Amazon Resource Name (ARN). You can then use these ARNs to use different versions of the Lambda function for different purposes.  

You also have the ability to create aliases for AWS Lambda functions. Aliases are essentially pointers to one specific Lambda version.

Each alias you create has a unique ARN. An alias can only point to one function version, not to another alias. You can update an alias to point to a new version of the function.

This is handy for a number of reasons, one being the following: consider event sources such as Amazon S3 which can be configured to invoke your Lambda function. These event sources maintain a mapping that identifies the function to invoke when events occur. If you specify a Lambda function alias in the mapping configuration, you don't need to update the mapping when the function version changes.

Read about versioning here: [https://docs.aws.amazon.com/lambda/latest/dg/configuration-versions.html](https://docs.aws.amazon.com/lambda/latest/dg/configuration-versions.html)

Read about aliases here: [https://docs.aws.amazon.com/lambda/latest/dg/configuration-aliases.html](https://docs.aws.amazon.com/lambda/latest/dg/configuration-aliases.html)