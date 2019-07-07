---
layout:     post
title:      "Build a serverless text to speech endpoint"
date:       2019-07-06 12:00:00
author:     "Jason Feng"
header-img: "img/post-bg-2015.jpg"
tags:
    - GCP
    - Cloud Function
    - Text-to-Speech
    - Faas
---

With cloud technologies getting more sophisticated nowadays, we are literally able to to have infinite storage, computing powers, and networking. Cloud is now abstracting most of the underlying infrastructure, Operation Systems, software applications, library packages and dependencies. Depending on the different levels of abstraction, we know about IaaS, PaaS, FaaS, SaaS, etc., all the fancy acronyms. Moreover, we can easily utilise all the advanced technologies owned by the big tech companies through the cloud, like computer vision, speech recognition, NLP. 

We just need to focus on the top levels of the development stack which is the business logic, and write the codes to implement the logic. We can quickly turn the ideas and prototypes into proof of concept without the need to setup a server, install the required software and packages, administrate the database. This can dramatically shorten the implementation period from weeks or days to just a couple of hours.

It only took me less than half a day to design and build an HTTP endpoint which converts the text contents in the request into synthesize natural-sounding speech.

Below are the steps of the solution.
<a name='cf-texttospeech'>![](/img/cf-texttospeech.JPG)</a>

1. Send an HTTP request to the endpoint with the text payload which will be converted to speech.
2. The cloud function receives the request. It will then call the Cloud Text-to-Speech API to convert the text to speech with the specific voice and language. Behind the scene, this is powered by DeepMind’s groundbreaking research in WaveNet and Google’s neural networks.
3. Upon receiving the response from Cloud Text-to-Speech API, the cloud function will save the audio as an MP3 file.
4. The cloud function with upload the MP3 file into a storage bucket.
5. The cloud function returns the URL of the audio file to the request.

The above implementation is serverless and event-driven, because I don't need to create any VM instance, install Python and all the relevant packages. The cloud function can automatically handle the scaling if the requests increase. And we only pay when the function is called. No money will be spent during idle time. 