+++
author = "Johannes Pirmann"
date = 2021-06-16T16:39:48Z
categories = ["aws"]
description = ""
draft = false
thumbnail = "/images/2021/07/taylor-vick-M5tzZtFCOfs-unsplash--1-.jpg"
slug = "aws-scripts"
title = "AWS Scripts"

+++


In this article I list all code snippets I find/create regarding AWS. Most of those snippets work with the AWS SDK for python 'boto3'.

#### Prerequisites

Please follow this article to setup your AWS development environment.

# Index

* [Print all S3 buckets](#print-s3-buckets)

<br id="print-s3-buckets">
<br>

# Print all S3 buckets

```python
import boto3

s3 = boto3.resource('s3')

for bucket 
```



