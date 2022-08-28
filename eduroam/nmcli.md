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
- A `PEM` or `P12` certificate 
- Root/`sudo` priveleges
- A text editor. Terminal emulators are easiest for this, but are not strictly necessary
    - `nano`
    - `micro`
    - `nvim`/`vim`/`vi`
    - `emacs`

## Instructions

All commands starting with `$` can be used as your standard user. All commands using `#` require to be run as root, generally by preceding the command with `sudo`.

1. Generate a personal certificate according to [this tutorial](/certificates.md).

2. Grab the name of the wireless network interface  that you want to use for this connection. You will need this throughout this setup process. You can list all network interfaces by running the command:
```
$ nmcli device show
```
>Check the one whose `GENERAL.TYPE` is `wifi`. Most commonly, this is `wlan0` or `wlp5s0`.
3. Open a terminal and use nmcli to create a new wifi connection on your wireless network interface that connects to the `eduroam` network:
```
$ nmcli connection add type wifi ifname <your interface> con-name eduroamWiFi ssid eduroam
```
4. Open the `eduroamWiFi.nmconnection` file in the editor of your choice *as root*. If you do not open it as root, the changes will not be saved.
```
# vim /etc/NetworkManager/system-connections/eduroamWiFi.nmconnection
```
>Substitute `vim` for editor of choice
5. Edit the file so that it contains the following lines. The file will contain most of these lines already. The `uuid` field will already be filled in, **do not edit this field**.
```
[connection]
id=eduroamWiFi
uuid=<generated UUID, don't edit this>
type=wifi
interface-name=<your interface>
permissions=

[wifi]
mac-address-blacklist=
mode=infrastructure
ssid=eduroam

[wifi-security]
auth-alg=open
key-mgmt=wpa-eap

[802-1x]
ca-cert=location/of/ritCACert.cer
client-cert=location/of/p12PrivateKey.p12
domain-suffix-match=radius.rit.edu
eap=tls;
identity=abc1234@rit.edu
private-key=location/of/p12PrivateKey.p12
private-key-password=<your private key password>

[ipv4]
dns-search=
method=auto

[ipv6]
addr-gen-mode=stable-privacy
dns-search=
method=auto
```

7. Run the following command to activate the connection:
```
# nmcli conection up eduroamWiFi
```
This tells `nmcli` that you want to bring up the network using the configuration that you created in the previous step.

8. (Optional) Test your connection by running:
```
$ ping 9.9.9.9
```
This command tests the ability to talk to the [Quad9 DNS Service](https://www.quad9.net/). If the command returns an error, then you are not connected to the `eduroam` network (or any other network, for that matter).

## Troubleshooting

Basic troubleshooting steps can be found in [the Archwiki](https://wiki.archlinux.org/title/NetworkManager#Troubleshooting) or your distribution's documentation.
