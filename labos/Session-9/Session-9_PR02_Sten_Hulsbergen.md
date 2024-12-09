<img src="file:///C:/Users/stenh/AppData/Roaming/marktext/images/2024-10-16-15-28-07-image.png" title="" alt="" width="186">                                                          Sten Hulsbergen - 20242689

# Session 9

## 1.

Install a new VM, like in the previous lab, that is connected to the `nat-network` for the `dns-server`, during the configuration set the IP-address to `192.168.200.2`.

## 2.

### DHCP

First, add a new IP in the `reservations-list` in the dhcp-server, this will be `192.168.1.2` that is linked to the `dns-server`.

### DNS

Install all the necessary packages like `sudo, bind9, dnsutils, ...` as the root user. When everything is ready, remove the `nat-network` and add the `internal-network` and change the static IP-address in `/etc/network/interface` to `dhcp`. 

### Forwarder

The forwarders are configured in `/etc/bind/named.conf.options`, the DNS that is used is from Google.

```
forwarders {
        8.8.8.8;
};
```

### Forward zone

The forward zones are configured in `/etc/bind/named.conf.local`, this zone is linked to file `db.dhcp.labnet.local`, which contains the configuration.

```
zone "dhcp.labnet.local" {
    type master;
    file "/etc/bind/db.dhcp.labnet.local";
};
```

The `db.dhcp.labnet.local` is configured with:

- `@ IN SOA ns.labnet.local. root.labnet.local.`: Start of Authority (SOA) record

- `@ IN NS labnet.local.`: Specifies nameserver linked to domain

- `@ IN A 192.168.1.1`: Specifies the IP-address of the domain

- `ns IN A 192.168.1.1`: Specifies the IP-address of the nameserver

```
;
; BIND data file for labnet.local
;
$TTL    604800
@       IN      SOA     ns.labnet.local. root.labnet.local. (
                              2         ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@       IN      NS      labnet.local.
@       IN      A       192.168.1.1
ns      IN      A       192.168.1.1
```

### Reverse zone

The reverse zones are also configured in `/etc/bind/named.conf.local`, this zone is linked to file `db.dns.labnet.local`, which contains the configuration.

```
zone "1.168.192.in-addr.arpa" {
    type master;
    file "/etc/bind/db.dns.labnet.local";
};
```

The `db.dns.labnet.local` is configured with:

- `@ IN SOA ns.dns.labnet.local. admin.dns.labnet.local.`: Start of Authority (SOA) record

- `@ IN NS dns.labnet.local.`: Specifies nameserver linked to domain

- `1 IN PTR dhcp.labnet.local.`: Points the IP-address to the hostname

```
;
; BIND data file for dns.labnet.local
;
$TTL    604800
@       IN      SOA     ns.dns.labnet.local. admin.dns.labnet.local. (
                              2         ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
        IN      NS      dns.labnet.local.
1       IN      PTR     dhcp.labnet.local.
```

## 4.

### Solved nslookup error

When using the `nslookup` command, some error messages where shown. This was caused by the `192.168.1.1` IP-address being in the `domain-name-server1` section, which is not correct.

```
{
    "name": "domain-name-servers",
    "data": "192.168.1.1, 192.168.1.2"
},
```

Removing `192.168.1.1` fixed the output of the `nslookup`.

```
{
    "name": "domain-name-servers",
    "data": "192.168.1.2"
},
```

### Results

As shown bellow the results are correctly matching the exercises outcome.

![](C:\Users\stenh\AppData\Roaming\marktext\images\2024-11-29-13-27-44-image.png)

![](C:\Users\stenh\AppData\Roaming\marktext\images\2024-11-29-13-28-09-image.png)
