---
title: Connecting to the RIT VPN under Linux
layout: default


---

## Using Network-manager
These instructions are for network-manager specifically, but should be adaptable for any system

Step 1:
=======

Navigate to [https://vpn.rit.edu](https://vpn.rit.edu/) from a linux device and sign in

![](/rit-linux-wiki/img/rit-vpn/download-login.png)

Step 2:
=======

Click the button to download the anyconnect client for linux. You should get a roughly 5-6 megabyte `.sh` script file.

![](/rit-linux-wiki/img/rit-vpn/download.png)

Step 3: Extract
===============

This installer script contains an encoded tarball which makes up the bulk of the file. To extract it, use [this script](https://gist.github.com/MoralCode/0e6d3515cc6546ada033a504d95c79f7).


Step 4: Find the certificate
============================

Locate and save the file named `VeriSignClass3PublicPrimaryCertificationAuthority-G5.pem`. You will need this to connect.

Step 5: Set up connection
=========================

Install the package `network-manager-openconnect` from your system's package manager (it should be in at least the ubuntu repos). This adds openconnect (and anyconnect) options to network manager (if your system uses it).

Then go into network manager and create a new vpn profile.

![](/rit-linux-wiki/img/rit-vpn/settings.png)

Change the VPN protocol to "Cisco AnyConnect" and provide the gateway URL (vpn.rit.edu) and certificate file from earlier, you shouldnt need to do anything else.

Step 6: Connect
===============

In network manager, connect to the VPN. You should get a window like this:

![](/rit-linux-wiki/img/rit-vpn/connecting.png)

To continue connecting, click on the button to the right of the "VPN Host" Dropdown. It may ask for your RIT credentials before connecting you to the VPN.
