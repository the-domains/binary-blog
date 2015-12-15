---
inFeed: true
hasPage: true
inNav: false
inLanguage: null
starred: false
keywords: []
description: 'The raw connections to the AWS API with PHP can be a bit tricky sometimes to get the right values. It is very sensitive and the slightest mistake can give you an authentication or signature error. '
datePublished: '2015-12-15T01:00:06.368Z'
dateModified: '2015-12-15T00:59:48.794Z'
title: PHP - AWS Raw API connections
author: []
authors: []
publisher:
  name: null
  domain: null
  url: null
  favicon: null
sourcePath: _posts/2015-12-15-php-aws-raw-api-connections.md
published: true
url: php-aws-raw-api-connections/index.html
_type: Article

---
# PHP - AWS Raw API connections
![](https://the-grid-user-content.s3-us-west-2.amazonaws.com/ae20a250-989c-4d02-9e8d-7fa31603610f.jpg)

**The raw connections to the AWS API with PHP can
be a bit tricky sometimes to get the right values. It is very sensitive
and the slightest mistake can give you an authentication or signature 
error. In this post I share code for the main connection methods. They 
have been minimized to only the necessary code so they are easy to 
understand and implement into your own application. Most services use 
very similar code so most of the below you can put into functions for 
repeat use between services.**

I have a connection function that handles the curl part and returning
a raw result. A function that builds a GET request and a function that 
builds a POST request. The only big exception is the S3 PUT request 
which is quite different and is a function on its own. I also have a 
separate builder function for S3 to build the different 
get/post/head/delete requests but once they are built they use the 
shared request function.

In the code below I have not used functions. Just simple raw 
procedural php code so you can decide how you want to structure it. I 
have also not included any connection error checking and retries which 
is very important with AWS communications. I have chosen to focus purely
on starting a connection and the right syntax to use for that.

## Authentication

The authentication method I use is AWS IAM Roles. These are temporary
credentials generated for your instances when they are launched and 
refreshed every 12 hours. You configure a role in your IAM manager, 
setting which services and methods your instance should have access to 
and then you can retrieve the credentials from the instance metadata 
like this:

You can get the credentials each time you use one of the API's to 
make sure you have the correct ones, or you can cache them. If you are 
only using 1 role for your instances then you could hard code the role 
name to avoid the first request. Once you have the auth credentials you 
can run your connection functions.

## The GET request

This is used for most services and is quite similar for each one.

## The POST request.

Like GET, this is pretty much the same between different services (not many services need this one though).

## S3

S3 is a little different in how it connects, it uses different types 
(GET/HEAD/POST/DELETE) of requests for different methods and a PUT that 
is done with sockets instead of curl. S3 curl request. Note that I have 
combined all 3 requests in the code below. You should pick the one 
suitable for your needs and remove the remaining code. Or make a 
function into which you pass the type of request and then use case or 
ifelse to execute the correct code for the selected type.

## S3 Put request

It has very little in common with the other functions. Note that I 
have included code for uploading a file, creating a bucket and putting a
lifecycle config, remove the code that isn't applicable to your needs 
or create a function with a case or if/else statement to include the 
correct code for the task at hand.