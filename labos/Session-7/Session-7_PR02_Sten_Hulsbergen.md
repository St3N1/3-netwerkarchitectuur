<img src="file:///C:/Users/stenh/AppData/Roaming/marktext/images/2024-10-16-15-28-07-image.png" title="" alt="" width="186">                                                          Sten Hulsbergen - 20242689

# Session 7

## Exercise 1: Ethernet

### a

The 48-bit Ethernet address from my computer is `10:7C:61:C2:01:97`.

![](C:\Users\stenh\AppData\Roaming\marktext\images\2024-11-13-14-13-07-image.png)

### b

The 48-bit destination address is `00:00:0C:07:AC:03` and it belongs to `All-HSRP-routers_03`.

![](C:\Users\stenh\AppData\Roaming\marktext\images\2024-11-13-14-15-30-image.png)

### c

The hexadecimal value of the Frame type is `0x0800` which is used for `IPv4` and is on the `internet layer`.

![](C:\Users\stenh\AppData\Roaming\marktext\images\2024-11-13-14-18-30-image.png)

### d

The 48-bit destination address is `80:6A:00:6D:E8:9F` and it belongs to `Cisco_6d:e8:9f`.

![](C:\Users\stenh\AppData\Roaming\marktext\images\2024-11-13-14-23-52-image.png)

### e

`O` appears in the 68th byte in the Ethernet frame.

![](C:\Users\stenh\AppData\Roaming\marktext\images\2024-11-13-15-05-55-image.png)

## Exercise 2: ARP

### a

Interface `143.129.40.79` is the current IPv4 address of the Ethernet adapter.

Interface `172.17.144.1` is an internal IPv4 address of my WSL.

- **Internet Address**: IP-address of the device.

- **Physical Address**: MAC-address of the device.

- **Type**: type of ARP entry, which can be dynamic or static.
  
  - Dynamic: entries are automatically added by the system when it communicates with other devices on the network. 
  
  - Static: entries are manually added and remain in the ARP table until they are manually removed.

![](C:\Users\stenh\AppData\Roaming\marktext\images\2024-11-13-14-47-16-image.png)

### b

In `arp -d *`, `-d` means `delete` and `*` means `all`.

![](C:\Users\stenh\AppData\Roaming\marktext\images\2024-11-13-15-11-38-image.png)

### c

**Source address**: `BC:0F:F3:5D:48:AC`, belongs to `HP_5d:48:ac`.

**Destination address**: `FF:FF:FF:FF:FF:FF` is a broadcast.

![](C:\Users\stenh\AppData\Roaming\marktext\images\2024-11-13-16-37-28-image.png)

### d

The hexadecimal value of the Frame type is `0x0806` which is used for `ARP` and is on the `link layer`.

![](C:\Users\stenh\AppData\Roaming\marktext\images\2024-11-13-16-38-51-image.png)

### e

There doesn't seem to be an ARP-reply in my recordings in Wireshark, hereby the results are purely speculative.

**Source address**: `10:7C:61:C2:01:97`, belongs to my computer.

**Destination address**: `BC:0F:F3:5D:48:AC`, belongs to `HP_5d:48:ac`.
