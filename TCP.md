Transmission Control Protocol (TCP) is a standard that defines how to establish and maintain a network conversation by which applications can exchange data. TCP resides at the transport layer of the Open Systems Interconnection ([[OSI]]) model.

## How Transmission Control Protocol works

TCP is a connection-oriented [[Protocol]], which means a connection is established and maintained until the applications at each end have finished exchanging messages.

TCP performs the following actions:

- Establishes through a [[Three-way handshake]] where the sender and the receiver exchange control [[Packet]]s to synchronize and establish a connection.
- Determines how to break application data into packets that networks can deliver.
- Sends packets to, and accepts packets from, the network layer.
- Manages flow control.
- Handles retransmission of dropped or garbled packets, as it's meant to provide error-free data transmission.
- Acknowledges all packets that arrive.
- Terminates connection once data transmission is complete through a four-way handshake.

When a web server sends an [[HTML]] file to a client, it uses the [[HTTP]] to do so. The HTTP program layer asks the TCP layer to set up the connection and send the file. The TCP stack divides the file into data packets, numbers them and then forwards them individually to the IP layer for delivery.