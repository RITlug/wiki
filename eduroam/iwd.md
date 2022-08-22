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
- A `PEM` certificate
- Root/`sudo` priveleges
- A text editor. Terminal emulators are easiest for this, but are not strictly necessary
    - `nano`
    - `micro`
    - `nvim`/`vim`/`vi`
    - `emacs`

## Instructions

All commands starting with `$` can be used as your standard user. All commands using `#` require to be run as root, generally by preceding the command with `sudo`.

1. Generate a personal certificate according to [this tutorial](/certificates.md).

2. Run the following command to determine what network interfaces you have available. This tutorial assumes a network interface of `wlan0`.
```
# ip link
```
This command will list all available network interfaces. `lo` (Loopback), `eth` (Ethernet) and `veth` (Virtual Ethernet) devices can all be ignored for the purposes of this tutorial.

3. Move both the RIT CA Cert and the encrypted `.pem` file into the following directory: `/var/lib/iwd`. This can be done by running the following command:
```
# cp location/of/pemFile /var/lib/iwd/
```
This command must be run as root, as the default user does not have permission to create files in this directory.

4. In the same directory, create a configuration file named `eduroam.8021x`. In it, input the following information:
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

[Settings]
AutoConnect=true
```
Replace `abc1234@rit.edu` with your RIT email. Ensure that the name of the `pem` file matches the name written in `eduroam.8021x`. *Do not change the `EAP-Identity` line*.

5. Run the following command as root (or with sudo):
```
# iwctl station wlan0 connect eduroam
```
This tells `iwd` to connect to the `eduroam` WiFi network, using the network interface `wlan0`. This will prompt you for the password *to the `.pem` file you downloaded*. Enter that now.

   - If you don't want to be asked for the cert's password each time you connect, add the password in plain text under a new `EAP-TLS-ClientKeyPassphrase` entry in the config file from the previous steps. Depending on your threat model this may be a security risk since the password will be stored in plaintext.

6. (Optional) Run the following to ensure that your connection is working:
```
ping 9.9.9.9
```
This command tests the ability to talk to the [Quad9 DNS Service](https://www.quad9.net/). If the command returns an error, then you are not connected to the `eduroam` network (or any other network, for that matter).

## Troubleshooting
For any devices having issues, see [this article on the ArchWiki](https://wiki.archlinux.org/title/Iwd#Verbose_TLS_debugging) to start the debugging process. Common problems include:

- Entering an incorrect location for the certificates, or entering an invalid name for the certificates. This results in a `Not configured` on attempted connection.
- If the above doesn't resolve the `Not configured` error, try running `modprobe pkcs8_key_parser` (as root/sudo). This kernel module is needed to actually load the certificate, which may not happen automatically on some distros (current suspicion is distros without systemd are most affected by this)
- Entering `TTLS` instead of `TLS`. This results in a `journalctl` error shown below:
```
EAP server tried method 13 while client was configured for method 21
EAP completed with eapFail
4-Way handshake failed for ifindex 4, reason: 23
```


## See Also
- [iwd's official autotests](https://git.kernel.org/pub/scm/network/wireless/iwd.git/tree/autotests)
