[ [Intro](README.md) ] - [ [Jail Creation](freenas_1_jail_creation.md) ] - [ [Bitcoin](freenas_2_bitcoin.md) ] - [**Tor**] - [ [Electrum](freenas_4_electrum.md) ] - [ [lnd](freenas_5_lnd.md) ] - [ [loopd ](freenas_5a_loopd.md)] - [ [RTL](freenas_6_rtl.md) ] - [ [Extras](extras.md) ]

## Guide to ₿itcoin & ⚡Lightning️⚡ on 🦈FreeNAS🦈

### Why Tor

Tor is a communications protocol that encrypts and anonymizes communications by bouncing encrypted data between relays. It's like using several different VPNs between client and server. TOR also allows you to securely and privately remote connect to your home server with a static address and zero router configuration! Hidden Service Version 3 is not discoverable, so you don't have to worry about exposing ports to the public, as long as you don't share your hidden service onion address!

This guide runs bitcoind on ~~clearnet and~~ tor, runs lnd exclusively on tor, and privately host ports for remote connections with mobile wallets and electrum clients.  


### Install & Configure Tor
```
root@bitcoin:~ # pkg install tor nano
root@bitcoin:~ # nano /usr/local/etc/tor/torrc
```
Uncomment (remove the #) from the following lines:
```
DataDirectory /var/db/tor
ControlPort 9051
CookieAuthentication 1
```
Add the following lines:
```
CookieAuthFileGroupReadable 1
CacheDirectoryGroupReadable 1
```

Add the following lines to privately serve your remote clients for mobile lightning wallets (`8080`) and (`10009`), and electrum (`50001`):

```
HiddenServiceDir /var/db/tor/remote_connections
HiddenServiceVersion 3
HiddenServicePort 50001 127.0.0.1:50001
HiddenServicePort 8080 127.0.0.1:8080
HiddenServicePort 10009 127.0.0.1:10009
```
Save (CTRL+O, ENTER), then exit (CTRL+X)

Enable autostart by adding `tor_enable="YES"` to `/etc/rc.conf`.
```
# nano /etc/rc.conf
tor_enable="YES"
```
Save (Ctrl+O,ENTER) and exit (CTRL+X)

In order to force bitcoind to use only tor the following changes to the bitcoin config file need to be applied (ignore this if you would like bitcoind to use clearnet and tor):  

```
# nano /usr/local/etc/bitcoin.conf
proxy=127.0.0.1:9050
listen=1
bind=127.0.0.1

maxconnections=20 # Add this line if you would like to restrict the connections to 20 to save bandwidth. 
```

Save (Ctrl+O,ENTER) and exit (CTRL+X)

Add user `bitcoin` to the` _tor` group so that bitcoin can read the cookie authentication file in `/var/db/tor`, then stop and start the tor and bitcoin service:

```
# pw usermod bitcoin -G _tor
# service bitcoind stop
# service tor start
# service bitcoind start
# bitcoin-cli -datadir=/var/db/bitcoin getnetworkinfo
```
You should see a .onion address listed!

View the private onion address of your new hidden service:
```
# cat /var/db/tor/remote_connections/hostname
myprivateonionaddressocyn4rixm632jid.onion
```

### How to upgrade tor:
```
# service tor stop
# pkg update && pkg upgrade tor
# service tor start
```

Next: [ [Electrum](freenas_4_electrum.md) ]
