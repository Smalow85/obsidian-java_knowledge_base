## Terminology

- [[Packet]]: A small unit of data that is transmitted over the internet.
- [[Router]]: A device that directs packets of data between different networks.
- [[IP Address]]: A unique identifier assigned to each device on a network, used to route data to the correct destination.
- [[Domain Name]]: A human-readable name that is used to identify a website, such as google.com.
- [[DNS]]: The Domain Name System is responsible for translating domain names into IP addresses.
- [[HTTP]]: The Hypertext Transfer Protocol is used to transfer data between a client (such as a web browser) and a server (such as a website).
- [[HTTPS]]: An encrypted version of HTTP that is used to provide secure communication between a client and server.
- [[SSL and TLS]]: The Secure Sockets Layer and Transport Layer Security protocols are used to provide secure communication over the internet.
---
## Protocols

A [[Protocol]] is a set of rules and standards that define how information is exchanged between devices and systems.

| Protocol Layer | Comments | Example |
| ---- | ---- | ---- |
| Application Protocols Layer | Protocols specific to applications such as WWW, e-mail, FTP, etc. | [[HTTP]] |
| Transmission Control Protocol Layer | TCP directs packets to a specific application on a computer using a port number. | [[TCP]] |
| Internet Protocol Layer | IP directs packets to a specific computer using an IP address. | [[IP Address]] |
| Hardware Layer | Converts binary packet data to network signals and back.  <br>(E.g. ethernet network card, modem for phone lines, etc.) |  |
## Transfer data through the Internet

![[Pasted image 20240316213823.png]]

1. The message would start at the top of the protocol stack on your computer and work it's way downward.
2. If the message to be sent is long, each stack layer that the message passes through may break the message up into smaller chunks of data. This is because data sent over the Internet (and most computer networks) are sent in manageable chunks. On the Internet, these chunks of data are known as **packets**.
3. The packets would go through the Application Layer and continue to the TCP layer. Each packet is assigned a **port number**. Ports will be explained later, but suffice to say that many programs may be using the TCP/IP stack and sending messages. We need to know which program on the destination computer needs to receive the message because it will be listening on a specific port.
4. After going through the TCP layer, the packets proceed to the IP layer. This is where each packet receives it's destination address, 5.6.7.8.
5. Now that our message packets have a port number and an IP address, they are ready to be sent over the Internet. The hardware layer takes care of turning our packets containing the alphabetic text of our message into electronic signals and transmitting them over the phone line.
6. On the other end of the phone line your ISP has a direct connection to the Internet. The ISPs **router** examines the destination address in each packet and determines where to send it. Often, the packet's next stop is another router. More on routers and Internet infrastructure later.
7. Eventually, the packets reach computer 5.6.7.8. Here, the packets start at the bottom of the destination computer's TCP/IP stack and work upwards.
8. As the packets go upwards through the stack, all routing data that the sending computer's stack added (such as IP address and port number) is stripped from the packets.
9. When the data reaches the top of the stack, the packets have been re-assembled into their original form, "Hello computer 5.6.7.8!"






