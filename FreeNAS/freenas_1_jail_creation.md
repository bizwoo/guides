[ [Intro](README.md) ] - [**Jail Creation**] - [ [Bitcoin](freenas_2_bitcoin.md) ] - [ [Tor](freenas_3_tor.md) ] - [ [Electrum](freenas_4_electrum.md) ] - [ [lnd](freenas_5_lnd.md) ] - [ [loopd ](freenas_5a_loopd.md)] - [ [RTL](freenas_6_rtl.md) ] - [ [Extras](extras.md) ]

## Guide to ₿itcoin & ⚡Lightning️⚡ on 🦈FreeNAS🦈

### Jail Creation

Think of jails as more efficient virtual machines (VMs). You could just install a bunch of VMs on FreeNAS, run linux on them, and pick your choice on the many varieties of linux guides available online. But running a VM requires a lot more resources than jails, allocating memory just for that VM, etc. Plus, if we mess up, we can delete the jail and start over. Anything we do in the jail should not mess up anything on the host machine. After all, we built a computer with server grade hardware for the uptime!

FreeNAS 11.2 uses iocage to manage jails. Previous versions used warden, which is now considered deprecated. To create a jail, log in to your FreeNAS user interface, and select Jails on the left hand menu. Click the ‘ADD’ button on the top right, and select 'advanced jail creation'.

![FreeNAS_Jail](images/jail_create.png)  

Give the jail a name, such as `bitcoin`. Select release `11.2-RELEASE`, select `DHCP Autoconfigure IPv4` and select `Auto-start` as shown. Click `SAVE`.

In my FreeNAS setup I use a dedicated storage pool for the Jails and a separate storage pool for all my media and data. The Jails' pool is of relative small capacity; therefore, I prefer to create a mount point for the bitcoin jail to store all the bitcoin data such as blockcain, etc. in the larger storage pool. Skip this step if you prefer to store the bitcoin data in the same location of the jail. In the Bitcoin jail click `Mount points` then in the `Actions` dropdown select `Add Mount Points`. Select your `Source` (directory in storage pool where I want the bitcoin data to be located) and your `Destination` (directory in bitcoin jail where I want the bitcoin source directory to be mounted) folders. Click `SAVE`. In my case it looks as in the image below.

![FreeNAS Mount point creation](images/mount_point_create.png) 

It would be a good idea to log into your router and give your bitcoin jail a static IP address. Also forward port 8333 from your WAN to your jail's LAN IP address. For example, my internal IP address assigned to my bitcoin jail is 192.168.84.123

![FreeNAS_Jail_Port_Forward](images/jail_port_forward.png)

*Your router's firmware may look different. This is how it looks on OpenWRT.*
```
Name : bitcoin
Protocol: TCP
External Zone: WAN
External Port: 8333
Internal Zone: LAN
Internal IP address: (inset your jail IP here)
Internal Port: 8333
```


Next: [ [Install Bitcoin](freenas_2_bitcoin.md) ]
