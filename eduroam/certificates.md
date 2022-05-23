---
title: Manually generate a personal certificate
layout: default


---
## Generating a personal certificate

To connect to RIT's `eduroam` WiFi network.

1. Connect to the `RIT-WiFi` network.

2. Go to [*https://rit.edu/wifi*](https://rit.edu/wifi), then select `eduroam`.

![](/assets/img/eduroam/wifi-page.png)

3. In the 'Select your device' dropdown, select the `user-defined` option.

![](/assets/img/eduroam/select-os.png)

4. Click 'Sign in' and enter your RIT credentials when prompted.

![](/assets/img/eduroam/start-user-cert.png)

5. Once you are logged in, you will be asked to create a certificate. Enter a name you would like to give this certificate in the 'User Description' input box, then, click the 'Create' button.

![](/assets/img/eduroam/create-user-cert.png)

6. You will then be prompted to create a password to protect your private key. This will be needed to set up eduroam on a new device. If you are unsure, you can use [1Password's public generator](https://1password.com/password-generator/?) which has a "memorable password" mode. The password you choose must be at least six (but no more than 16) characters long and contain a letter, number, and symbol.

![](/assets/img/eduroam/password.png)

7. Choose a format to download your certificate in. Check the page in this wiki for your network manager to see which version you will need. Most managers will use `P12`.

![](/assets/img/eduroam/cert-download.png)

8. Click the link on this page to save the RIT CA Cert.

![](/assets/img/eduroam/root-ca.png)

## Next Steps

Once you have generated your certificate, go to [this tutorial](./) to configure your network manager.
