---
title: Setting up RIT Email in Thunderbird
layout: default


---
## Introduction

This is a guide on how to connect your RIT email to a third-party email client.
This was tested on Linux Mint, Windows 10, and Android, but should work on most operating systems.

## Setup

Make sure that you have Mozilla Thunderbird installed.
If it isn't, you can download it from the [Thunderbird Website](https://www.thunderbird.net/en-US/download/).
On Linux, it can be installed through the terminal by installing the thunderbird package.
Thunderbird can also be installed on Android through the Google Play Store.
Some Linux distros may come with it already installed, like Linux Mint.

- Open Thunderbird, and in the top right corner (by default), click the Thunderbird Menu button.
Then, click "add account", and then "email".
- Enter in your name, RIT email (abc1234@g.rit.edu), and the email password.
You will need to use your @g.rit.edu email that you access through gmail, not @rit.edu.
- After entering in your name, email, and password, click the "configure manually" button, There you will be prompted to enter in more information.

For the incoming server, enter the following information:
- Protocol: IMAP
- Hostname: imap.gmail.com
- Port: 993
- Connection security: SSL/TLS
- Authentication method: OAuth2
- Username: your RIT email (abc1234@g.rit.edu)

For the outgoing server, enter the following information:
- Hostname: smtp.gmail.com
- Port: 465 (587 might also work, but it wasn't tested here)
- Connection security: SSL/TLS
- Authentication method: OAuth2
- Username: your RIT email (abc1234@g.rit.edu) (again)

## Images

Below are images of the settings used to set up Thunderbird on various operating systems.
- Linux Mint:

![](/assets/img/email/Thunderbird-settings-LM.png)
- Windows 10:

![](/assets/img/email/Thunderbird-settings-win10.png)
- Android incoming:

![](/assets/img/email/Thunderbird-incoming.jpg)
- Android outgoing:

![](/assets/img/email/Thunderbird-outgoing.jpg)

## More Info

More information about the email settings can be found here [Here](https://web.archive.org/web/20220819054252/https://google.rit.edu/help/emailclient.html).
This link may not always work.

Last edited on: 26 March 2025
