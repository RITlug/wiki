---
title: How to connect your RIT email to Thunderbird
layout: default


---
## Introduction

This is a guide on how to connect your RIT email to Thunderbird.
This was tested on Linux Mint, Windows 10, and Android, but should work on most operating systems.

## Setup

Make sure that you have Thunderbird installed.
If it isn't, you can download it from the [Thunderbird Website](https://www.thunderbird.net/en-US/download/).
On Linux, it can be installed through the terminal by installing the thunderbird package.
Thunderbird can also be installed on Android through the Google Play Store.
K-9 Mail (what Thunderbird for Android is based on) will also work.
Some Linux distros may come with it already installed, like Linux Mint.

- Open Thunderbird, and in the top right corner (by default), click the Thunderbird Menu button.
Then, click "add account", and then "email".
- Enter in your name, RIT email, and the email password.
You can use either your @rit.edu, or your @g.rit.edu email here, as this just changes what address what address the mail recpient sees, but for the manual configuration, only your @g.rit.edu email will work.
- The password doesn't matter and it doesn't need to be the same password for your RIT account (as long as it's secure it should be fine).
- After entering in your name, email, and password, click the "configure manually" button, There you will be prompted to enter in more information.

For the incoming server, enter the following information:
- Protocol: IMAP
- Hostname/Server: imap.gmail.com
- Port: 993
- Connection security: SSL/TLS
- Authentication method: OAuth2
- Username: your RIT email (abc1234@g.rit.edu)

For the outgoing server, enter the following information:
- Hostname/Server: smtp.gmail.com
- Port: 465
- Connection security: SSL/TLS
- Authentication method: OAuth2
- Username: your RIT email (abc1234@g.rit.edu)

These settings should work on other email clients, but only Thunderbird and K-9 were tested here.

After you enter in the information above, an RIT login window will open up. After logging in, you should get a multifactor authentification prompt, make sure to approve it. Then, you will be sent to a page for allowing permissions. These include, allowing Thunderbird to send, an recieve mail, modify contacts, and view your calendar, click allow to give Thunderbird these permissions.

## Images

Below are images of the settings used to set up Thunderbird on various operating systems. The order of email settings in the lists above correspond to how they should when being filled in.

- The incoming and outgoing settings for Thunderbird on a desktop/laptop. The order of settings corresponds to the list order above.

![The incoming and outgoing settings for Thunderbird on a desktop/laptop are as follows: Incoming server: Protocol: IMAP, Hostname: imap.gmail.com, Port: 993, Connection security: SSL/TLS, Authentification method: OAuth2, Username: Your google RIT email (abc1234@g.rit.edu), Outgoing Server: Hostname: smtp.gmail.com, port: 465, Connection security: SSL/TLS, Authentification method: OAuth2, username: Your google RIT email (abc1234@g.rit.edu)](/assets/img/email/Thunderbird-settings-LM.png)

- The incoming server settings for Thunderbird on Android. The order of settings corresponds to the order above. Leave the client certificate field blank.

![The incoming server settings for Thunderbird/K9 on Android are as follows: Protocol: IMAP, Server: imap.gmail.com, Security: SSL/TLS, Port: 993, Authentification: OAuth 2.0, Username: Your google RIT email (abc1234@g.rit.edu), Client certificate: None](/assets/img/email/Thunderbird-incoming.jpg)

- The outgoing server settings for Thunderbird on Android. The order of settings corresponds to the order above. Leave the client cerfificate field blank. 

![The outgoing server settings for Thunderbird/K9 on Android are as follows: Server: smtp.gmail.com, Security: SSL/TLS, Port: 465, Authentification: OAuth 2.0, Username: Your google RIT email (abc1234@g.rit.edu), Clent certificate: None](/assets/img/email/Thunderbird-outgoing.jpg)

## More Info

More information about the email settings can be found [Here](https://web.archive.org/web/20220819054252/https://google.rit.edu/help/emailclient.html).
This link may not always work.
