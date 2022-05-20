---
title: Connecting to the RIT eduroam network under Linux
layout: default


---
## An Overview

Starting after 2022 commencement RIT is replacing the 'RIT' wifi network with a larger, multi-university 'eduroam' network. This can lead to some confusion, especially for anyone not running MacOS or Windows.

## Purpose of this Guide
This guide aims to provide Linux users at RIT with an alternate to the [official RIT Helpdesk guidance](https://help.rit.edu/sp/?id=kb_article_view&sysparm_article=KB0040935) for users who want more manual control over their connection than is provided by the SecureW2 utility that RIT's instructions guide people to.

## General Steps

This guide contains several methods, each for a different purpose. At the bottom of this page, there are links to specific instructions for each currently supported networking tools. However, as a general rule, these are the high-level steps. This may be useful for creating additional information for your preferred network manager, should it not currently be documented. 

1. Connect to `RIT-WiFi` using traditional means. This allows you to download the certificates needed later in the process.
2. Open your browser of choice, and go to [https://rit.edu/wifi](https://rit.edu/wifi). Once loaded, click on the `eduroam` options.
3. Choose `User-defined`, and sign in with your RIT account. 
4. Enter the name of your device, or some description thereof, then enter a passphrase. This will be referred to as your certificate passphrase later in this tutorial.
5. Download the correct certificate for your setup, then download the RIT CA Certificate. 
6. Close the browser, and configure your network manager with the following settings.
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
7. Optionally, test your network connection.

## Network Management Options

- [Network Manager](./eduroamConfigs/networkManager.md)
- [nmcli/Network Manager CLI](./eduroamConfigs/nmcli.md)
- [iwd](./eduroamConfigs/iwd.md)
- [wpa_supplicant](./eduroamConfigs/wpa_supplicant.md)


## Alternate - RIT-WiFi

If you are unable to connect using certificates, you can follow the [ITS RESNet manual registration instructions](https://www.rit.edu/its/resnet/manual-registration).

## See Also

- [Connecting a Raspberry Pi to UVA's Eduroam Wifi](https://scholarslab.lib.virginia.edu/blog/raspberry-pi-uva-eduroam/)
- [Alternate Raspberry Pi information](https://s55ma.radioamater.si/2020/10/28/raspberry-pi-eap-tls-wi-fi-with-nmcli-network-manager/)
- [wpa_supplicant's official documentation](https://w1.fi/wpa_supplicant)
- [iwd's official autotests](https://git.kernel.org/pub/scm/network/wireless/iwd.git/tree/autotests)
