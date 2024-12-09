<img src="file:///C:/Users/stenh/AppData/Roaming/marktext/images/2024-10-16-15-28-07-image.png" title="" alt="" width="186">                                                          Sten Hulsbergen - 20242689

# Session 6

## Exercise 1: TCP

### a.

#### 1.

The name of the server is `course-3networkarchitecture.ei.fti.uantwerpen.be`.

![](C:\Users\stenh\AppData\Roaming\marktext\images\2024-11-06-14-26-02-image.png)

#### 2.

The protocol HTTP uses is `TCP`.

![](C:\Users\stenh\AppData\Roaming\marktext\images\2024-11-06-14-30-18-image.png)

### b.

The first 3 frames will make a handshake with TCP between client and server.

![](C:\Users\stenh\AppData\Roaming\marktext\images\2024-11-10-17-20-01-image.png)

### c.

**Frame 39**: The client sends a SYN packet to the server on port 443.

**Frame 41**: The server responds with a SYN-ACK packet to the client.

**Frame 42**: The client sends an ACK packet back to the server, completing the handshake.

![](C:\Users\stenh\AppData\Roaming\marktext\images\2024-11-10-17-29-15-image.png)

### d.

**Total bytes**: 66B + 66B + 54B = 186B

![](C:\Users\stenh\AppData\Roaming\marktext\images\2024-11-10-17-29-58-image.png)

**Epoch time frame 39**: 1730898701,799112s

![](C:\Users\stenh\AppData\Roaming\marktext\images\2024-11-10-17-33-05-image.png)

**Epoch time frame 42**: 1730898701,801467s

![](C:\Users\stenh\AppData\Roaming\marktext\images\2024-11-10-17-33-18-image.png)

**Epoch time delta**: 1730898701,801467s - 1730898701,799112s = 0,002355s

**Throughput**: 186B / 0,002355s = 78980B/s = 78,98MB/s

## Exercise 2: UDP

### a.

There are 4 fields in the UDP header:

- **Source Port**: port of the sending application.

- **Destination Port**: port of the receiving application.

- **Length**: length of the UDP header and data.

- **Checksum**: used for error-checking of the header and data.

![](C:\Users\stenh\AppData\Roaming\marktext\images\2024-11-10-17-38-57-image.png)
