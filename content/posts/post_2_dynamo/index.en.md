---
weight: 1
title: "Basics API functions you need to know to work with DynamoDB on Ruby"
date: 2020-09-06T15:32:23+00:00
lastmod: 2020-09-06T15:32:23+00:00
draft: true
author: "Faizaan Ahmed Khan"
description: "I discuss some of the basic API functionality of DynamoDB in this post"
resources:
- name: "featured-image"
  src: "Featured.jpg"

tags: ["AWS", "Technical Articles", "DynamoDB"]
categories: ["Tech"]

lightgallery: true
---

## Intro
Hi, in this blog post I'm discussing some of the basic API functions which come in handy to use dynamoDB.
These are all well documented on Amazon documentation as well as the library which might use to integrate with AWS, So why
this post ? The reason for that is there's a lot of options, and to help you with what you need in the beginning here I list my opinions and my suggested way of doing it. Remember working on a DynamoDB is almost a no-brainer when you got your basics clear.
If you are a beginner and you know nothing about no SQL databases, I have a basic introduction post where I talk about most
of the if's and but's you need to know and a simple guide on the concepts DynamoDB [here](https://www.faizaankhan.com/post_1_dynamo/). Here my choice of language is Ruby, but it's an easy digest for any other as well.

For rails users, DynamoDB is not the right choice for ORM in my opinion, but you might have a different one. If you are looking for
using DynamoDB with your Active Record here's the best you got with this gem called [Dynamoid](https://github.com/Dynamoid/dynamoid).
If that's not what you need Dynamo for this is the right place.

## Setting up a local development environment
1. Download a locally executable DynamoDB application which you can use for development from [aws](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/DynamoDBLocal.DownloadingAndRunning.html)

2. Navigate to the folder and run `java -Djava.library.path=./DynamoDBLocal_lib -jar DynamoDBLocal.jar -help`, use any option, if you want to.

3. I use the inMemory flag, to not persist data locally and let it run just on memory, and the sharedDB for storing all region all work at one place, good for saving on some system requirements. The flags are well described [here](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/DynamoDBLocal.UsageNotes.html)
`java -Djava.library.path=./DynamoDBLocal_lib -jar DynamoDBLocal.jar -sharedDb -inMemory`

4. And Hurray, your local DB server is running.

5. Quickly download aws-cli if you don't have one. awscli provides you with the superpowers to communicate with aws from command line :p.
   1. Linux Users: `sudo apt install awscli`
   2. Mac Users: `brew install awscli` (Add Homebrew if you don't have it :))
   3. Alternatively, take some time and read about cli [here](https://aws.amazon.com/cli/)

6.
