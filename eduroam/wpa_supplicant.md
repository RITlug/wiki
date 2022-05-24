---
title: Connecting to the RIT eduroam network under Linux - wpa_supplicant
layout: default


---
## wpa_supplicant Tutorial

This is the complete instruction list for configuring `wpa_supplicant`. This was tested on a clean Arch install ISO. File location may differ across distributions. If directories don't exist, check your distribution's documentation for the correct location of the files.

## Requirements
The following is required to use this configuration:
- `wpa_supplicant`, and a terminal emulator to run commands in
- A modern web browser
- A `P12` certificate
- Root/`sudo` priveleges
- A text editor. Terminal emulators are easiest for this, but are not strictly necessary
    - `nano`
    - `micro`
    - `nvim`/`vim`/`vi`
    - `emacs`

## Instructions

All commands starting with `$` can be used as your standard user. All commands using `#` require to be run as root, generally by preceding the command with `sudo`.

1. Generate a personal certificate according to [this tutorial](/certificates.md).

2. Run the following command to determine what network devices are available. This tutorial assumes the use of the network interface `wlan0`.
```
# wpa_cli interface
```

3. Move both the RIT CA Cert and the encrypted `.p12` file into a common directory that you don't plan on interacting with much. For testing purposes, `/opt/` was used. Locations within `~` or `/home/$USER` may not work properly, due to improper permissions, although this is untested as of writing.

4. Open your text editor of choice, create a file in `/etc/wpa_supplicant`, and fill in the file according to the block below. You can name the file whatever you wish, but for automation purposes its recommended to name the file `wpa_supplicant-[interface name].conf`. This makes it easier for `systemd` and `dhcpcd` to interface with `wpa_supplicant` once it's set up.
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
    domain_match="radius.rit.edu"
    identity="abc1234@rit.edu"
    ca_cert="/location/of/RIT/cert"
    private_key="location/of/p12PrivateKey.p12"
    private_key_passwd="P@ssw0rd1"
}
```

5. You're all set! Run the following commands in a terminal to start `wpa_supplicant`, then start `dhcpcd`. After this, your network should be set up properly.
```
# wpa_supplicant -B -i wlan0 -c /etc/wpa_supplicant/[config file name]
# dhcpcd wlan0
```
The first command tells `wpa_supplicant` to open in the background and, using the `wlan0` interface, use the configuration file created in the previous step to connect to `eduroam`.
The second command tells the local `DHCP` client that there is an open network connection on the network interface `wlan0`, and to assign an IP address to the device.

## Automation notes
Note that while this config file is persistent across reboots, as this guide stands, you will need to start `wpa_supplicant` and `dhcpcd` manually on each boot. For more information on starting `wpa_supplicant` at boot, and auto-starting `dhcpcd`, consult the [ArchWiki](https://wiki.archlinux.org/title/Wpa_supplicant#At_boot_(systemd)), or your distribution's wiki for non-`systemd` systems.

## Troubleshooting
For debugging purposes, the `-B` flag shown above can be omitted, allowing you to see the complete output of the command. Note that this omitting this flag will make the current terminal unusable, so a second session or terminal will be required to run the `dhcpcd` command. Also note that closing the terminal containing the `wpa_supplicant` command will also end the connection to the Wi-Fi, so act with care.
