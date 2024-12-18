<img src="file:///C:/Users/stenh/AppData/Roaming/marktext/images/2024-10-16-15-28-07-image.png" title="" alt="" width="186">                                                          Sten Hulsbergen - 20242689

# Session 10

## 1.

After completing the installation of the VM, add the `internal-network` with `sudo virsh attach-interface --type network --source internal-network --model virtio router --persistent`.

Add the following settings for the LAN-interface in `/etc/network/interfaces` on the router.

```
allow-hotplug enp7s0
iface enp7s0 inet dhcp
```

Next step is to setup a static IP-address, which is `192.168.1.254`, for the router within the config of the dhcp.

```
{
  "hw-address": "52:54:00:f0:35:36",
  "ip-address": "192.168.1.254"
}
```

![](C:\Users\stenh\AppData\Roaming\marktext\images\2024-12-10-09-25-41-image.png)

## 2.

To allow forwarding on ipv4, edit `/etc/sysctl.conf` and uncomment `net.ipv4.ip_forward = 1`. Save the settings with `sudo sysctl -p`.

![](C:\Users\stenh\AppData\Roaming\marktext\images\2024-12-10-09-26-51-image.png)

Next is adding the NAT-rule and forwarding-rules.

```
iptables -t nat -A POSTROUTING -o enp1s0 -j MASQUERADE
iptables -A FORWARD -i enp7s0 -o enp1s0 -j ACCEPT
iptables -A FORWARD -i enp1s0 -o enp7s0 -m state --state RELATED,ESTABLISHED -j ACCEPT
```

Use `sudo netfilter-persistent save` to save the iptables and `sudo netfilter-persistent reload` to restart the service.

![](C:\Users\stenh\AppData\Roaming\marktext\images\2024-12-10-09-27-40-image.png)

## 3.

Logs of iptables `systemctl status iptables`

![](C:\Users\stenh\AppData\Roaming\marktext\images\2024-12-10-09-28-25-image.png)

Iptables rules `iptables -t nat -L -n -v`

![](C:\Users\stenh\AppData\Roaming\marktext\images\2024-12-10-09-29-05-image.png)

IPv4 forwarding status `sysctl net.ipv4.ip_forward`

![](C:\Users\stenh\AppData\Roaming\marktext\images\2024-12-10-09-29-37-image.png)

## 4.

Checking if the NAT is successful by pinging `8.8.8.8` on both clients.

- Client 1

![](C:\Users\stenh\AppData\Roaming\marktext\images\2024-12-10-09-30-27-image.png)

- Client 2

![](C:\Users\stenh\AppData\Roaming\marktext\images\2024-12-10-09-31-10-image.png)

## 5.

To be able to install packages, some changes have to be made in the `/etc/apt/sources.list`. Uncomment the first line and add the following:

```
#deb cdrom:[Debian GNU/Linux 12.8.0 _Bookworm_ - Official amd64 NETINST with fi>

deb http://deb.debian.org/debian/ bookworm main non-free-firmware
deb-src http://deb.debian.org/debian/ bookworm main non-free-firmware

deb http://security.debian.org/debian-security bookworm-security main non-free->
deb-src http://security.debian.org/debian-security bookworm-security main non-f>

# bookworm-updates, to get updates before a point release is made;
# see https://www.debian.org/doc/manuals/debian-reference/ch02.en.html#_updates>
deb http://deb.debian.org/debian/ bookworm-updates main non-free-firmware
deb-src http://deb.debian.org/debian/ bookworm-updates main non-free-firmware

# This system was installed using small removable media
# (e.g. netinst, live or single CD). The matching "deb cdrom"
# entries were disabled at the end of the installation process.
# For information about how to configure apt package sources,
# see the sources.list(5) manual.
```

Now  `dnsutils` can be installed on both clients. Testing it with `google.com` gave the following results in both clients.

![](C:\Users\stenh\AppData\Roaming\marktext\images\2024-12-10-16-20-18-image.png)

![](C:\Users\stenh\AppData\Roaming\marktext\images\2024-12-10-16-20-03-image.png)
