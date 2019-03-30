[ [<< Back to Extras](https://github.com/seth586/guides/blob/master/FreeNAS/extras.md) ]

## Ride The Lightning (RTL) over Tor for Android

Tor has numerous benefits for serving RTL for remote connections: 
* Securely, anonymously and remotely connect to your home server anywhere you are. 
* Connections are encrypted end to end.
* Onion address will remain static even if you are behind a dynamic IP address. No ddns required!
* No port forwards required by your router! 
* Tor cookie authenticated hidden services keeps your RTL website a secret 
* Tor cookie authentication prevents denial of service attacks!
VPN Disadvantages:
* Increased configuration complexity
* requires ddns
SSL Disadvantages:
* Increased configuration complexity
* requires ddns
* vulnerable to denial of service attacks
* Your website is public. Best defense is being private in the first place!

Edit `torrc` with `nano /usr/local/etc/tor/torrc` and add the following lines (`myandroid` can be unique):
```
HiddenServiceDir /var/db/tor/rtl/
HiddenServiceVersion 2
HiddenServiceAuthorizeClient stealth myandroid
HiddenServicePort 3000 127.0.0.1:3000
```
Save (Ctrl+O, ENTER) and exit (Ctrl+X)

Restart Tor 
```
# service tor restart
```

View the private credentials of your new hidden service. The first part is the onion address, the second part is the secret.
```
# cat /var/db/tor/rtl/hostname
z1234567890abc.onion AbyZXCfghtG+E0r84y/nR # client: myandroid
```

Download orbot for android. https://guardianproject.info/apps/orbot/

Download Tor Browser for android: https://www.torproject.org/download/#downloads-alpha

Open orbot. Click the `⋮`, select `hidden services ˃`, select `Client cookies`.

Press the + button on the lower right. Type in the the onion address and secret cookie you revealed with `cat  /var/lnd/tor/rtl/hostname`.

Go back to orbot's main screen, and select the gear icon under `tor enabled apps`. Add `Tor Browser`, then press back. Click `stop` on the big onion logo. Exit orbot and reopen it. Turn on `VPN Mode`. Start your connection to the tor network by clicking on the big onion (if it has not automatically connected already)

Now open Tor browser and type in the onion address (example `z1234567890abc.onion`) Only you have access to this website!

[ [<< Back to Extras](https://github.com/seth586/guides/blob/master/FreeNAS/extras.md) ]