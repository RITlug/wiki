---
title: Connecting to the RIT eduroam network under Linux
layout: default


---
## An Overview

Starting after 2022 commencement RIT is replacing the 'RIT' wifi network with a larger, multi-university 'eduroam' network. While this change may seem straightforward, RIT has decided to use EAP-TLS authentication rather than using MSCHAPV2. This is not a widespread practice, even among other universities who utilize the eduroam network. This can lead to some confusion, especially for anyone not running MacOS or Windows. This guide hopes to clear that up.

## Initial Setup

Initial setup requires internet access to download certificates. 

1. Go to [*https://rit.edu/wifi*](https://rit.edu/wifi), then select 'eduroam'. ![](/assets/img/eduroam/wifi-page.png)

2. In the 'select your device' dropdown, select the 'user-defined' option. ![](/assets/img/eduroam/select-os.png)

3. Click 'sign in' and enter your RIT credentials when prompted. ![](/assets/img/eduroam/start-user-cert.png)

4. Once you are logged in, you will be asked to create a certificate. Enter a name you would like to give this certificate in the 'User Description' input box, then, click the 'create' button. ![](/assets/img/eduroam/create-user-cert.png)

5. You will then be prompted to create a password to protect your private key. This will be needed to set up eduroam on a new device. If you are unsure, you can use [1Password's public generator](https://1password.com/password-generator/?) which has a "memorable password" mode. The password you choose must be at least six (but no more than 16) characters long and contain a letter, number, and symbol. ![](/assets/img/eduroam/password.png)

6. Choose a format to download your certificate in. If you aren't sure what you need, select P12 format and save the file somewhere you can get to it easily. If you are using `wpa_supplicant` and the terminal-only setup, select the PEM option. ![](/assets/img/eduroam/cert-download.png)

7. Click the link on this page to save the RIT CA Cert. You'll also want to have this handy for the next step ![](/assets/img/eduroam/root-ca.png)

8. In a terminal, navigate to the directory you saved the certificates to. Copy the RIT certificate file to '/usr/local/share/ca-certificates/'. A command like `sudo cp *.cer /usr/local/share/ca-certificates/` will copy all files ending in .cer in your current folder to this location.

9. You now have everything you need to connect to the network


## General Network Information

The process of connecting to the network may be different depending on the Linux distribution you use. In general, here is some information that may be asked during the connection process:

- *Identity:* Your RIT email address. i.e. [username]@rit.edu
- *Authentication:* TLS
- *Wifi Security:* WPA and WPA2 Enterprise
- *CA certificate:* The RIT .cer certificate you downloaded earlier.
- *User Certificate/private key:* The other file you downloaded (**not the .cer file**). Note that, depending on the file you download, the certificate and private key may or may not be separate.
- *User Key Password:* The password you set for the above file earlier.


### NetworkManager (GUI) Setup

1. To connect click the wifi logo in the status bar, and then select 'eduroam' ![](/assets/img/eduroam/open-networkmanager.png)

2. Enter the configuration details in the window that appears. If the password field turns red it means the password is incorrect. ![](/assets/img/eduroam/configure-networkmanager.png)

Once these instructions are complete, you will be able to connect to the
new eduroam setup without needing to run any scripts!

### wpa_supplicant (CLI) Setup

Not all systems are built for GUIs, or have NetworkManager installed. This method, while more involved, should work properly for these types of systems.

If you do not have a `.pem` file from RIT before starting this section, go back to the beginning of this page, and create a new certificate, downloading the `PEM` option instead.

1. Open the downloaded `.pem` file in the text editor of your choice. You will notice that this contains two separate blocks, an encrypted private key, and a certificate. These are both crucial, but they cannot be used in their current state for multiple reasons. To make these work properly with `wpa_supplicant`, copy each block, including the `BEGIN` and `END` lines to their own separate files. I called these files `personalCert.pem` and `personalKey.pem`.

2. Once you have these two files, exit your text editor and open the folder where the files created in step 1 are in a terminal. Currently, both the certificate and key are encrypted, and while `wpa_supplicant` can work with them in their current state, it's not guaranteed. To ensure that they can be used properly, run the following commands. This will create two new files, which are decrypted copies of the certificate and key.
```bash
sudo openssl pkey -in personalKey.pem -out decryptedKey.pem
sudo openssl x509 -in personalCert.pem -out decryptedCert.pem
```
The first of these commands will ask for a password. Enter the password of the file.

3. Now that these files are decrypted, we can create the configuration file for `wpa_supplicant`. Open your text editor of choice and create a file in `/etc/wpa_supplicant`, and fill in the file according to the block below. You can name the file whatever you wish, but for automation purposes, its recommended to name the file `wpa_supplicant-[interface name].conf`. This makes it easier for `systemd` and `dhcpcd` to interface with `wpa_supplicant` once it's set up.
```
ctrl_interface=/var/run/wpa_supplicant
ctrl_interface_group=wheel
update_config=1
network={
    ssid="eduroam"
    scan_ssid=1
    key_mgmt=WPA-EAP
    eap=TLS
    identity="abc1234@rit.edu"
    ca_cert="/location/of/RIT/cert"
    client_cert="location/of/decryptedCert.pem"
    private_key="location/of/decryptedKey.pem"
}
```

4. You're all set! Run the following commands in a terminal to start `wpa_supplicant`, then start `dhcpcd`. After this, your network should be set up properly. 
```bash
sudo wpa_supplicant -i [interface name] -c /etc/wpa_supplicant/[config file name]
sudo dhcpcd [interface name]
```
Note that while this config file is persistent across reboots, as this guide stands, you will need to start `wpa_supplicant` and `dhcpcd` manually on each boot. For more information on starting `wpa_supplicant` at boot, and auto-starting `dhcpcd`, consult the [ArchWiki](https://wiki.archlinux.org/title/Wpa_supplicant#At_boot_(systemd)), or your distribution's wiki for non-`systemd` systems.


## Alternate - RIT-Legacy

If you are unable to connect using certificates, you can follow the [ITS RESNet manual registration instructions](https://www.rit.edu/its/resnet/manual-registration).


<!--
## Getting the config

1. Create a temporary directory somewhere and cd into it from a terminal
2. download the linux installer from https://rit.edu/wifi. Dont open it since it might break the CRLF line endings and the binary file thats encoded at the bottom of the file if you save it after opening on a linux machine.
3. run `sed -e '0,/^#ARCHIVE#$/d' "PATH_TO_INSTALLER.run" | gzip -d | tar -x` inside that folder to extract the contents
4. create a new python file in that folder. name it whatever you want. paste in the following contents:
```python
from client import PaladinLinuxClient

deciphered = PaladinLinuxClient.decipher(PaladinLinuxClient.CONFIG_FILE)
strip_namespace=PaladinLinuxClient.strip_namespace(deciphered)


with open(PaladinLinuxClient.CONFIG_FILE + ".xml", "w") as conf:
	conf.write(strip_namespace)
```
5. run this script to use the clients own decryption mechanism to decrypt the config file to a plain XML file.
6. this file seems to contain all the config that you should need in order to manually configure the network. Sections of it from which you should grab values will be referenced in the steps below so as to avoid copying the values themselves and compromising the security of RIT's network.

## Configuring the network

TBD - the eduroam network has not yet been enabled

this might be vaguely helpful in the meantime: http://kb.mit.edu/confluence/pages/viewpage.action?pageId=152599592
 -->

