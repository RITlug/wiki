---
title: Connecting to the RIT eduroam network under Linux
weight: 10

---

## Getting the config

1. Create a temporary directory somewhere and cd into it from a terminal
2. download the linux installer from https://rit.edu/wifi. Dont open it since it might break the CRLF line endings and the binary file thats encoded at the bottom of the file if you save it after opening on a linux machine.
3. run `sed -e '0,/^#ARCHIVE#$/d' "PATH_TO_INSTALLER.run" | gzip -d | tar -x` inside that folder to extract the contents
4. create a new python file in that folder. name it whatever you want. paste in the following contents:
```python
from client import PaladinLinuxClient

deciphered = PaladinLinuxClient.decipher(PaladinLinuxClient.CONFIG_FILE)
strip_namespace=PaladinLinuxClient.strip_namespace(deciphered)


with open(PaladinLinuxClient.CONFIG_FILE + ".xml", "w") as conf:
	conf.write(strip_namespace)
```
5. run this script to use the clients own decryption mechanism to decrypt the config file to a plain XML file.
6. this file seems to contain all the config that you should need in order to manually configure the network. Sections of it from which you should grab values will be referenced in the steps below so as to avoid copying the values themselves and compromising the security of RIT's network.

## Configuring the network

TBD - the eduroam network has not yet been enabled

this might be vaguely helpful in the meantime: http://kb.mit.edu/confluence/pages/viewpage.action?pageId=152599592
