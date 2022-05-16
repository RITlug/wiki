---
title: Connecting to the RIT eduroam network under Linux
layout: default


---
## An Overview

Starting after 2022 commencement RIT is replacing the 'RIT' wifi network with a larger, multi-university 'eduroam' network. This can lead to some confusion, especially for anyone not running MacOS or Windows.

## Purpose of this guide
This guide aims to provide Linux users at RIT with an alternate to the [official RIT Helpdesk guidance](https://help.rit.edu/sp/?id=kb_article_view&sysparm_article=KB0040935) for users who want more manual control over their connection than is provided by the SecureW2 utility that RIT's instructions guide people to.

## Required Programs

### GUI Setup
- NetworkManager and a desktop environment that uses it (KDE, GNOME, LXDE, Cinnamon, MATE)
- Terminal emulator
- Root/`sudo` permissions

### CLI Setup
- `wpa_supplicant`, `iwd` <!--, or NetworkManager/`nmcli`-->
- Terminal emulator
- Root/`sudo` permissions
<!--
- `openssl` or similar
    this line may not be necessary... wpa_supplicant seems to have a way to open encrypted cert/key files. Will explore further.-->
- Some text editor (terminal text editors such as `micro`, `nvim`/`vim`, or `emacs` are easiest for this purpose, but not strictly necessary)

## Initial Setup

Initial setup requires internet access to download certificates. This can be done on campus by connecting to the `RIT-WiFi` network.

1. Go to [*https://rit.edu/wifi*](https://rit.edu/wifi), then select `eduroam`. ![](/assets/img/eduroam/wifi-page.png)

2. In the 'Select your device' dropdown, select the `user-defined` option. ![](/assets/img/eduroam/select-os.png)

3. Click 'Sign in' and enter your RIT credentials when prompted. ![](/assets/img/eduroam/start-user-cert.png)

4. Once you are logged in, you will be asked to create a certificate. Enter a name you would like to give this certificate in the 'User Description' input box, then, click the 'Create' button. ![](/assets/img/eduroam/create-user-cert.png)

5. You will then be prompted to create a password to protect your private key. This will be needed to set up eduroam on a new device. If you are unsure, you can use [1Password's public generator](https://1password.com/password-generator/?) which has a "memorable password" mode. The password you choose must be at least six (but no more than 16) characters long and contain a letter, number, and symbol. ![](/assets/img/eduroam/password.png)

6. Choose a format to download your certificate in. If you aren't sure what you need, select P12 format and save the file somewhere you can get to it easily. If you are using `iwd` and the terminal-only setup, select the PEM option. ![](/assets/img/eduroam/cert-download.png)

7. Click the link on this page to save the RIT CA Cert. You'll also want to have this handy for the next step ![](/assets/img/eduroam/root-ca.png)

8. If you are using the GUI setup process, copy the RIT certificate file to `/usr/local/share/ca-certificates/`. As this must be done as root, this will most likely require a terminal. A command like `sudo cp *.cer /usr/local/share/ca-certificates/` will copy all files ending in .cer in your current folder to this location.

9. You now have everything you need to connect to the network.


## General Network Information

The process of connecting to the network may be different depending on the Linux distribution you use. In general, here is some information that may be asked during the connection process:

- *Identity:* Your RIT email address. i.e. [username]@rit.edu
- *Authentication:* TLS
- *Wifi Security:* WPA2 Enterprise
- *CA certificate:* The RIT `.cer` certificate you downloaded earlier.
- *User Certificate/private key:* The other file you downloaded (**not the .cer file**). Note that, depending on the file you download, the certificate and private key may or may not be separate.
- *User Key Password:* The password you set for the above file earlier.

## GUI Setup

<!-- Once confirmed all configurations are acceptable, this comment will be removed
Note that while this connection is functional, it is less secure, as it only checks that the client is safe for the server. The `wpa_supplicant` configuration instructions includes an authentication that the server is safe for the client as well. -->

### NetworkManager Configuration

1. To connect click the wifi logo in the status bar, and then select `eduroam`. ![](/assets/img/eduroam/open-networkmanager.png)

2. Enter the configuration details in the window that appears. If the password field turns red it means the password is incorrect. ![](/assets/img/eduroam/configure-networkmanager.png)

Your computer should now be connected to the `eduroam` network.

## CLI Setup

Note: Both the `iwd` and the `wpa_supplicant` configurations were tested on an otherwise-blank Arch install ISO. File location may differ across distributions. If directories don't exist, check your distribution's documentation for the correct location of the files.

<!-- Once confirmed all configurations are acceptable, this comment will be removed
This is the configuration recommended by ITS, as it authenticates both the client to the server, and the server to the client. While this method is more involved, it should work properly for systems without a GUI, as well as for systems that already have `wpa_supplicant` installed.
-->

### `iwd` Configuration

If you do not have a `.pem` file from RIT before starting this section, go back to the beginning of this page, and create a new certificate, downloading the `PEM` option instead.

1. Move both the RIT CA Cert and the encrypted `.pem` file into the following directory: `/var/lib/iwd`.
2. In the same directory, create a configuration file named `eduroam.8021x`. In it, input the following information:
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
Replace `abc1234@rit.edu` and `P@ssw0rd1` with your RIT email and password. *Do not change the `EAP-Identity` line*.
3. Run the following command as root: `iwctl station wlan0 connect eduroam`
4. The above command will prompt you for the password *to the `.pem` file you downloaded*. Enter that now.
5. Run the following to ensure that your connection is working: `ping 9.9.9.9`

For any devices having issues, see [this link](https://wiki.archlinux.org/title/Iwd#Verbose_TLS_debugging) to start the debugging process. Common problems include:

- Entering an incorrect location for the certificates, or entering an invalid name for the certificates. This results in a `Not configured` on attempted connection.
- Entering `TTLS` instead of `TLS`. This results in a `journalctl` error shown below:
```
EAP server tried method 13 while client was configured for method 21
EAP completed with eapFail
4-Way handshake failed for ifindex 4, reason: 23
```

### wpa_supplicant Configuration

If you do not have a `.p12` file from RIT before starting this section, go back to the beginning of this page, and create a new certificate, downloading the `P12` option instead.

1. Move both the RIT CA Cert and the encrypted `.p12` file into a common directory that you don't plan on interacting with much. For testing purposes, `/opt/` was used. Locations within `~` or `/home/$USER` may not work properly, due to improper permissions, although this is untested as of writing.
2. Open your text editor of choice and create a file in `/etc/wpa_supplicant`, and fill in the file according to the block below. You can name the file whatever you wish, but for automation purposes, its recommended to name the file `wpa_supplicant-[interface name].conf`. This makes it easier for `systemd` and `dhcpcd` to interface with `wpa_supplicant` once it's set up.
```
ctrl_interface=/var/run/wpa_supplicant
ctrl_interface_group=wheel
update_config=1

network={
    ssid="eduroam"
    scan_ssid=1
    key_mgmt=WPA-EAP
    proto=WPA2
    pairwise=CCMP
    group=CCMP
    eap=TLS
    anonymous_identity="anonymous@rit.edu"
    domain_suffix_match="rit.edu"
    identity="abc1234@rit.edu"
    ca_cert="/location/of/RIT/cert"
    private_key="location/of/p12PrivateKey.p12"
    private_key_passwd="P@ssw0rd1"
}
```

3. You're all set! Run the following commands in a terminal to start `wpa_supplicant`, then start `dhcpcd`. After this, your network should be set up properly.
```bash
sudo wpa_supplicant -B -i [interface name] -c /etc/wpa_supplicant/[config file name]
sudo dhcpcd [interface name]
```

For debugging purposes, the `-B` flag shown above can be omitted, allowing you to see the complete output of the command. Note that this omitting this flag will make the current terminal unusable, so a second session or terminal will be required to run the `dhcpcd` command. Also note that closing the terminal containing the `wpa_supplicant` command will also end the connection to the Wi-Fi, so act with care.


Note that while this config file is persistent across reboots, as this guide stands, you will need to start `wpa_supplicant` and `dhcpcd` manually on each boot. For more information on starting `wpa_supplicant` at boot, and auto-starting `dhcpcd`, consult the [ArchWiki](https://wiki.archlinux.org/title/Wpa_supplicant#At_boot_(systemd)), or your distribution's wiki for non-`systemd` systems.

## Alternate - RIT-Legacy

If you are unable to connect using certificates, you can follow the [ITS RESNet manual registration instructions](https://www.rit.edu/its/resnet/manual-registration).

## See Also

- [Connecting a Raspberry Pi to UVA's Eduroam Wifi][https://scholarslab.lib.virginia.edu/blog/raspberry-pi-uva-eduroam/)
- [Alternate Raspberry Pi information](https://s55ma.radioamater.si/2020/10/28/raspberry-pi-eap-tls-wi-fi-with-nmcli-network-manager/)
- [wpa_supplicant's official documentation](https://w1.fi/wpa_supplicant)
- [iwd's official autotests](https://git.kernel.org/pub/scm/network/wireless/iwd.git/tree/autotests)
