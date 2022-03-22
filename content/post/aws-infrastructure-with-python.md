+++
author = "Johannes Pirmann"
categories = ["aws", "infrastructure", "python"]
date = 2021-06-16T14:35:20Z
description = ""
draft = true
image = "https://images.unsplash.com/photo-1531431057391-da7a1aabd412?crop=entropy&cs=tinysrgb&fit=max&fm=jpg&ixid=MnwxMTc3M3wwfDF8c2VhcmNofDZ8fGluZnJhc3RydWN0dXJlfGVufDB8fHx8MTYyMzg2MTYzNQ&ixlib=rb-1.2.1&q=80&w=2000"
slug = "aws-infrastructure-with-python"
title = "AWS Infrastructure with Python"

+++


### Prerequisites

Please follow [this article](__GHOST_URL__/configure-your-aws-development-environment/) to setup your [AWS development environment](__GHOST_URL__/configure-your-aws-development-environment/).

### Setting up the directory

In the terminal go to the location where you want to put this project and create a new directory with mkdir and switch into the new directory with cd.

```shell
sudo mkdir directory_name
cd directory_name
```

### Initialize the default CDK app

```shell
cdk init app --language python
```

### Creating a virtual environment for our dependencies

If you are using python 3.3 or newer than 'venv' is the prefered way. 'venv' is already included in python.Below python 3.3 you need 'virtualenv'. [Here](https://virtualenv.pypa.io/en/latest/installation.html) you can find the [installation guide](https://virtualenv.pypa.io/en/latest/installation.html) for virtualenv. I'm using python 3.9.5 therefore I will be using 'venv'.

With 'venv' you create a new virtual environment with the following command:

```shell
python3 -m venv name_of_your_virtual_environment
```

### Activate the virtual environment and install the dependencies

Now you can **activate** the virtual environment and install the dependencies with this command:

```shell
source name_of_your_virtual_environment/bin/activate
pip3 install -r requirements.txt

```

To **deactivate** the virtual environment just type:

```shell
deactivate
```



