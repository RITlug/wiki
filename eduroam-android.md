# Connecting to the RIT eduroam network under Android

## Getting certificates

This process mostly mirrors the desktop equivalent, with a couple of changes.

1. Go to [https://rit.edu/wifi](https://rit.edu/wifi) and select eduroam.  
   ![A screenshot of rit.edu/wifi showing the two options, "eduroam" and "start.rit.edu"](/assets/img/eduroam-android/rit-edu-wifi.png)
2. Select 'User-Defined' in the 'Select your device' dropdown menu.  
   ![A page saying 'The following system was detected' and an Android logo. The 'Select your device' dropdown below it is circled](/assets/img/eduroam-android/os-select-android.png)
3. Click 'Sign In' and sign in with your RIT account.  
   ![The next page, showing a 'Sign In' button](/assets/img/eduroam-android/sign-in-page.png)
4. You'll be asked to create a certificate. Enter a name for it in the 'User Description' box, then click 'Create'.  
   ![A dialog titled 'Create Certificate', with a disabled 'Operating System' field and a 'User Description' field to be filled in](/assets/img/eduroam-android/create-certificate.png)
5. You will be prompted to create a password for your private key. The password you choose must be at least 6 and at most 16 characters long and contain a letter, number, and symbol.
   > Note: once you click 'Submit' here, the webpage may become unresponsive for up to a minute. This is normal, creating a certificate request is fairly computationally expensive, and some phones will need a bit.
   
   ![A dialog titled 'Passphrase for private key' with a text field, a 'Cancel' button, and a 'Submit' button](/assets/img/eduroam-android/enter-passphrase.png)
6. Download your certificate as a P12 file and click 'Next'.  
   ![A dialog reading 'Install your certificate by selecting any one of the following formats:' with links for P12, PFX, PEM, and PVK, followed by a 'Next' button](/assets/img/eduroam-android/select-format.png)
7. Download the RIT root CA.  
   ![A page listing wifi settings, including a link to the RIT root CA certficate](/assets/img/eduroam-android/rit-root-ca-download.png)

## Installing the certificates

1. Open the Settings application and navigate to 'Install certificates'.  
   > This may vary by phone manufacturer, as some phone manufacturers like to customize their settings menus. You can search for this menu option if you aren't able to navigate to it manually.
   
   On my Moto e5 running near-stock Android 8.1, the path is 'Network & Internet' > 'Wi-Fi' > 'Wi-Fi Preferences' > 'Advanced' > 'Install certificates'.
2. Clicking 'Install certificates' should open up a file picker. Navigate to your user certificate (the P12 file downloaded earlier), which is likely in your 'Downloads' folder, and choose that. You will need to enter the password you chose earlier for the certificate, and then enter a name for the certificate. Make sure under 'Credential use' you pick 'Wi-Fi'.  
   ![The Android file picker, showing 'Downloads' folder and a file called '{blacked out} (RIT Student).p12'](/assets/img/eduroam-android/downloads-p12.png)
   ![A dialog titled 'Extract certificate' with a password prompt](/assets/img/eduroam-android/cert-password.png)
   ![A dialog titled 'Name the certificate' with a textbox for the name and a dropdown called 'Crdential use', with the options 'VPN and apps' and 'Wi-Fi'](/assets/img/eduroam-android/rit-user-cert-name-dialog.png)
3. Do the same for the RIT root CA.  
   > Some phones (like Samsung) won't allow you to install CA certificates without root, and if so, you may skip this step. Having the RIT root CA installed is recommended but not necessary.
   
   Make sure you also select 'Wi-Fi' for 'Credential use' in this dialog as well.
   ![The same 'Name the certificate' dialog from the last step](/assets/img/eduroam-android/rit-root-ca-install.png)

## Connecting to the hotspot

Connect to the 'eduroam' hotspot. A settings menu should open. Input the following:
1. Make sure 'EAP method' is set to 'TLS'.
2. Select the RIT root CA under 'CA certificate'.  
   > If you were unable to install this certificate, select 'Do not validate' or an equivalent option.
3. Enter 'radius.rit.edu' for the domain.
4. Select your user certificate under 'User certificate'.
5. Under 'Identity', enter '[your RIT username]@main.ad.rit.edu'.

![The wifi dialog showing the settings described above](/assets/img/eduroam-android/eduroam-settings.png)
