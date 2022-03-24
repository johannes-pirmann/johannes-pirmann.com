+++
author = "Johannes Pirmann"
categories = ["raspberry-pi", "relay", "api", "electronics", "python", "flask"]
date = 2021-06-01T16:26:57Z
description = ""
draft = false
thumbnail = "/images/2021/07/raspberry_pi-relay_controller.jpg"
slug = "control-a-relay"
title = "Control a Relay with a Raspberry Pi and Python"

+++


With this project I wanted to control a motor over the internet. The motor needs 12-24V DC with 1500mA. It is started through an impulse with the same potential as the power supply.

I chose a Raspberry Pi model B for this project, as I had one laying around. As OS I used Raspberry Pi OS Lite on a 8GB SD Card. To control the motor I created a simple Flask API.

## Hardware

* Raspberry Pi Model B
* [8GB SD Card](https://www.amazon.de/SanDisk-SDSDB-8192-BULK-Secure-Digital-Speicherkarte/dp/B000UZL0YU/ref=sr_1_3?__mk_de_DE=%C3%85M%C3%85%C5%BD%C3%95%C3%91&crid=34QMCF02EMGAH&dchild=1&keywords=sandisk+8gb+sd+karte&qid=1622633571&sprefix=san+disk+8gb+s%2Caps%2C156&sr=8-3)
* [5V Relay](https://www.az-delivery.de/products/relais-modul?variant=8138392109152)
* ethernet cable or [wifi dongle](https://www.amazon.de/AVM-MU-MIMO-Dualband-WLAN-5-GHz-Verbindungen-Multi-User-MIMO/dp/B074H1GDBJ/ref=sr_1_5?__mk_de_DE=%C3%85M%C3%85%C5%BD%C3%95%C3%91&crid=2RXNVJN032HUP&dchild=1&keywords=fritz+wlan+usb+stick+n+v2&qid=1622633494&sprefix=fritz+wlan+usb%2Caps%2C157&sr=8-5)
* power supply

### Demo

You can see the relay being control through the flashing red led and by the click sound.

<iframe  src="https://www.youtube.com/embed/Ih_BLnO3msk?feature=oembed" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

## Circuit Diagram

<img src="/images/2021/07/diagram.png">


## Code

```python
import RPi.GPIO as GPIO
from flask import Flask, redirect, url_for
import time

relayPin = 15  # Define the pin to be controlled

GPIO.setmode(GPIO.BCM)  # Choose BCM or BOARD
GPIO.setup(relayPin, GPIO.OUT)  # Set pin as an output
GPIO.output(relayPin, False)  # set pin value to 0/GPIO.Low/False

app = Flask(__name__)


@app.route('/', methods=['GET'])
def home():
    return "<h1>Hello</h1>"


@app.route('/success/<message>')
def success(message):
    return message


@app.route('/open_door', methods=['POST'])
def open_door():
    try:
        GPIO.output(relayPin, True)  # set pin value to 1/GPIO.High/True
        time.sleep(0.2)
        GPIO.output(relayPin, False)  # set pin value to 0/GPIO.Low/False
    except KeyboardInterrupt:
        GPIO.output(relayPin, False)  # set pin value to 0/GPIO.Low/False
        GPIO.cleanup()
    return redirect(url_for('success', message='success'))


if __name__ == '__main__':
    # Make the Raspberry Pi accessible in the same network through ip
    app.run(host='0.0.0.0')
```

See [here](https://www.raspberrypi-spy.co.uk/2015/10/how-to-autorun-a-python-script-on-boot-using-systemd/) for how to run the api after booting the raspberry pi.

