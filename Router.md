## What is routing?
Network routing is the process of selecting a path across one or more networks. The principles of routing can apply to any type of network, from telephone networks to public transportation. In packet-switching networks, such as the Internet, routing selects the paths for Internet Protocol ([[IP Address]]) packets to travel from their origin to their destination. These Internet routing decisions are made by specialized pieces of network hardware called routers.

![[Pasted image 20240318221839.png]]
## How does routing work?
Routers refer to internal routing tables to make decisions about how to route packets along network paths. A routing table records the paths that packets should take to reach every destination that the router is responsible for. Think of train timetables, which train passengers consult to decide which train to catch. Routing tables are like that, but for network paths rather than trains.

Routers work in the following way: when a router receives a [[Packet]], it reads the headers* of the packet to see its intended destination, like the way a train conductor may check a passenger's tickets to determine which train they should go on. It then determines where to route the packet based on information in its routing tables.

Routers do this millions of times a second with millions of packets. As a packet travels to its destination, it may be routed several times by different routers.

Routing tables can either be static or dynamic. Static routing tables do not change. A network administrator manually sets up static routing tables. This essentially sets in stone the routes data packets take across the network, unless the administrator manually updates the tables.

Dynamic routing tables update automatically. Dynamic routers use various routing protocols (see below) to determine the shortest and fastest paths. They also make this determination based on how long it takes packets to reach their destination — similar to the way Google Maps, Waze, and other GPS services determine the best driving routes based on past driving performance and current driving conditions.

Dynamic routing requires more computing power, which is why smaller networks may rely on static routing. But for medium-sized and large networks, dynamic routing is much more efficient.

_*Packet headers are small bundles of data attached to packets that provide useful information, including where the packet is coming from and where it is headed, like the packing slip stamped on the outside of a mail parcel._