---
title: Connecting to the RIT eduroam network under Linux
layout: default


---
## An Overview

Starting after 2022 commencement RIT became part of a larger, multi-university 'eduroam' system of campus networks. 

This changes some things about authentication with the network and can lead to some confusion, especially for anyone not running MacOS or Windows, or for those who like to tinker with the in-dept technical details of their systems.

To learn more about what eduroam is and how it works, see the [What is Eduroam?](#what-is-eduroam) section of this page.

## Purpose of this Guide
This guide, first and foremost, aims to provide Linux users at RIT with alternatives to the [official RIT ITS Helpdesk guidance](https://help.rit.edu/sp/?id=kb_article_view&sysparm_article=KB0040935) and SecureW2 client. These guides are intended for linux users who are comfortable managing their own system and want more control and ability to tinker with their setup.

These guides are not official and ITS most likely won't provide support for the configurations and clients listed below. If you are looking to talk to someone about these instructions, the [RIT Linux Users Group](https://ritlug.com/) is the best place to reach out.

## General Steps

The next section of this page contains links to specific instructions for a variety of different networking clients. Most of these guides follow very similar high-level steps. This outline may be useful for anyone looking to understand the general process or create new guides for other clients.

1. [Generate a personal certificate](./certificates) (and a password to secure it) 
2. Configure your network manager with the following settings.
    - SSID/Network Name: eduroam
    - Key Management: WPA-EAP
    - EAP method: TLS
    - Anonymous Identity: anonymous@rit.edu
    - Domain: radius.rit.edu
    - Phase2/Inner Identity/"Identity": abc1234@rit.edu
    - Phase2/Inner Password: [Your RIT Password]
    - Location of the CACert
    - Location of the personal certificate and/or key
    - Certificate Password: created previously

## Network Management Options
If you are using a specific piece of client software on your linux machine, you may find one of these specific guides to be more useful. 

- [Network Manager](./networkManager)
- [nmcli/Network Manager CLI](./nmcli)
- [iwd](./iwd)
- [wpa_supplicant](./wpa_supplicant)
- [Android](./android)

### Alternate - RIT-WiFi

If you are unable to connect using certificates, you can follow the [ITS RESNet manual registration instructions](https://www.rit.edu/its/resnet/manual-registration).

## What is Eduroam?

Eduroam (https://http://eduroam.org/) is simply an authentication proxy on top of the existing RIT network that allows students from other universities to use the RIT network after securely authenticating with their home institution. It was started in Europe but has since expanded throughout the research community.

Essentially all eduroam does when you're trying to connect to another institution's network (say Intitution B) is proxy your authentication request to the correct authentication server. This request essentially says "I'm abc1234@rit.edu", eduroam's auth proxies say "I know where rit.edu's auth servers are" and forwards/proxies your login attempt to RIT's auth servers. RIT's auth servers then give an "approved" or "denied" response that determines whether you can access Intitution B's network.

### An analogy
You can think of this process as being similar to RIT's login portal. When you go to a non-RIT website such as Zoom or Gmail you're connecting to servers owned by a thid party. But when you tell that service "I'm from RIT" (by logging in with your RIT account) you get sent to the RIT Login page.

After logging in through the RIT portal, you get sent back to the service you came from. The service gets some data that indicates whether the login was successful in order to let you in, and from that point you are communicating directly with the service again.

Eduroam is essentially the same concept but for wifi. When trying to log in to Institution B's network with RIT credentials, your login request will get securely sent to RIT to confirm you are allowed to access the network, but once authenticated, you will be communicating directly with Institution B's network.

## See Also
- [What is eduroam](https://eduroam.org/what-is-eduroam/)
- [Connecting a Raspberry Pi to UVA's Eduroam Wifi](https://scholarslab.lib.virginia.edu/blog/raspberry-pi-uva-eduroam/)
- [Alternate Raspberry Pi information](https://s55ma.radioamater.si/2020/10/28/raspberry-pi-eap-tls-wi-fi-with-nmcli-network-manager/)

