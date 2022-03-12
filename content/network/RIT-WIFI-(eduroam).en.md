---
title: Connecting to the RIT eduroam network under Linux
weight: 10

---
## An Overview

Starting after 2022 commencement RIT is replacing the 'RIT' wifi network with a larger, multi-university 'eduroam' network. While this change may seem straightforward, RIT has decided to use EAP-TLS authentication rather than using MSCHAPV2. This is not a widespread practice, even among other universities who utilize the eduroam network. This can lead to some confusion, especially for anyone not running MacOS or Windows. This guide hopes to clear that up.

## Steps

1. Go to [*https://rit.edu/wifi*](https://rit.edu/wifi), then select 'eduroam'. ![](/rit-linux-wiki/img/eduroam/wifi-page.png)

2. In the 'select your device' dropdown, select the 'user-defined' option. ![](/rit-linux-wiki/img/eduroam/select-os.png)

3. Click 'sign in' and enter your RIT credentials when prompted. ![](/rit-linux-wiki/img/eduroam/start-user-cert.png)

4. Once you are logged in, you will be asked to create a certificate. Enter a name you would like to give this certificate in the 'User Description' input box, then, click the 'create' button. ![](/rit-linux-wiki/img/eduroam/create-user-cert.png)

5. You will then be prompted to create a password to protect your private key. This will be needed to set up eduroam on a new device. If you are unsure, you can use [1Password's public generator](https://1password.com/password-generator/?) which has a "memorable password" mode. The password you choose must be at least six (but no more than 16) characters long and contain a letter, number, and symbol. ![](/rit-linux-wiki/img/eduroam/password.png)
   
6. Choose a format to download your certificate in. If you arent sure what you need, select P12 format and save the file somewhere you can get to it easily. ![](/rit-linux-wiki/img/eduroam/cert-download.png)

7. Click the link on this page to save the RIT CA Cert. You'll also want to have this handy for the next step ![](/rit-linux-wiki/img/eduroam/root-ca.png)

8. In a terminal, navigate to the directory you saved the certificates to. Copy the RIT certificate file to '/usr/local/share/ca-certificates/'. A command like `sudo cp *.cer /usr/local/share/ca-certificates/` will copy all files ending in .cer in your current folder to this location. 

9. You now have everything you need to connect to the network


## Connecting to the network

The process of connecting to the network may be different depending on the Linux distribution you use. In general, here is some information that may be asked during the connection process:

**Identity:** Your RIT email address. i.e. [username]@rit.edu

**Authentication:** TLS

**Wifi Security:** WPA and WPA2 Enterprise

**CA certificate:** The RIT .cer certificate you downloaded earlier

**User Certificate/private key:** Your .p12 file from earlier.

**User Key Password:** The password you set for this .p12 file earlier.


### Kali Linux

1. To connect click the wifi logo in the status bar, and then select 'eduroam' ![](/rit-linux-wiki/img/eduroam/open-networkmanager.png)

2. Enter the configuration details in the window that appears. If the password field turns red it means the password is incorrect. ![](/rit-linux-wiki/img/eduroam/configure-networkmanager.png)

Once these instructions are complete, you will be able to connect to the
new eduroam setup without needing to run any scripts!

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

