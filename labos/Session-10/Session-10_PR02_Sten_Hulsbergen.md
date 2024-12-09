<img src="file:///C:/Users/stenh/AppData/Roaming/marktext/images/2024-10-16-15-28-07-image.png" title="" alt="" width="186">                                                          Sten Hulsbergen - 20242689

# Session 10

## 1.

After completing the installation of the VM, add the `internal-network` with `sudo virsh attach-interface --type network --source internal-network --model virtio router --persistent`.

Add the following settings for the LAN-interface in `/etc/network/interfaces` on the router.

```
allow-hotplug enp7s0
iface enp7s0 inet dhcp
```

Next step is to setup the router with a static IP-address in config of the dhcp.

```
{
  "hw-address": "52:54:00:f0:35:36",
  "ip-address": "192.168.1.254"
}
```

(foto ip -c a)

## 2.

To allow forwarding on ipv4, edit `/etc/sysctl.conf` and uncommend `net.ipv4.ip_forward = 1`. Save the settings with `sudo sysctl -p`.

(foto sudo sysctl -p)

Next is adding the NAT-rule and forwarding-rules.

```
iptables -t nat -A POSTROUTING -o enp1s0 -j MASQUERADE
iptables -A FORWARD -i enp7s0 -o enp1s0 -j ACCEPT
iptables -A FORWARD -i enp1s0 -o enp7s0 -m state --state RELATED,ESTABLISHED -j ACCEPT


```

Use `sudo netfilter-persistent save` to save the iptables and `sudo netfilter-persistent reload` to restart the service.

(foto save en reload)

## 3.

- Logs of iptables `systemctl status iptables`

(foto)

- Iptables rules `iptables -t nat -L -n -v`

(foto)

- IPv4 forwarding status `sysctl net.ipv4.ip_forward`

(foto)

## 4.

Checking if the NAT is successful by pinging `8.8.8.8` on both clients.

- Client 1

(foto ping)

- Client 2

(foto ping)
