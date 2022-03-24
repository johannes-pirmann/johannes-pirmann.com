+++
author = "Johannes Pirmann"
categories = ["aws", "Getting Started"]
date = 2021-06-16T15:34:57Z
description = ""
draft = false
thumbnail = "https://images.unsplash.com/photo-1534190239940-9ba8944ea261?crop=entropy&cs=tinysrgb&fit=max&fm=jpg&ixid=MnwxMTc3M3wwfDF8c2VhcmNofDJ8fHRvb2xzfGVufDB8fHx8MTYyMzg2NDg2OQ&ixlib=rb-1.2.1&q=80&w=2000"
slug = "configure-your-aws-development-environment"
title = "Configure your AWS development environment"

+++


This article covers the basic steps which you need to be able to develop with AWS on your local machine.

1. [Install the AWS CLI](https://aws.amazon.com/de/cli/)
2. [Create an AWS IAM user](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_users_create.html#id_users_create_console)
3. Configure your development environment
4. [Install the AWS CDK](https://docs.aws.amazon.com/cdk/latest/guide/getting_started.html#getting_started_install)
5. Install the AWS SDK for Python

### Configure your development environment

Now run the following command to configure your development environment and insert your corresponding values:

```shell
aws configure
```

### Install the AWS SDK for Python

```shell
pip3 install boto3
```



