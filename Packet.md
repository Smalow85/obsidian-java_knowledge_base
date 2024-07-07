A packet is the basic unit of information transmitted over the internet. Splitting information up into small, digestible pieces allows the network’s capacity to be used more efficiently.

A packet has two parts. The [header](https://en.wikipedia.org/wiki/IPv4#Header) contains information that helps the packet get to its destination, including the length of the packet, its source and destination, and a checksum value that helps the recipient detect if a packet was damaged in transit. After the header comes the actual data. A packet can contain up to 64 kilobytes of data, which is roughly 20 pages of plain text.

If internet routers experience congestion or other technical problems, they are allowed to deal with it by simply discarding packets. It’s the sending computer’s responsibility to detect that a packet didn’t reach its destination and send another copy. This approach might seem counterintuitive, but it simplifies the internet’s core infrastructure, leading to higher performance at lower cost.

![[Pasted image 20240316215122.png]]