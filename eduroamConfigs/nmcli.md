---
title: Connecting to the RIT eduroam network under Linux - nmcli
layout: default


---
## nmcli Tutorial

This is the complete instruction list for configuring `nmcli`. This was tested on a clean Tails install. File location may differ across distributions. If directories don't exist, check your distribution's documentation for the correct location of the files. 

## Requirements
The following is required to use this configuration:
- `nmcli`/NetworkManager, and a terminal emulator to run commands in
- A modern web browser
- A `PEM` or `P12` certificate (described below in steps 1-8)
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

7. Choose a format to download your certificate in. `P12` and `PEM` certificates are currently known to be compatible with NetworkManager.

![](/assets/img/eduroam/cert-download.png)

8. Click the link on this page to save the RIT CA Cert. You'll also want to have this handy for the next step 

![](/assets/img/eduroam/root-ca.png)

9. To find out what the name of your network interface is, run the following command:
```
$ nmcli
```
This command returns all network interfaces `nmcli` can access. For the examples that follow, this tutorial assumes that `wlan0` is the correct network interface.

10. Open a terminal, and run the following command: 
```
$ nmcli connection add type wifi ifname wlan0 con-name eduroamWiFi ssid eduroam
# cd /etc/NetworkManager/system-connections/
```
The first command creates a new wifi connection named `eduroamWiFi`, bound to the `wlan0` interface, which will connect to the network named `eduroam`.
The second command changes your current working directory to the NetworkManager/`nmcli` configuration directory.

11. Open the `eduroam.nmconnection` file in the editor of your choice *as root*. If you do not open it as root, the changes will not be saved.

12. Edit the file so that it contains the following lines
```
[connection]
id=eduroamWiFi
uuid=
type=wifi
interface-name=wlan0
permissions=

[wifi]
mac-address-blacklist=
mode=infrastructure
ssid=eduroam

[wifi-security]
auth-alg=open
key-mgmt=wpa-eap

[802-1x]
ca-cert=location/of/ritCACert
client-cert=location/of/p12PrivateKey
domain-suffix-match=radius.rit.edu
eap=tls;
identity=abc1234@rit.edu
private-key=location/of/p12PrivateKey
private-key-password=P@ssw0rd1

[ipv4]
dns-search=
method=auto

[ipv6]
addr-gen-mode=stable-privacy
dns-search=
method=auto
```
This file will contain most of these lines already. The `uuid` field will already be filled in, do not edit this field.

13. Run the following command: 
```
# nmcli conection up eduroamWiFi
```
This tells `nmcli` that you want to use the configuration `eduroamWiFi`, which was edited in the previous step. 

14. (Optional) Test your connection by running: 
```
$ ping 9.9.9.9
```
This command tests the ability to talk to the [Quad9 DNS Service](https://www.quad9.net/). If the command returns an error, then you are not connected to the `eduroam` network (or any other network, for that matter). 

## Troubleshooting

Basic troubleshooting steps can be found in [the Archwiki](https://wiki.archlinux.org/title/NetworkManager#Troubleshooting) or your distribution's documentation. 