+++
author = "Johannes Pirmann"
categories = ["electronics", "raspberry-pi"]
date = 2021-06-09T14:30:20Z
description = ""
draft = false
image = "https://images.unsplash.com/photo-1585146702843-45507b83e662?crop=entropy&cs=tinysrgb&fit=max&fm=jpg&ixid=MnwxMTc3M3wwfDF8c2VhcmNofDR8fHJhc3BiZXJyeXBpfGVufDB8fHx8MTYyMzI1NjIwMw&ixlib=rb-1.2.1&q=80&w=2000"
slug = "raspberry-pi-configuration"
title = "Basic Raspberry Pi Configuration for Headless Mode"

+++


## Enable SSH

Add an file to the boot partition called "**ssh**". This enables you to connect to the pi via ssh.

## Enable WiFi

To enable wifi on the raspberry pi add a file named "**wpa_supplicant.conf**" to the boot partition. Add following content to this file and adapt it to your needs.

```Shell
ctrl_interface=DIR=/var/run/wpa_supplicant GROUP=netdev
country=<Insert 2 letter ISO 3166-1 country code here>
update_config=1

network={
 ssid="<Name of your wireless LAN>"
 psk="<Password for your wireless LAN>"
}
```



