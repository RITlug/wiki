---
title: Connecting an Android device to the RIT eduroam network 
layout: default


---

This is the complete instruction list for configuring Android to connect to eduroam. These instructions use the `P12` certificate.

## Instructions

Generate a personal certificate according to [this tutorial](./certificates).

### Installing the certificates

1. Open the Settings application and navigate to 'Install certificates'.  
   This may vary by phone manufacturer, as some phone manufacturers like to customize their settings menus. You can search for this menu option if you aren't able to navigate to it manually.
   
   On my Moto e5 running near-stock Android 8.1, the path is 'Network & Internet' > 'Wi-Fi' > 'Wi-Fi Preferences' > 'Advanced' > 'Install certificates'.  
   [This stackoverflow post](https://stackoverflow.com/a/65319223) and [this Pixel support page](https://support.google.com/pixelphone/answer/2844832) may help.
2. Clicking 'Install certificates' should open up a file picker. Navigate to your user certificate, which is likely in your 'Downloads' folder, and choose that. You will need to enter the password you chose earlier for the certificate, and then enter a name for the certificate. Make sure under 'Credential use' you pick 'Wi-Fi'.  
   ![The Android file picker, showing 'Downloads' folder and a file called '{blacked out} (RIT Student).p12'](/assets/img/eduroam/android/downloads-p12.png)
   ![A dialog titled 'Extract certificate' with a password prompt](/assets/img/eduroam/android/cert-password.png)
   ![A dialog titled 'Name the certificate' with a textbox for the name and a dropdown called 'Crdential use', with the options 'VPN and apps' and 'Wi-Fi'](/assets/img/eduroam/android/rit-user-cert-name-dialog.png)
3. Install the RIT root CA. Make sure you also select 'Wi-Fi' for 'Credential use' in this dialog as well.  

   The steps for installing a CA certificate may be different than the steps for a user certificate. [This stackoverflow post](https://stackoverflow.com/a/65319223) may help.  
   
   ![The same 'Name the certificate' dialog from the last step](/assets/img/eduroam/android/rit-root-ca-install.png)

### Connecting to the hotspot

Connect to the 'eduroam' hotspot. A settings menu should open. Input the following:
1. Make sure 'EAP method' is set to 'TLS'.
2. Select the RIT root CA under 'CA certificate'.  
   If you were unable to install this certificate and a 'Do not validate' or equivalent option exists, you can select that instead.
3. Enter 'radius.rit.edu' for the domain.
4. Select your user certificate under 'User certificate'.
5. Under 'Identity', enter '[your RIT username]@main.ad.rit.edu'.

![The wifi dialog showing the settings described above](/assets/img/eduroam/android/eduroam-settings.png)
