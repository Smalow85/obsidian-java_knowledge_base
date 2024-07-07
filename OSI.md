The open systems interconnection (OSI) model is a conceptual model created by the International Organization for Standardization which enables diverse communication systems to communicate using standard [[Protocol]]s

## What are the 7 layers of the OSI Model?

The seven abstraction layers of the OSI model can be defined as follows, from top to bottom:

![[Pasted image 20240316220427.png]]
#### 7. The application layer
![[Pasted image 20240316220505.png]]
Application layer protocols include [[HTTP]] as well as [[SMTP]] (Simple Mail Transfer Protocol is one of the protocols that enables [email](https://www.cloudflare.com/learning/email-security/what-is-email/) communications).

#### 6. The presentation layer
![[Pasted image 20240316220625.png]]
This layer is primarily responsible for preparing data so that it can be used by the application layer; in other words, layer 6 makes the data presentable for applications to consume. The presentation layer is responsible for translation, [encryption](https://www.cloudflare.com/learning/ssl/what-is-encryption/), and compression of data.

#### 5. The session layer
![[Pasted image 20240316220710.png]]
This is the layer responsible for opening and closing communication between the two devices. The time between when the communication is opened and closed is known as the session. The session layer ensures that the session stays open long enough to transfer all the data being exchanged, and then promptly closes the session in order to avoid wasting resources.

#### 4. The transport layer
![[Pasted image 20240316220827.png]]
Layer 4 is responsible for end-to-end communication between the two devices. This includes taking data from the session layer and breaking it up into chunks called segments before sending it to layer 3. The transport layer on the receiving device is responsible for reassembling the segments into data the session layer can consume.
Transport layer protocols include the Transmission Control Protocol ([[TCP]]) and the User Datagram Protocol ([[UDP]])
#### 3. The network layer
![[Pasted image 20240316222138.png]]
The network layer is responsible for facilitating data transfer between two different networks. If the two devices communicating are on the same network, then the network layer is unnecessary. The network layer breaks up segments from the transport layer into smaller units, called packets, on the sender’s device, and reassembling these packets on the receiving device. The network layer also finds the best physical path for the data to reach its destination; this is known as routing.

Network layer protocols include IP (see [[IP Address]]), the Internet Control Message Protocol ([[ICMP]]), the Internet Group Message Protocol ([[IGMP]]), and the [[IPsec]] suite.
#### 2. The data link layer
![[Pasted image 20240316222636.png]]
The data link layer is very similar to the network layer, except the data link layer facilitates data transfer between two devices on the _same_ network.
#### 1. The physical layer
![[Pasted image 20240316222711.png]]
This layer includes the physical equipment involved in the data transfer, such as the cables and [[router]]s. This is also the layer where the data gets converted into a bit stream, which is a string of 1s and 0s. The physical layer of both devices must also agree on a signal convention so that the 1s can be distinguished from the 0s on both devices.
