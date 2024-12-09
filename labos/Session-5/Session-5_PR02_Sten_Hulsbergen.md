<img src="file:///C:/Users/stenh/AppData/Roaming/marktext/images/2024-10-16-15-28-07-image.png" title="" alt="" width="186">                                                          Sten Hulsbergen - 20242689

# Session 5

## 1.

- DNS

![](C:\Users\stenh\AppData\Roaming\marktext\images\2024-10-30-14-44-37-image.png)

- HTTP

![](C:\Users\stenh\AppData\Roaming\marktext\images\2024-10-30-14-47-05-image.png)

- TCP

![](C:\Users\stenh\AppData\Roaming\marktext\images\2024-10-30-14-48-05-image.png)

| Layer       | Protocol  |
|:----------- |:--------- |
| Application | HTTP, DNS |
| Transport   | TCP       |
| Network     |           |
| Link        |           |
| Physical    |           |

## 2.

- DNS (**Domain Name System**)
  
  - Linking an easier readable name, called a domain, to an IP-address of a system or service.

- HTTP (**Hypertext Transfer Protocol**)
  
  - Used in communication between webserver and webbrowser. Data can be accessed and send with it.

- TCP (**Transmission Control Protocol**)
  
  - Used in datatransfer on the internet in the form of a stream of data, like a large download. To be assured the datatransfer is complete, this will be used.

## 3.

First the DNS-resolver is being spoken to, followed up by the communication.

In my case, when filtering the destination and source address, out of the 1408 packets there were 33 packets exchanged in my session.

![](C:\Users\stenh\AppData\Roaming\marktext\images\2024-10-30-14-55-17-image.png)

## 4.

The IP-address in Wireshark is `143.129.43.250`. After installing *dnsutils* with `sudo apt install dnsutils` it is possible to do `dnslookup course-3networkarchitecture.ei.fti.uantwerpen.be` which resulted in the same IP-address `143.129.43.250`.

![](C:\Users\stenh\AppData\Roaming\marktext\images\2024-10-30-15-03-29-image.png)
