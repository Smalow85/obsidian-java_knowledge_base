The User Datagram Protocol, or UDP, is a communication [[Protocol]] used across the Internet for especially time-sensitive transmissions such as video playback or [[DNS]] lookups. It speeds up communications by not formally establishing a connection before data is transferred. This allows data to be transferred very quickly, but it can also cause packets to become lost in transit — and create opportunities for exploitation in the form of DDoS attacks.

#### TCP vs. UDP

UDP is faster but less reliable than [[TCP]], another common transport protocol. In a TCP communication, the two computers begin by establishing a connection via an automated process called a ‘[[Three-way handshake]].’ Only once this handshake has been completed will one computer actually transfer data packets to the other.

UDP communications do not go through this process. Instead, one computer can simply begin sending data to the other:

![[Pasted image 20240318224952.png]]

## How does UDP work?

Following steps explain how UDP works:

### **Step 1: Data Segmentation**

Whenever an application wants to send data using UDP, it breaks down the data into smaller units called datagrams. Each datagram includes:

- Source Port: The sender’s port number.
- Destination Port: The port number of the recipient application.
- Length: The length of the datagram, including the header and data.
- Checksum (optional): A value used for data integrity verification.

### **Step 2: Sending the Datagram**

The application hands the datagram over to the UDP layer, which encapsulates it into a UDP packet. The UDP packet’s header is added, including the source and destination port numbers.

1. The UDP packet is then passed to the IP (Internet Protocol) layer for further encapsulation. The IP layer adds its own header, including the source and destination IP addresses.
2. The final packet, now including the UDP header and IP header, is sent over the network.

### **Step 3: Network Transmission**

The packet travels through the network infrastructure, including routers and switches, based on the destination IP address. Routers make forwarding decisions to route the packet closer to its destination.

### **Step 4: Receiving at the Destination** 

When the packet reaches its destination device, it is processed by the network stack. The IP layer decapsulates the packet, removing its header and extracting the encapsulated UDP packet.

1. The UDP layer processes the UDP packet, extracting the UDP header and data payload.
2. The UDP packet’s data payload is then passed to the application layer, where it is delivered to the appropriate application based on the destination port number.

### **Step 5: No Acknowledgment or Connection** 

UDP does not require a prior connection setup, so there is no handshaking process before data transmission.

1. UDP does not provide acknowledgments for received packets or retransmit lost packets. It operates on a “best-effort” basis, meaning it sends the data and hopes it reaches its destination.

### **Step 6: Error Handling (Optional)** 

If the sender has included a checksum in the UDP header, the recipient can use it to check the integrity of the received data. If the data is corrupted during transmission, it may be discarded

![[Pasted image 20240318224344.png]]