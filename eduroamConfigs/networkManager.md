---
title: Connecting to the RIT eduroam network under Linux - NetworkManager
layout: default


---
## NetworkManager Tutorial

This is the complete instruction list for configuring `wpa_supplicant`. These instructions use the `P12` certificate, although the `PEM` will also work properly. 

## Requirements
Currently, the only required software required for this is a modern web browser, and a desktop environment compatible with NetworkManager. Desktop managers that default to using NetworkManager include KDE, GNOME, LXDE, Cinnamon, and MATE.

## Instructions

1. Connect to the `RIT-WiFi` network.

2. Go to [*https://rit.edu/wifi*](https://rit.edu/wifi), then select `eduroam`. 

![](/assets/img/eduroam/wifi-page.png)

3. In the 'Select your device' dropdown, select the `user-defined` option. 

![](/assets/img/eduroam/select-os.png)

4. Click 'Sign in' and enter your RIT credentials when prompted. 

![](/assets/img/eduroam/start-user-cert.png)

5. Once you are logged in, you will be asked to create a certificate. Enter a name you would like to give this certificate in the 'User Description' input box, then, click the 'Create' button. 

![](/assets/img/eduroam/create-user-cert.png)

6. You will then be prompted to create a password to protect your private key. This will be needed to set up eduroam on a new device. If you are unsure, you can use [1Password's public generator](https://1password.com/password-generator/?) which has a "memorable password" mode. The password you choose must be at least six (but no more than 16) characters long and contain a letter, number, and symbol. 

![](/assets/img/eduroam/password.png)

7. Choose a format to download your certificate in. `P12` and `PEM` certificates are currently known to be compatible with NetworkManager.

![](/assets/img/eduroam/cert-download.png)

8. Click the link on this page to save the RIT CA Cert. 

![](/assets/img/eduroam/root-ca.png)

9. Click the wifi logo in the status bar, and then select `eduroam`. 

![](/assets/img/eduroam/open-networkmanager.png)

10. Enter the configuration details in the window that appears. If the password field turns red it means the password is incorrect. 

![](/assets/img/eduroam/configure-networkmanager.png)

## Troubleshooting 

If you cannot connect to the internet after attempting to use this tutorial, go to the [nmcli](./nmcli.md) documentation, and ensure that all settings are correct. There are also more troubleshooting steps listed there. 