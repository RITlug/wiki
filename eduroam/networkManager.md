---
title: Connecting to the RIT eduroam network under Linux - NetworkManager
layout: default


---
## NetworkManager Tutorial

This is the complete instruction list for configuring NetworkManager. These instructions use the `P12` certificate, although the `PEM` will also work properly.

## Requirements
Currently, the only required software required for this is a modern web browser, and a desktop environment compatible with NetworkManager. Desktop managers that default to using NetworkManager include KDE, GNOME, LXDE, Cinnamon, and MATE.

## Instructions

1. Generate a personal certificate according to [this tutorial](/certificates.md).

2. Click the wifi logo in the status bar, and then select `eduroam`.

![](/assets/img/eduroam/open-networkmanager.png)

3. Enter the configuration details in the window that appears. If the password field turns red it means the password is incorrect. 

![](/assets/img/eduroam/configure-networkmanager.png)

## Troubleshooting

If you cannot connect to the internet after attempting to use this tutorial, go to the [nmcli](./nmcli.md) documentation, and ensure that all settings are correct. There are also more troubleshooting steps listed there.
