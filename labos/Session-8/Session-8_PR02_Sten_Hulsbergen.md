<img src="file:///C:/Users/stenh/AppData/Roaming/marktext/images/2024-10-16-15-28-07-image.png" title="" alt="" width="186">                                                          Sten Hulsbergen - 20242689

# Session 8

## 1.

Configuring a `nat-network.xml` and defining a new network `nat-network` by using `sudo virsh net-define nat-network.xml`. Starting and autostart uses `sudo virsh net-start nat-network` and `sudo virsh net-autostart nat-network`.

To be able to to go and download files into `/var/lib/libvirt/images` it is necessary to be superuser. After downloading, installing and configuring the VM, there was a mistake with it's IP-address, which got fixed by changing it from `192.168.200.1` to `192.168.200.2`. 

## 2.

Log in to `root` and run `apt-get install sudo` to be able to use the sudo-command, after that add the *dhcpserver* to *sudoers-group* with `adduser dhcpserver sudo`.

Use `sudo virsh detach-interface dhcp network 52:54:00:ff:d4:5a` to detach `nat-network` from the VM.

Configuring a `internal-network.xml` and defining a new network `internal-network` by using `sudo virsh net-define internal-network.xml`. Starting and autostart uses `sudo virsh net-start internal-network` and `sudo virsh net-autostart internal-network`.

Use `sudo virsh attach-interface --type network --source internal-network --model virtio dhcp --persistent` to attach`internal-network` to the VM.

Make a backup of the current kea-dhcp4 config with `cp /etc/kea/kea-dhcp4.conf /etc/kea/kea-dhcp4.conf.bak` and change the configuration to what is necessary. 

## 3.

After everything is setup and loaded, the interfaces look like the following:

![](C:\Users\stenh\AppData\Roaming\marktext\images\2024-11-21-00-40-51-image.png)

## 4.

After configuring two more VM's, client1 and client2, and connecting them to `internal-network` will do nothing at first.

Use `sudo virsh attach-interface --type network --source internal-network --model virtio client(1/2) --persistent` to attach`internal-network` to the VM.

According to the exercise client1 will get a dynamic IP-address of `192.168.1.10-100` and client2 a static IP-address `192.168.1.111` which is outside the configured range.

### **Client1**

Editing `/etc/network/interfaces` to the following will assign a dynamic IP.

```
# This file describes the network interfaces available on your system
# and how to activate them. For more information, see interfaces(5).

source /etc/network/interfaces.d/*

# The loopback network interface
auto lo
iface lo inet loopback

allow-hotplug enp1s0
iface enp1s0 inet dhcp
```

![](C:\Users\stenh\AppData\Roaming\marktext\images\2024-11-21-01-03-20-image.png)

### **Client2**

Editing `/etc/network/interfaces` to the following will assign a static IP.

```
# This file describes the network interfaces available on your system
# and how to activate them. For more information, see interfaces(5).

source /etc/network/interfaces.d/*

# The loopback network interface
auto lo
iface lo inet loopback

allow-hotplug enp1s0
iface enp1s0 inet static
address 192.168.1.111/24
gateway 192.168.1.254
# dns-* options are implemented by the resolvconf package, if installed
dns-nameservers 192.168.1.2
dns-search labnet.local
```

![](C:\Users\stenh\AppData\Roaming\marktext\images\2024-11-21-01-04-13-image.png)

### Logs

Systemctl shows the current logs of the DHCP-server service and it is clear to see an IP has been automatically allocated.

![](C:\Users\stenh\AppData\Roaming\marktext\images\2024-11-21-01-12-58-image.png)

Journalctl shows all the past logs of the DHCP-server and the same logs appear in it.

![](C:\Users\stenh\AppData\Roaming\marktext\images\2024-11-21-01-11-11-image.png)

### Leases

The `kea-leases4.csv` shows all the leases and it is clear to see `192.168.1.11` has been leased to `52:54:00:BA:58:FF`.

![](C:\Users\stenh\AppData\Roaming\marktext\images\2024-11-21-01-15-49-image.png)

### Renew DHCP

After using `dhclient -r` and `dhclient` on client1, nothing changes, the IP was already dynamic and stayed `192.168.1.11`.

![](C:\Users\stenh\AppData\Roaming\marktext\images\2024-11-21-01-34-38-image.png)

After using `dhclient -r` and `dhclient` on client2 however, did remove the static allocation and changed it to dynamic allocation, the IP-address went from `192.168.1.111` to `192.168.1.10` because of the DHCP-server.

![](C:\Users\stenh\AppData\Roaming\marktext\images\2024-11-21-01-32-36-image.png)
