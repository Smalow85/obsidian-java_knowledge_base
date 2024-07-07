In networking, a three-way handshake is a process that's used to initiate a connection in a transmission control protocol/internet protocol ([[TCP]]/[[IP]]) network. TCP is responsible for ensuring data is delivered correctly between computers on an internet network. The three-way handshake involves the following three steps: synchronize (SYN), synchronize-acknowledge (SYN-ACK), and acknowledge (ACK).

**Here's how it works:**

**1. SYN:** The initiating computer (or active client) sends a synchronize sequence number (SYN) packet to the receiving computer (usually a server) with the value set to an arbitrary number (e.g. 100) to “ask” if any open connections are available.

**2. SYN-ACK:** If the receiving computer (also known as a passive client) has open ports that can accept the connection, it sends back a synchronize-acknowledge (SYN-ACK) packet to the initiating computer. The packet includes two numbers: the receiving computer’s own SYN, which can be any arbitrary number as well (e.g. 200), and the ACK number, which is the initiating computer’s SYN plus one (e.g. 101). 

**3. ACK:** The initiating computer (active client) then sends an acknowledge sequence number (ACK) packet back to the receiving computer, acknowledging receipt of the SYN-ACK packet. The packet value is set to the receiving computer’s SYN (sent in step two) plus one again (e.g. 201). With this final step, the connection establishes, and data transmission can begin.

![[Pasted image 20240318223814.png]]