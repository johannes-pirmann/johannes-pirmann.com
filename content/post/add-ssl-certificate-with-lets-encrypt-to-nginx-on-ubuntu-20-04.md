+++
author = "Johannes Pirmann"
categories = ["SSL", "Nginx", "Server Setup", "Security", "Ubuntu", "Let's Encrypt", "Ubuntu 20.04"]
date = 2021-09-29T06:24:38Z
description = ""
draft = false
thumbnail = "/images/2021/10/Add-SSL-to-Ubuntu.png"
slug = "add-ssl-certificate-with-lets-encrypt-to-nginx-on-ubuntu-20-04"
title = "Add SSL Certificate with Lets Encrypt to Nginx on Ubuntu 20.04"

+++


## Introduction

In this tutorial you will learn how to install **certbot** which communicates with Let's Encrypt. We will create an **SSL** certificate for our **example.com** domain and adapt the **Nginx** configuration as well as the **ufw** configuration.

> This article was first published at Hetzner Community: [https://community.hetzner.com/tutorials/add-ssl-certificate-with-lets-encrypt-to-nginx-on-ubuntu-20-04](https://community.hetzner.com/tutorials/add-ssl-certificate-with-lets-encrypt-to-nginx-on-ubuntu-20-04)

**Prerequisites**

* [Setup Ubuntu 20.04](/post/setup-ubuntu-20-04/)
* [Install Nginx on Ubuntu 20.04](/post/how-to-install-nginx-on-ubuntu-20-04/)
* A top-level domain, e.g. **example.com**

## Step 1 - Install Certbot

First, update your server:

```shell
holu@10.0.0.1:~$ sudo apt-get update && sudo apt-get upgrade -y
```

Then we are going to install **Certbot**:

```shell
holu@10.0.0.1:~$ sudo apt install certbot python3-certbot-nginx
```

## Step 2 - Allow HTTPS traffic

In the earlier tutorials we only allowed **HTTP** traffic through our firewall. So your **ufw** status should look like this:

```shell
holu@10.0.0.1:~$ sudo ufw status
Status: active

To                         Action      From
--                         ------      ----
OpenSSH                    ALLOW       Anywhere
Nginx HTTP                 ALLOW       Anywhere
OpenSSH (v6)               ALLOW       Anywhere (v6)
Nginx HTTP (v6)            ALLOW       Anywhere (v6)
```

Now we need to change this to also allow **HTTPS** traffic:

```shell
holu@10.0.0.1:~$ sudo ufw allow 'Nginx Full'
holu@10.0.0.1:~$ sudo ufw delete allow 'Nginx HTTP'
```

Our new **ufw** status looks like this:

```shell
holu@10.0.0.1:~$ sudo ufw status
Status: active

To                         Action      From
--                         ------      ----
OpenSSH                    ALLOW       Anywhere
Nginx Full                 ALLOW       Anywhere
OpenSSH (v6)               ALLOW       Anywhere (v6)
Nginx Full (v6)            ALLOW       Anywhere (v6)
```

## Step 3 - Get an SSL Certificate

In this step we will ask **certbot** to give us an **SSL** certificate. **Certbot** will also automatically adapt our **Nginx** configuration we previously set.

For this we need to run the following command:

```shell
holu@10.0.0.1:~$ sudo certbot --nginx -d example.com
```

> If you will use both **example.com** and **www.example.com** you need to tell it certbot in this step:

```shell
holu@10.0.0.1:~$ sudo certbot --nginx -d example.com -d www.example.com
```

### Step 3.1 - Certbot Prompt

**Certbot will prompt you with the following:**

```shell
Saving debug log to /var/log/letsencrypt/letsencrypt.log
Plugins selected: Authenticator nginx, Installer nginx
Enter email address (used for urgent renewal and security notices) (Enter 'c' to
cancel):
```

Specify an email that you check regularly, as Let's Encrypt will warn you by email about expiring certificates.

```shell
Please read the Terms of Service at
https://letsencrypt.org/documents/LE-SA-v1.2-November-15-2017.pdf. You must
agree in order to register with the ACME server at
https://acme-v02.api.letsencrypt.org/directory
- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
(A)gree/(C)ancel: A

- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
Would you be willing to share your email address with the Electronic Frontier
Foundation, a founding partner of the Let's Encrypt project and the non-profit
organization that develops Certbot? We'd like to send you email about our work
encrypting the web, EFF news, campaigns, and ways to support digital freedom.
- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
(Y)es/(N)o:
```

Here you need to accept the terms of service and choose whether you want to subscribe to the newsletter of EFF.

```shell
Obtaining a new certificate
Performing the following challenges:
http-01 challenge for example.com
Waiting for verification...
Cleaning up challenges
Deploying Certificate to VirtualHost /etc/nginx/sites-enabled/example.com

Please choose whether or not to redirect HTTP traffic to HTTPS, removing HTTP access.
- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
1: No redirect - Make no further changes to the webserver configuration.
2: Redirect - Make all requests redirect to secure HTTPS access. Choose this for
new sites, or if you're confident your site works on HTTPS. You can undo this
change by editing your web server's configuration.
- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
Select the appropriate number [1-2] then [enter] (press 'c' to cancel):
```

For new sites you should choose **2**, but choose according to your needs.

```shell
Redirecting all traffic on port 80 to ssl in /etc/nginx/sites-enabled/example.com

- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
Congratulations! You have successfully enabled https://example.com

You should test your configuration at:
https://www.ssllabs.com/ssltest/analyze.html?d=example.com
- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -

IMPORTANT NOTES:
 - Congratulations! Your certificate and chain have been saved at:
   /etc/letsencrypt/live/example.com/fullchain.pem
   Your key file has been saved at:
   /etc/letsencrypt/live/example.com/privkey.pem
   Your cert will expire on 2021-11-20. To obtain a new or tweaked
   version of this certificate in the future, simply run certbot again
   with the "certonly" option. To non-interactively renew *all* of
   your certificates, run "certbot renew"
 - Your account credentials have been saved in your Certbot
   configuration directory at /etc/letsencrypt. You should make a
   secure backup of this folder now. This configuration directory will
   also contain certificates and private keys obtained by Certbot so
   making regular backups of this folder is ideal.
 - If you like Certbot, please consider supporting our work by:

   Donating to ISRG / Let's Encrypt:   https://letsencrypt.org/donate
   Donating to EFF:                    https://eff.org/donate-le
```

You just obtained an **SSL** certificate!

Your new **Nginx** config should look as follows:

```config
server {

  root /var/www/example.com/html;
  index index.html;

  server_name example.com;

  location / {
    try_files $uri $uri/ =404;
  }

    listen [::]:443 ssl ipv6only=on; # managed by Certbot
    listen 443 ssl; # managed by Certbot
    ssl_certificate /etc/letsencrypt/live/example.com/fullchain.pem; # managed by Certbot
    ssl_certificate_key /etc/letsencrypt/live/example.com/privkey.pem; # managed by Certbot
    include /etc/letsencrypt/options-ssl-nginx.conf; # managed by Certbot
    ssl_dhparam /etc/letsencrypt/ssl-dhparams.pem; # managed by Certbot

}
server {
    if ($host = example.com) {
        return 301 https://$host$request_uri;
    } # managed by Certbot


  listen 80;
  listen [::]:80;

  server_name example.com;
    return 404; # managed by Certbot


}
```

## Conclusion

We have installed **certbot** which communicates with Let's Encrypt and installed an **SSL** certificate. **Certbot** also configured **Nginx** correctly to use our new **SSL** certificate. We also adapted the **ufw** firewall to let both **HTTP** and **HTTPS** traffic through.

