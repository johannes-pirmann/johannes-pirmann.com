+++
author = "Johannes Pirmann"
categories = ["Ansible", "Ubuntu 20.04", "server", "Server Setup"]
date = 2021-10-29T18:39:12Z
description = ""
draft = false
slug = "setup-ubuntu-20-04-server-with-ansible"
tags = ["Ansible", "Ubuntu 20.04", "server", "Server Setup"]
title = "Setup Ubuntu 20.04 Server with Ansible"

+++


If you want a more hands on approach you can follow the following tutorial: [https://johannes-pirmann.com/setup-ubuntu-20-04/](__GHOST_URL__/setup-ubuntu-20-04/)

## Prerequisites

* An Ubuntu 20.04 Server with the LAMP-Stack installed.
* A root user and password
* A domain
* The Ansible Playbook: [https://github.com/Pirmann-Media/ansible-playbooks/tree/main/ubuntu-20-04-setup](https://github.com/Pirmann-Media/ansible-playbooks/tree/main/ubuntu-20-04-setup)

## Step 1 - Get the Playbook

Go to the [GitHub Repository](https://github.com/Pirmann-Media/ansible-playbooks/tree/main/ubuntu-20-04-setup) and download the ansible playbook.

## Step 2 - Adapt the provision.yml file

Go to the provision.yml file and set your desired username and the corresponding password. Additionally, specify the path to your public SSH key and the host which should be used.

## Step 3 - Execute the playbook

To execute the ansible playbook run the following command:

```shell
ansible-playbook provision.yml --ask-pass
```

Now you need to enter the root password.

