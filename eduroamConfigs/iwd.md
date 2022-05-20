---
title: Connecting to the RIT eduroam network under Linux - iwd
layout: default


---
## iwd Tutorial

This is the complete instruction list for configuring `iwd`. This was tested on a clean Arch install ISO. File location may differ across distributions. If directories don't exist, check your distribution's documentation for the correct location of the files. 

## Requirements
The following is required to use this configuration:
- `iwd`/`iwctl`, and a terminal emulator to run commands in
- A modern web browser
- A `PEM` certificate (described below in steps 1-8)
- Root/`sudo` priveleges
- A text editor. Terminal emulators are easiest for this, but are not strictly necessary
    - `nano`
    - `micro`
    - `nvim`/`vim`/`vi`
    - `emacs`

## Instructions

All commands starting with `$` can be used as your standard user. All commands using `#` require to be run as root, generally by preceding the command with `sudo`. 

1. Connect to the `RIT-WiFi` network.

2. Go to [*https://rit.edu/wifi*](https://rit.edu/wifi), then select `eduroam`. 

![](/assets/img/eduroam/wifi-page.png)

3. In the 'Select your device' dropdown, select the `User-defined` option. 

![](/assets/img/eduroam/select-os.png)

4. Click 'Sign in' and enter your RIT credentials when prompted. 

![](/assets/img/eduroam/start-user-cert.png)

5. Once you are logged in, you will be asked to create a certificate. Enter a name you would like to give this certificate in the 'User Description' input box, then, click the 'Create' button. 

![](/assets/img/eduroam/create-user-cert.png)

6. You will then be prompted to create a password to protect your private key. This will be needed to set up eduroam on a new device. If you are unsure, you can use [1Password's public generator](https://1password.com/password-generator/?) which has a "memorable password" mode. The password you choose must be at least six (but no more than 16) characters long and contain a letter, number, and symbol. 

![](/assets/img/eduroam/password.png)

7. Choose a format to download your certificate in. Select PEM format and save the file somewhere you can get to it easily.

![](/assets/img/eduroam/cert-download.png)

8. Click the link on this page to save the RIT CA Cert. You'll also want to have this handy for the next step 

![](/assets/img/eduroam/root-ca.png)

9. Run the following command to determine what network interfaces you have available. This tutorial assumes a network interface of `wlan0`.
```
# ip link
```
This command will list all available network interfaces. `lo` (Loopback), `eth` (Ethernet) and `veth` (Virtual Ethernet) devices can all be ignored for the purposes of this tutorial. 

10. Move both the RIT CA Cert and the encrypted `.pem` file into the following directory: `/var/lib/iwd`. This can be done by running the following command:
```
# cp location/of/pemFile /var/lib/iwd/
```
This command must be run as root, as the default user does not have permission to create files in this directory.

11. In the same directory, create a configuration file named `eduroam.8021x`. In it, input the following information:
```
[Security]
EAP-Method=TLS
EAP-Identity=anonymous@rit.edu
EAP-TLS-CACert=/var/lib/iwd/ritCACert
EAP-TLS-ClientCert=/var/lib/iwd/encryptedCertKey.pem
EAP-TLS-ClientKey=/var/lib/iwd/encryptedCertKey.pem
EAP-TLS-ServerDomainMask=radius.rit.edu
EAP-TLS-Phase2-Method=Tunneled-PAP
EAP-TLS-Phase2-Identity=abc1234@rit.edu
EAP-TLS-Phase2-Password=P@ssw0rd1

[Settings]
AutoConnect=true
```
Replace `abc1234@rit.edu` and `P@ssw0rd1` with your RIT email and password. Ensure that the name of the `pem` file matches the name written in `eduroam.8021x`. *Do not change the `EAP-Identity` line*.

12. Run the following command as root: 
```
# iwctl station wlan0 connect eduroam
```
This tells `iwd` to connect to the `eduroam` WiFi network, using the network interface `wlan0`. This will prompt you for the password *to the `.pem` file you downloaded*. Enter that now.

13. (Optional) Run the following to ensure that your connection is working: 
```
ping 9.9.9.9
```
This command tests the ability to talk to the [Quad9 DNS Service](https://www.quad9.net/). If the command returns an error, then you are not connected to the `eduroam` network (or any other network, for that matter). 

## Troubleshooting
For any devices having issues, see [this article on the ArchWiki](https://wiki.archlinux.org/title/Iwd#Verbose_TLS_debugging) to start the debugging process. Common problems include:

- Entering an incorrect location for the certificates, or entering an invalid name for the certificates. This results in a `Not configured` on attempted connection.
- Entering `TTLS` instead of `TLS`. This results in a `journalctl` error shown below:
```
EAP server tried method 13 while client was configured for method 21
EAP completed with eapFail
4-Way handshake failed for ifindex 4, reason: 23
```