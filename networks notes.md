Pieter Benjamin  
pieterb@cs.washington.edu  
UW CSE 461 Au20
# Physical Layer
- Physical layer is about the physical mechanisms used to send bit sequences to different computers
## Latency
- Transmission Delay (time to put m bits on the wire)
  - T-delay = $\frac{M (bits)}{Rate (bits/sec)} = \frac{M}{R}$ seconds
- Propagation delay (time for bits to propagate across wire)
  - P-delay = $\frac{Length}{\text{speed of signals}} = \frac{Length}{\frac{2}{3} C} = D$ seconds
- Our total latency is equal to $\fbox{$L = \frac{M}{R} + D$}$
- Bandwidth-Delay Product
  - The amount of data in flight is the BD product = $\fbox{$BD = R*D$}$
  - measured in bits or messages
  - small for LANs, but large for "long fat" pipes
## Transmission Media (guided and unguided)
- The physical layer transmits bits from one machine to another
- You can get massive bandwidth by loading a ton of CDs or tapes into a truck
and transporting them cross country
- However, the delay is massive when doing so, and we often prefer lower
bandwidth/higher speed connections
- Links which broadcast in both directions simultaneously are called *full-duplex*
  - *half-duplex* do one at a time, and *simplex* only do one direction
- Wires (twisted pair)
  - A twisted pair consists of two insulated copper wires (typically 1 mm thick)
  - When twisted in a helical form, the waves from different wires cancel out and radiate less effectively
  - Signals are usually transmitted as the difference in voltage between two wires (which means noise has less of an impact)
  - Telephones are the number one users of twisted cables
  - Can run up to several km before needing a repeater
  - These can achieve several Mbps over a few km, and are very cheap (therefore popular)
  - Category 5 cabling ("Cat 5") is garden variety right now, and replaced Category 3 by introducing more twists which allowed for less crosstalk, and faster transmission
  - Newer cables have shielding on the individual twists, which also introduces higher quality transmission
- Wires (coaxial cable)
  - better bandwidth and shielding than twisted pair
  - 50 ohm cable often digital, 75 ohm often analog (historical  more than mathematic)
  - In the  90s, TV providers offered internet access over coax cables, which made them more important
  - largely replaced by fiber glass
- Fiber
  - we are nowhere close to achieving the 50Tbps maximum fiber could offer because of the time it takes to convert between electrical and optical signals
  - Very thin inner core of glass which has light signals travel through it
  - Multimode fiber has several rays bouncing inside at many different angles
  - Single-mode fiber has a very small diameter and the light propagates in a straight line
  - This is more expensive, but are very fast and go can 100km without amplification
  - Three main ways to connect these cables
    1. A connector (~10-20% light loss, easy to reconfigure)
    2. Mechanical splice (which puts two carefully cut ends right up next to each other, ~10% loss and 5 minutes)
    3. Fusion melts the two bits of the wire to form a solid connection (almost as good as continuous wire, but small attenuation occurs)
  - Comparison with copper
    - Much higher bandwidth
    - repeaters needed every 50km (vs 5)
    - much less interference
    - more lightweight
    - not as familiar (not as many skilled engineers)
    - damaged easily by being bent too much
- Wireless
  - Began in the Hawaiian Islands where the Pacific Ocean prevented physical cables from being laid down
  - Goes in many directions
  - Many signals at the same time causes collisions - we need to coordinate and share the wireless channel amongst multiple devices
  - FCC regulates frequency in US
  - WiFi permitted at 2.4 and 5 Ghz
  - WiFi and Bluetooth are unlicensed band (like low frequencies such as HAM radio)
    - If you broadcast at 2.4 and use a microwave oven, you get a lot of interruption (especially for older models)
    - Companies didn't want to bid for 2.4 since there was so much interference
    - Government opened it up for innovation and personal use
    - This model of unlicensed use was extremely popular and _did_ lead to innovation, so the government opened up the 5g band as well
  - Why not just keep going to higher and higher frequencies?
    - At really high frequencies, your signal won't go through walls
- Fourier Analysis
  - Any signal can be represented as the sum of a number of sines and cosines
  - $g(t) = \frac{1}{2}c + \sum\limits_{n=1}^\infty a_nsin(2\pi nft) + \sum\limits_{n=1}^\infty b_ncos(2\pi nft)$
  - When you lose higher bandwidth (fewer frequencies) you lose some of the signal
  - A change from 0 $\rightarrow$ 1 (or 1 $\rightarrow$ 0) is a very abrupt change, and you need a fast moving frequency to be able to represent such a change
  - If you are working at lower frequencies, you will smooth out your signal and it will look less like what you actually wanted to transmit
  - In practice, you have much less
- Shannon Capacity
  - In 1948, Claude Shannon proved the max capacity for a channel is $\fbox{$Blog_2(1 + \frac{S}{BN})\frac{bits}{sec}$}$
    - $S = signal$ $strength$
    - $BN = bandwidth * noise$
    - proving this led to the creation of the field of information theory
    - Say you are 10 km (far) away, and $\frac{S}{N} \rightarrow 0$, what can you do to increase your data rate ($\frac{bits}{sec}$) the most?
    - You could increase your signal strength or the bandwidth
    - Note - when $X \rightarrow 0$, you have $log_2(1 + X) \approx X$
    - Therefore, when $\frac{S}{N} \approx 0$, you have $C \approx B*\frac{S}{BN} \approx \frac{S}{N}$ - so increasing bandwidth won't make a huge difference (so you should increase your power)
    - Now say you are super close (10m) and your signal to noise ratio is pretty good
    - Increasing your bandwidth is the way to go here since S is a logarithmic term while B is linear
  - Bluetooth has a bandwidth of 1 Mhz, WiFi has 20 Mhz
    - Increasing the bandwidth of your router is what gives such a huge increase in $\frac{bits}{sec}$
  - Say you have two nodes (A and B) broadcasting to a central node with the same signal to noise ratio. If you alternate receiving between A and B at a regular interval (i.e. equal time), your (upper bound for) data rate of the whole network is $R_N = R_A + R_B = (\frac{1}{2}Blog_2(1 + \frac{S}{BN})\frac{bits}{sec} + \frac{1}{2}Blog_2(1 + \frac{S}{BN})\frac{bits}{sec}) = Blog_2(1 + \frac{S}{BN})\frac{bits}{sec}$
    - If instead, you gave A the bottom half of your frequency to transmit on, and B the top half, you would have $R_N = R_A + R_B = (\frac{B}{2}log_2(1 + \frac{S}{\frac{B}{2}N})\frac{bits}{sec} + \frac{B}{2}log_2(1 + \frac{S}{\frac{B}{2}N})\frac{bits}{sec}) = Blog_2(1 + \frac{2S}{BN})\frac{bits}{sec}$
    - Note that frequency division gives twice (in the logarithmic term) as much of an improvement as time division
    - This is because you are effectively giving twice the power into the system (assuming A and B were using the same amount of power)


# Link Layer
- The Link Layer is about _how_ to send bits across the channels available
- The physical layer can detect when frames start/stop by looking at how much energy is in the system (there is none when you are not sending anything)

## Framing Methods
- Byte Count (not great)
  - Start each frame with a length field
  - The issue is that noise can mess up your header, and if just one bit gets messed up the whole sequence is now illegible
- Byte Stuffing
  - Put a special flag which marks the start and end of the frame
  - The payload itself may have the flag as a bit sequence!
  - You need to escape those bits, but then also need to escape the escape itself (if that sequence also occurs)
  - [A][ESC][FLAG][ESC][B] $\rightarrow$ [A][ESC][ESC][ESC][FLAG][ESC][ESC][B]
  - This is a little challenging and has a lot of overhead
- Bit Stuffing
  - Similar to byte stuffing but at bit level
  - You can have six 1s as a flag, then after five 1s a 0 is inserted
  - On receiving, you delete all 0s after five 1s
  - Longer flags have lower probabilities of occurring (each flag of length $n$ has probability $\frac{1}{2^n}$)
- What happens if you receive bits in error?
  - You want to detect the error, and be able to correct for it after receiving
  - In the case that your receiver cannot recover from your error, you need to be able to retransmit
  - Need to balance the ability to detect/correct errors with computation time
  - If you repeat your message $n$ times, and your channel can introduce a maximum $n$ errors, you can detect $n - 1$ errors, but correct none of them
  - In this class, we assume deterministic errors (known maximum) but irl, you do probabilistic models

## Error Detection
- For a D-bit message, you will have R check bits, where $R = f(D)$
- Then, all valid codewords are all $D + R$ permutations of $x \in \{1, 0\}$ where the last $R$ bits are the correct output of $f(D)$
  - This means there are only $2^D$ correct codewords out of $2^{D + R}$
  - Therefore the probability of a randomly selected codeword being correct is $\frac{2^D}{2^{D+R}} = \frac{2^D}{2^D2^R} = \fbox{$\frac{1}{2^R}$}$
- Hamming Distance
  - Minimum number of bits needed to be flipped to convert $D_1 \rightarrow D_2$
  - The Hamming Distance of a code is the minimum value of $D_i, D_j \in Code, D_i \neq D_j \mid Distance(D_i, D_j)$
  - For a distance of up to $d + 1$, you can can always detect up to $d$ errors
    - This is because your set of legal codewords is known in advance, so it would take $d + 1$ bitflips to go from a valid codeword to an invalid one
    - Imagine your point falling on 2x2 grid of uniformly separated points (i.e. any $(x_i, y_i) \mid x_i, y_i \in integers$) and think of the distance between points. You are guaranteed to be able to detect any errors which move your point off of the set of integers.
  - A code distance of $2d + 1$ can detect and correct $d$ errors by mapping to closest codeword
- Parity Bit
  - You could add one final bit which is the result of xoring all other bits with each other (Hamming Distance of two)
  - This gives a Hamming distance of 2 since two bits would need to be flipped to maintain the correct parity bit
  - This is the same distance as sending the entire message twice
  - As shown above, can detect 1 error and correct 0
- Internet Checksum
  - More popular than a single parity bit
  - You arrange data into 16 bit words, then add them all up (re-adding any overflow), then complement (negate) to get your sum
  -  On the receiving end, you add all the words back up (including the carryout) and the final result  should be 0 since you negated your sum
  - Hamming distance is still only 2
- Cyclic Redundancy Check
  - Do polynomial division with some bitwise representation of a polynomial, then append the remainder onto the original message
  - Hamming distance 4
- CRC is often used at the link layer, Checksum used at the internet (IP, TCP, UDP, but is weak) and parity bits are rarely used
- Hamming codes have a hamming distance of 3
  - You add $k$ check bits, using the equation $n = 2^k - k - 1$
  - You add check bits at all the powers of 2 positions (e.g. 1, 2, 4, 8, 16...)
    - To be more verbose, bit, the $i^{th}$ cehck bit goes at index $2^i$, using 1-based indexing
  - The check bit in position $i$ is the result of xoring all bits at positions $j \mid binary(j)[i] == 1$ (using 1-based indexing from the right for the binary representation of j)
    - Again to be more verbose, consider all indices $j$ of your message. The $i^{th}$ check bit will be xored with the bits at all indices $j$ if the binary representation of the index $j$ has a 1 in the $i^{th}$ bit from the right
    - The values at the indices of the check bits in your outgoing message may change as you do your xoring (see below)
  - Example - send 0101 $\rightarrow n = 2^k - k - 1 \rightarrow k = 3$
    - $p_1p_20p_4101$
    - notice that the third bit is in the fourth (1-based) index because $2^{3-1} = 4$
    - $p1 = p_1 \oplus p_3 \oplus p_5 \oplus p_7$, because $1 = 001$, $3 = 011$, $5 = 101$, $7 = 111$ (notice the $x...x...1$ pattern where there is a 1 in the $i^{th}$ index of the binary representations of index $j$)
    - $p2 = p_2 \oplus p_3 \oplus p_6 \oplus p_7$
    - $p4 = p_4 \oplus p_5 \oplus p_6 \oplus p_7$
    - initially assume that all check bits are 0, which gives us the following expansion
    - $p_1 = 0 \oplus 0 \oplus 1 \oplus 1 \rightarrow 0p_20p_4101$
    - $p_2 = 0 \oplus 0 \oplus 0 \oplus 1 \rightarrow 010p_4101$
    - $p_4 = 0 \oplus 1 \oplus 0 \oplus 1 \rightarrow \fbox{0100101}$
    - Receiver will compute the same $p_i$ bits, and if there is at most one bit flipped, $p_4p_2p_1$ treated as a binary number will give the index of the erroneous bit
  - This code is rad because the number of check bits needed is logarithmic in the number of bits you want to transmit (as opposed to linear)
- Error correction codes are typically only busted out at the noisy physical layer when you are almost expecting something  to go wrong
- Detection is used at the link layer and just requests retransmission to correct things
- Every layer should have reliability/correctness guarantees
## Framing Methods
- ARQ (Automatic Repeat reQuest)
  - Ask for retransmission when you can't recover from an error
  - Lots of complications with timeouts and when to wait/send request
  - Duplicates
    - Acknowledgment for packet may get received
    - Receiver will not know if new frame is repeat if the previous frame was not acked
    - To get around this, we use sequence numbers for frames
    - Stop and wait can use just a single bit to distinguish between alternating frames
    - This pretty much solves the lost ack problem
- Stop/Wait
  - The above alternating bit label for frame
  - not great when you have really large round-trip time because of how much you sit around waiting
- Sliding Window
  - Generalization of stop/wait
  - You have a window of $w$ frames, and continually send frame $w, w+1, w+2, ... , w+n$ until all are acked
- Multiplexing
  - We have been talking about a single link/pipe, but in reality you need to share resources to be as effecient as possible
  - Two popular ones are Time Division Multiplexing (TDM) and Frequency Division Multiplexing (FDM)
  - In TDM, you send at a high rate at a fraction of the time, and in FDM you are continuously broadcasting at a low rate
  - These two strategies are commonly used in continuous traffic/fixed number of user situations
    - TV and Radio as well as 2G cellular employ this strategy
## Multiplexing Network Traffic
- If you think about how you use your phone to brose the internet, it is usually in focused bursts of requests rather than a continuous stream
  - As a consequence, most sources cannot always precisely predict how their resources should be allocated
- Multiple access schemes use statistics to multiplex resources
- Aloha Network
  - Seminal network connecting Hawaiian islands in the 1960s
  - Nodes always send data
  - When a collision occurs, just wait a random amount of time and then retransmit
  - This is simple and decentralized, but only works well under low loads
  - At high loads, you only achieve ~18% efficiency
  - Can improve to ~36% by aligning transmitters to pre-specified time slots so that there is some amount of synchrony
- Classic Ethernet
  - ALOHA inspired Bob Metcalfe to invent Ethernet for LANs in 1973
  - Nodes share ether (coax cable up to 10 Mbps)
  - CSMA (Carrier Sense Multiple Access)
    - Listen to the wire, and only transmit if there is no traffic
    - Does not eliminate all collisions (what happens if $n$ nodes all want to transmit, and start at exactly the same time?)
    - Also has the issue of not detecting another node's message already on the wire because of the physical delay of sending a message
    - Only great when $bandwidth * delay$ product is small
    - Can add Collision Detection by listening to the wire with your receiver - if you receive what you transmit, then you're all clear but when things get garbled you know that someone else is transmitting and you need to abort (jam) your transmission
    - You also want _all_ the transmitting nodes to know when a collision has occurred, which can be accomplished by specifying a minimum frame size that takes $2*RTT$ transmit, so no node can finish sending a frame before a collision happens
- Binary Exponential Backoff
  - This gets at the issue of not having a centralized controller who coordinates when to retransmit
  - Exponentially back off every time you have a collision (double the interval every time you collide)
- Wireless Complications
  - Nodes may have different areas of coverage - doesn't fit Carrier sense
  - Nodes can't hear while sending - no collision detection
    - This is because of the inverse strength of signals over time - like trying to hear someone whisper while you are speaking through a megaphone
- Hidden Terminal Problem
  - If you have nodes [A] [B] [C] [D], when A and B are transmitting, they are hidden from each other but collide at B and can't do any collision sensing
- Exposed Terminal Problem
  - Using the same nodes as above, if B and C sense they are colliding, but want to transmit to A and D respectively, they aren't actually colliding at their destinations but will backoff anyway (which is pointless)
- MACA (Multiple Access with Collision Advoidance)
  - Wireless handshake which is the wireless equivalent of CMSA/CD
  - Very short packets are used to reserve the medium to prevent large loss of transmission during collisions
  1. Sender transmits RTS (request to send), and if the receiver doesn't detect any collisions
  2. The receiver replies with a CTS message (clear to send) with a sender ID/interval to transmit over
  3. the original transmitter sends over its data while other nodes stay silent
  - Collisions with RTS packets can still happen, but we have minimized the size to minimize the loss of effort
- MACA solves the hidden terminal problem, but not the exposed
- WiFi uses a mixture of all these strategies (CSMA/CA with BEB and RTS/CTS)
## Switches
- Basis of modern (switched) ethernet
- Hosts are wired to ethernet switches with twisted pair cables
- Switch serves to connect the hosts
- Routers span the network and link layers
- A hub/repeater spans the physical layer
  - All ports are wired together to a single point in a hub
  - Effectively a single, shared wire
  - Physical entity which takes all incoming packets and forwards to all outgoing ports
- Switches span the link layer
  - Doesn't just blindly broadcast
  - Uses outgoing address to direct packet to correct port
  - Ports are full-duplex (send and receive)
  - No multiple access control, just forwarding
- Implemented using buffers
  - If packets are too big, or buffer is too full, you are forced to drop the packet
  - You don't want massive buffers (which minimizes loss) because the queues would go through the roof and the average wait time of a packet would increase
  - On the flipside, a too-tiny buffer size will drop _too many_ packets
- The primary advantage is the reliability/ease of installation
  - If one link breaks, it's easier to find/debug the issue
  - Also super scalable
    - If you share a 100 Mbps ethernet cable between 10 people, you would only see 10 Mbps on average per user
    - Switches offer 100 Mbps per port in that case
- Backward learning
  - Switch has a port and address table
  - Whenever a packet comes from an input port, you check the source address and map it to that port (if you don't already have it mapped)
  - This way, you "learn" which port goes to which address
  - What if you have multiple switches?
    - watch the lecture idk how to transcribe this lol - basically you start off knowing nothing about the ports, so when your first packet is sent, you blast it on all ports and then write down at each stage which port A originates from
  - if you send a packet from A to D, then change A's port and don't send any more packets, D will try to send a packet to A and the port will be lost because the port table is outdated
  - Loops are totally valid to add, but they cause issues for the backward learning algorithm
  - you get around it by using spanning trees
- Spanning Tree solution to loops in a Network
  - Acyclic subset of edges which contains an end of an edge at every node in the graph
  - Rules of the distributed game
    - All switches run same algorithm
    - No information to start with
    - All operate in parallel and send messages
    - Always searching for the best solution
  - Radia Perlman mother of internet for her work on this solution in the ARPANET
    1. Choose a root node (switch with lowest address)
    2. Grow tree with by connecting nodes with minimum distance from the root (use lower addresses to break ties)
    3. When you have made a spanning tree, turn off any ports which are redundant
    - Each switch believes it is the root
    - I can't transcribe this. Watch the example from 10/26 around 6-8 minutes in.
    - Essentially, every node believes that it is the root at first and will broadcast as such. But there are local minimums for the addresses which will win the ties with their neighbors and become the local root. At each iteration, that information will spread out to all of the neighbors of the nodes who have found a root, and the distance will increase by 1.
    - The algorithm continuously runs, with timeouts, and if a node goes offline the process is repeated and other nodes forget about it
  - This works at the link layer, but at the internet layer it is not great to be turning off links and not utilizing resources
# Network Layer
- The network layer needs to scale up, support diverse technologies, and use link bandwidth well
- Routers are used here to send packets across networks
- Networks are built with links and switches
- Previous example does not scale for massive networks - you would need a routing table and spanning tree for the entire world
- Network layer provides the ability to interoperate between different technologies
- IP is the technology which allows for internetworking
- Network layer is pretty constant - most innovation occurs at the physical and application layers
- Routing
  - Process of deciding which direction to send traffic  
  - Spanning tree algorithm/backwards learning
- Forwarding
  - You know what your routing algorithm is, forwarding is taking action based on your routing table
## Network Service Models
- Two main ones
- Both are implemented using _store-and-forward packet switching_
  - Use statistical multiplexing to share the routers resources between different users
  - Buffers are FIFO (queue)
  - When buffers fill up, packets are discarded (dropped)
- Datagrams (connectionless service)
  - Like postal letters
  - This is IP
  - No reason/guarantee that two different messages will go on the same path
  - IPv4 has 32 bit addresses
  - data flow is not stored, but router tables still exist
- Virtual circuits (connection-oriented service)
  - Think telephone call
  - Corporations/rich people use these
  1. Connection is established - path is chosen, circuit information is stored in routers
  2. Data transfer, circuit is used - packets are forwarded along the path
  3. Connection teardown, circuit is deleted - information removed from routers
  - Huge router tables are needed (for each flow) so you need to pay some money for it
  - Packets only need to specify a circuit
  - MPLS (Multi-Protocol Label Switching)
    - Used by ISPs who want to allocate faster speeds to certain clients
    - Circuits are setup in the ISP backbone ahead of time
    - MPLS labels are added to IP packets at ingress, and removed at egress
- Datagrams vs Virtual Circuits
  | Issue              | Datagrams                     | Virtual Circuits           |
  |--------------------|------------------------------|----------------------------|
  | Setup Phase        | Not needed                   | Required                   |
  | Router state       | Per destination              | Per connection             |
  | Addresses          | Packet carries full address  | packet carries short label |
  | Routing            | Per packet                   | Per circuit                |
  | Failures           | Easier to mask               | Difficult to mask          |
  | Quality of service | Difficult to add             | Easier to add              |
- Internetworking enables multiple networks, which can be arbitrarily different, to connect and operate together
  - Vint Cerf and Bob Kahn pioneered this and are considered the "fathers of the internet"
- Two different routers could be speaking totally different languages (Ethernet, 802.11/WiFi, MPLS, etc) and you need them to be able to communicate
- IP is the "narrow waist" of the internet, because it's the only thing you _need_ to support tp be connected to the internet
  - Allows lower and higher layers to expand in complexity as long as everyone agrees on midpoint
  - Since it is the common link, it needs to be computationally and resource lightweight
  - Each packet should have a source/destination address (32 bit for IPv4) as well as a few other tags in the header to be "IP compatible"
## IP (Internet Protocol)
- IPv4
  - 32-bit, written in dot-quad notation (e.g. four integers of 8 bits $\rightarrow$ A.B.C.D)
- How do you assign IP addresses?
  - Allocated in blocks called prefixes (top L bits)
  - $x_1x_2x_3x_4x_5x_6x_7x_8x_9x_{10}x_{11}x_{12}x_{13}x_{14}x_{15}x_{16}\mid y_1...y_{16}$
  - In the example above, the prefix is all of the $x$s, and you have $2^{32-num(x)} = 2^{16}$ possible addresses in that particular prefix block
- Prefixes are very important for scalability, and are a major help for building router tables
  - They are written in the following format: [IP adress]/[Length of prefix] (e.g. 128.13.0.0/16)
  - a "/24" matches $2^{32 - 24} = 2^8 = 256$ addresses, and a "/32" matches only one
  - In olden times, Class A had an 8-bit prefix and $2^{24}$ addresses, B had 16/$2^{16}$ and C had 24/$2^8$
- Universities which were part of the ARPANET grandfathered themselves in to give themselves a bunch of IP addresses and now some countries have fewer addresses than universities (enter IPv6)
- The useful bit about IP addresses is that the prefixes allow you to group machines together so your router only needs to store a port for the prefix instead of all $2^{32-L}$ machines
- The _Longest Matching Prefix_ algorithm allows for more specific/overlapping addresses in the router table - say there are $n$ entries in your router table which match your prefix. You will send your packet to the port with the most bits that match in a row.
  - Using the table below, a packet with the destination address $192.24.6.0$ will go to port D, but $192.24.14.32$ will go to port B (this is because the third byte of the address is $00001110$, which has a longer matching prefix with $000011XX$ than $000XXXXX$) - note that I have X'd out the bits which are not fixed in the router table
  - However, $192.24.54.0$ does _not_ match the prefix for port B, so it goes to D
    | Prefix        | Port |
    |---------------|------|
    | 192.24.0.0/19 | D    |
    | 12.24.12.0/22 | B    |
- People used to manually assign their IP addresses to their computers, but now DHCP does it for us
## DHCP (Dynamic Host Configuration Protocol)
- From 1993, very popular
- Leases IP addresses to nodes
  - Lots of other information like where your router is
- Client-server application which uses UDP ports 67 and 68
- Send out message to address $11111111.11...11$ (aka 255.255.255.255) which is recognized as a discovery message
  - Server then responds by offering to "lease" an IP address
  - Host responds to desired offered IP for more information
  - If an IP address hasn't been used in a very long time, it is recycled
## ARP (Address Resolution Protocol)
- Sits on top of link layer
- How do you get from an IP address to an ethernet address?
  - Every computer on the internet may have one or more IP addresses, but all come with a unique 48 bit ethernet address on their NIC (Network Interface Card) which is guaranteed to be unique by the IEEE
  - Since IP addresses are leased by DHCP, it doesn't work to have a static mapping of IP to Ethernet
  - To figure out which ethernet address to use, the host computer will send out a broadcast message, which a router will forward to everyone, asking who (which ethernet address) currently owns the specified IP address
  - Then, whichever computer owns that IP address will respond with its ethernet address
  - This is of course optimized by having the original node include its own ethernet address in the outgoing packet which prevents the receiving node from needing to perform another instance of ARP to figure out which ethernet address to respond to
- No servers, just ask nodes with target IP to identify self
## Internetwork Packet Conflicts
- Networks may not agree on the maximum size of a packet - if such a conflict occurs, you will need to divide up the packet and send it in bits
  - Ethernet has 1.5k Bytes, WiFi has 2.3k (for max packet size)
  - Larger packets are generally preferred for efficiency
- IPv4 Packet Fragmentation
  - Routers break up the packet at the ingress of the network, and the receiving host bears the responsibility of reassembling it to reduce the overall load on network resources
  - Part of the IPv4 headers include information about the fragmentation offset - all but last packet will have a MF (More Fragments) on all pieces except last
  - Issue here is that the routers have to do a lot more copying of headers, and the probability of loss is high which would require the entire (unfragmented) packet to be retransmitted
- Path MTU (Maximum Transmission Unit)
  - Test packets with different sizes (before sending message) to find out what the largest possible packet size is
  - Packet sizes are pretty standard which helps simplify this solution
  - Loss and changing router tables can cause some hiccups here
## ICMP (Internet Control Message Protocol)
- Mechanism to figure out when something goes wrong
- Companion protocol to IP
- Sits on top of IP protocol
- Allows for error reporting and testing
- When something goes wrong, an ICMP error report is sent back to the IP source address - the problematic packet is also discarded and left to the host to rectify
- ICMP packets contain some codes/types to let the host know what went wrong in broad strokes
- Traceroute uses the TTL (Time To Live) field of ICMP packet headers to step one router out at a time, and examine the name of routers which report the error
## IPv6
- Some countries have run out of IPv4 addresses (not the US) and now we are introducing 128 bit addresses
- Instead of 4 groups of 8 bit numbers, you have 8 groups of 16 bit numbers (for the legible translation)
- Hard to deploy because of need to be compatible with IPv4, one way people get around is by tunneling
  - At ingress of IPv4 network, encapsulate IPv6 packet inside IPv4, and then remove IPv4 at egress
  - In this sense, the tunnel is a single link across the entire IPv4 network
## NAT (Network Address Translation), a "middlebox"
- Used widely at the edge of networks (such as homes)
- Modems do this work
- Rapid deployment/multiple control
- Breaks encapsulation because routers are looking at application layers
- Comcast wants to use NAT to control traffic and ruin net neutrality
- Connect internal to external network
- Motivated by IP address scarcity - multiple internal hosts are connected using few external addresses
- Adds a bit of security by making all computers on your home address look like a single entity
- Still maintain different port numbers on external IP addresses
  - _Why maintain different port numbers instead of using ethernet addresses to differentiate_
- Downsides
  - Connectivity is broken (can only do incoming packets after the outgoing connection is set up)
  - Doesn't work great when there are no connections (UDP apps)
  - Apps which expose their IP addresses don't work great with this
- Upsides
  - Relieves IP address pressure
  - Easy to deploy
  - More control/power (such as firewalls)
## Improving the Spanning Tree (Routing Algorithms)
- Recall that our spanning trees generated by Perlman's algorithm did not necessarily use the shortest path between nodes - just that it removed cycles
- Routers may also fail which requires the link layer to be adaptable
- Delivery methods
  - Unicast (single source/single destination)
  - Broadcast (single source/all destinations)
  - Multicast (single source/subset of destinations)
  - Anycast (multiple source/multiple destinations)
- Our algorithms must
  - Find paths which work
  - Use resources efficiently
  - Be fair (not starve nodes)
  - Adapt quickly
  - Scale well
- Trying to do a spanning tree at the network layer is probably not a great move
- At this level, the cost of transmission is also a concern
- We can approximate the "best" path by looking for the shortest using the following parameters
  1. Assign each link a cost (distance)
  2. Defined the best path between pairs of nodes as the path with the lowest cost/shortest distance
  3. Pick randomly to break ties
- Costs should be bidirectional (equal in each direction) though this can be changed if needed
- Every subset of your shortest path is also a shortest path
  - Say the shortest path $A \rightarrow E$ is $A \rightarrow F \rightarrow H \rightarrow Z \rightarrow E$ then the shortest paths $A \rightarrow H$ and $A \rightarrow Z$ must not cost _any more_ than the costs of the paths $A\rightarrow F \rightarrow H$ and $A \rightarrow F \rightarrow H \rightarrow Z$
- The sink tree for a node $N$ in a graph $G$ is the union of all shortest paths $n \in \{G - N\} \rightarrow N$
  - If you construct this tree, then each router $R$ only needs to store a forwarding table which contains the "next hop" to reach a node $N$ along the best path, instead of storing the entire path
  - Can run Dijkstra's algorithm to construct the sink/source tree for all nodes, but the issue is actually obtaining a complete graph/topology in a decentralized system
## Distance Vector Routing
- One of two main approaches to routing algorithms
- In practice, more advanced things are used but this dominated the early stages of the internet
- All nodes compute forwarding tables in a distributed setting
  1. Nodes only know the cost of reaching their neighbors
  2. Nodes can talk to their neighbors using messages
  3. All nodes run the same algorithm
  4. Failure can happen
- Very similar algorithm to Dijkstra's, you store a vector of costs with 0 to self and initially $\infty$ to all others, then periodically ask neighbors $N_{1, ..., n}$ what the cost is to reach another node $V$, and then take the minimum of $cost(N_i) + cost_{N_i}(V)$
  - Once you have that minimum cost, use node $N_i$ to reach $V$ until you hear a better cost or $N_i$ fails
- Converges, in the worst case, in $O(n)$ time if $G$ is basically a linked list
- Good news travels quickly, but bad news travels slowly (which makes sense, considering how we are measuring "best")
  - Specifically, if a node $N$ goes down, all other nodes in the network may not actually know that it is _gone_, and they will continually run the distance vector algorithm, "counting to $\infty$" trying to find the shortest path to $N$ (which DNE)
- Decent in resource-constrained environments, but most people use the link-state routing algorithm in practice
- RIP (Routing Information Protocol)
  - DV protocol with a hop count (TTL header?)
  - Infinity is 16 hops which limits network size
  - Runs on top of UDP, constantly sending vectors every 30 seconds with a 3 minute timeout to detect failures
- Does not scale well (don't want linear time)
- Today, people use flooding (broadcast messaging) which is not efficient in terms of resources but faster
  - Send incoming message to all neighbors
  - Remember the packet so it is only flood once
  - Nodes may receive the same message multiple times
- Link state routing is more computationally/resource intensive, but much faster
  - Same rules as for the distance-vector algorithm
  - Two main phases
    1. Nodes flood the network in the form of link state packets (who am I connected to, what are the costs of the edges)
    2. Each node computes its own forwarding table (by running Dijkstra or equivalent)
  - Essentially, each node learns the entire topology instead of just the one-hop
  - If a link fails, both nodes connected to it will notice and update (reflood)
  - If node fails, all of its neighbors will figure it out and update
  - Pretty simple to just add another node
- The main differences between DV/LS is that DV is slow but scalable, and LS is fast but takes a good bit of time to store/compute on each node
## Routers and IP
- Hosts are computers, routers control the flow of packets
- Your local network is switched, and connected to a larger network via a router/IP
- You can group hosts under IP prefixes to keep the size of routing tables small
- Importantly, you can reduce the size of your routing table by doing geographical routing in addition to IP prefixing
  - How do I get across the Atlantic ocean (instead of specific IP prefixes which are all in Europe while you are in America)
- Internet is still experiencing major growth
  - If router tables get too large, lookup time goes up and speed goes down
  - Link-state routing broadcasting becomes pretty heinous at large scale
- Hierarchical routing controls the flow of packets based first on a geographic region, and then the IP prefix inside the region
  - Still storing one-hop routing tables, but instead of information about external-region nodes, you  just store the first hop which brings you closer to all the nodes in that region
  - You may not always get the shortest path when you substitute a full table with a hierarchical table as a natural consequence of forfeiting information
- You can help scale regions by using subnets and aggregation
  - An IP/16 manager can split its traffic into three IP/18 networks
    - Then all traffic which matches the IP/16 needs only know how to get to that top level
  - Routers can change prefix lengths as you go outside your region without impacting hosts
- How do you route between different ISPs?
  - Each may have their own routing policies
  - Internet-wide BGP routing
  - Not always the shortest path - you want to make your choices based on how much things are going to cost

## Interdomain Internet Routing
- The following are notes from a lecture by [Hari Balakrishnan & Nick Feamster](https://courses.cs.washington.edu/courses/cse551/09sp/papers/Balakrishnan_Internet_Interdomain_Routing_2005.pdf)
- Autonomous systems
  - In an ideal world, end-hosts would be connected to each other via a central/fault tolerant network which played together nicely
  - In reality, various commercial entities provide internet service, and have to consider profitability when making routing (and other) decisions
  - Tier 3 ISPs are small and local (customers are usually close geographically)
  - Tier 2 ISPs are regional (statewide or small country)
  - There are less than 10 Tier 1 ISPs and they constitute the truly "global" internet
  - An Autonomous System (AS) is a collection of routers and routing tables which interact with other ASes to "exchange reachability information"
  - ASes have IGPs (*Interior Gateway Protocols*) which determine how to handle internal traffic
    - While EGPs are concerned with exchanging information and policy in a scalable matter, IGPs are more focused on "optimizing a path metric" and are not as concerned with scalability
    - Distance Vectors and Link-State protocol are examples of IGPs
- Inter-AS Relationships: Transit and Peering
  - Peering v. Transit
    - The primary traffic an ISP is concerned with comes from their customers - they will share all information in their routing tables with their customers and then make decisions concerning speed depending on how much the customers pays
    - While the above is called transit, peering is when ISPs share information with other ISPs
    - This is usually done to avoid going through a skip-level ISP which means payment as well as additional latency
    - If two smaller ISPs set up a direct link to share traffic which passes through their neighboring communities, their customers benefit from the speedup and they benefit from not paying their backbone (tier n+1) ISP
    - Because of this peering is often a mutual relationship, but if it becomes too asymmetrical, contracts can be worked out
    - These agreements are behind closed door types of things and since they are between competitors, are often renegotiated
    - Advertising routes is defined as the following: *"A route advertisement from B  to A for a destination prefix is an agreement by B that it will forward packets sent via A destined for any destination in the prefix"*
    - One way to think about the business model of an ISP is two-pieces: an entry in their routing table and handling of packets to/from that leased IP address (the data rate of which can play a major role in deciding cost)
  - Exploring Routes: Route Filtering
    - ISPs don't want to be responsible for handling packets which they aren't making money off of so they need to be mindful of cost when advertising routes to neighbors
    - I don't understand why the customer is advertising here $\rightarrow$ *"if a destination were advertised from multiple neighbors, an ISP should prefer the advertisement made from a customer over all other choices (in particular, over peers and transit providers)."*
    - I think the above is discussing the relationship between backbone ISPs and their customers - since the customer (B) is paying the larger ISP (A), A should favor B since it is paying for the use of As network
    - ISPs do not want to provide transit to their providers since they don't make money on it so they are more likely to advertise an external route to customer rather than peer traffic
    - In fact, advertising a route through a provider ISP to a peer may cause a smaller ISP to send a peers traffic through the route they are paying to use
    - The applications of BGP we have discussed so far are primarily concerned with relationships *between ISPs* but less interesting customers (such as end users) do not need such complex route management and usually have static routes along which traffic flows
  - Importing Routes
    - An AS needs to be thoughtful in which routes it keeps in its routing tables if it hears multiple paths to a single destination
    - If an ISP A hears an advertisement from a peer (B) for one of As customers, A needs to keep in mind that B could route the traffic to even more ISPs creating latency, so a direct route to the customer is preferred (the rule of thumb is *customer > peer > provider*)
- Border Gateway Protocol
  - Design Goals
    1. Scalability
    2. Policy - each AS needs to be able to decide what to export/import
    3. Cooperation Under Competitive Circumstances - BGP was designed as a means to handle the transition from a central backbone (NSFNet) to a varying number of commercial entities
  - The Protocol
    - Pretty well defined
    - Runs over TCP on an established port (179)
    - Router R will establish a TCP connection with another router S. Then an `OPEN` message is sent and completed, and both routers share their filtered tables which can take several minutes
    - The connection then stays open for an amount of time. Because TCP is a reliable connection, updates are only needed when a change happens - such as an annoucement (a new route or tweak to existing) or a withdrawal, to inform that a route is no longer operational
    - Since TCP does not define a transport-layer "keepalive", BGP does - essentially there is a known timer counting down in which each router guarantees to send a single message (keepalive in place of any substantial messages)
    - Recall that BGP does not optimize any metric but sends simple messages in the format *IP Prefix: Attributes*
    - There are several standardized attributes
  - Disseminating Routes within an AS: eBGP and iBGP
    - eBGP sessions are across ASes between BGP speaking routers while iBGP is between BGP speaking routers inside a single AS
    - Routers _within_ an AS need to be careful that
      1) There are no loops in the routes which packets are sent across
      2) All gateway routers need to be in agreement of everything  (this is implemented by maintaining several iBGP sessions across the network)
    - The full-mesh configuration of a network might be alright for a smaller ISP, but is not scalable as it requires a quadratic number of iBGP sessions
    - Two popular alternatives are _route reflectors_ and _confederations_ of BGP routers
    - Route Reflectors
      - Basically the selective exporting we discussed above
      - Has a  notion of money - knows who is a _client_ BGP router
      - Selects the optimal route for destination prefix and announces based on the following
        1. Routes learned from a client are announced to everyone
        2. Routes learned via iBGP from a non client router are only announced to its client routers
      - In this way, route reflectors basically act as if they are a singular network - and do not want to do any work for other routers
      - In practice, there are hierarchies of route reflectors which help ensure the best use of all known routes
    - How routes are updated depends on whether the update traveled through an eBGP or iBGP session
    - eBGP is typically point-to-point over the same LAN
    - iBGP may be several hops away, and so the IGP decides how to connect the routers and figure out what the next hop IP address is
    - Setting up iBGP topology is tricky, and two common types of incorrect behavior include loops and oscillations
    - Confederations
  - BGP Policy Expression: Filters and Rankings
  - Exchanging Reachability: `NEXT HOP` Attribute
    - There are several attributes which come along with an announced prefix in BGP
      | Route Atribute                    | Description                                                                                                                                         |
      |-----------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------|
      | NEXT HOP                          | The next router to go to along the route. When exiting an AS this is set to the gateway router, but the field is not modified in iBGP sessions.     |
      | AS Path                           | List of the ASes (identified by unique 16 bit integers) which span the route advertisement                                                          |
      | Local Preference                  | At the gateway of a network, this field will be assigned by the eBGP router then forwarded internally, telling routers how to compare/select routes |
      | Multiple-Exit Discriminator (MED) | Used to break ties between multiple routes through a single AS (assigned externally as a polite "this is what we prefer")                           |
    - There are many factors deciding which route to take, and `ASPATH` may factor in, or may not (how concerned about latency are we?)
- Failover and Scalability
  - Because there can be multiple eBGP sessions across ASes, BGP is _technically_ fault tolerant, though pretty slow to react
  - When an issue is detected, the withdrawal announcement propagation is damped throughout the AS to prevent a constant back/forth of up and down
  - The above does mean that there is a delay in state across the AS while the nodes which _do_ know about the failure wait for it to recover before informing their sub-nodes
  - Multi-homing: Promise and Problems
    - Multi Homing is when a customer uses multiple providers for their traffic
    - Not super scalable with our modern systems
    - For this to work, you would need both providers to advertise (roughly the same amount) your IP address with the same prefix width or else you would only see traffic coming through the one with the longest matching prefix
    - Larger IP prefixes are preferred in this situation as well
  - Convergence Problems
    - The slow fault detection in BGP is a major problem (can be "super-exponential") and leads essentially to an absence of wide area routes for mission critical applications
- The following are notes from Shyam Gollakota's lecture on the topic
  - Different ISPs may implement different policies which impacts how traffic is routed (quickest out of my network, minimum use of my resources, etc)
  - Peering is only really set up with specific endpoints in mind - Network B will forward traffic from A only to nodes within Bs network since it is not making money on anything else
  - Say you want to go from $AS_i \rightarrow AS_j$ and can go through paths $P_c, P_p, and P_\$$ representing client paths, peer paths, and provider paths, you are most likely to pick the client path since they are paying for your traffic
  - ASes will advertise client routes to everyone since they are being paid for that, but keep the peer/provider routes for their clients since they don't make money off of that
  - Peers would not advertise routes through their providers since they wouldn't make money
# Transport Layer
- Builds on Network Layer to deliver data across networks - offers guarantees of reliability/quality
- The transport layer equivalent of frames from the network layer is segments
  - Segments are contained within packets within frames
- UDP sends unreliable datagrams (glorified packets) and TCP sends reliable bytestreams
- TCP
  - Connections
  - Single, orderly, reliable delivery of bytes
  - Arbitrary length
  - Flow/Congestion controls matches sender to receiver/network
## UDP
- Datagrams
- Potential loss/reorder/duplication
- Limited size
- Can send regardless of network and receiver state
- Applications use ports, typically anything below $1024$ is a well known port which requires administrative privileges (the ports have an expected use) while things above 1024 are typically temporary client ports
- Voice-over-IP, DNS, RPC, and DHCP are typically done over UDP
  - Specifically for the voice, you care a lot more about latency than accuracy of order (a few lost packets is fine, but a 5 second delay is worthless)
  - Can use ML or some extrapolation/interpolation scheme to deal with losses at the Application layer
- Processes need to maintain buffers for UDP packets because the OS may not schedule them 100% of the time

## TCP
- Not actually 100% reliable, but pretty dang good
- Connection Establishment
  - Sender and Receiver need to be ready to receive, canonical way to think about this is the *Three Way Handshake*
  - Both sides probe the other with a fresh Initial Sequence Number (ISN)
  - Synchronize segment sent to server, responds with ACK
  - Example timeline
    - Client sends $SYN(x)$
    - Server responds with $SYN(y)ACK(x+1)$
    - Client responds with $ACK(y + 1)$, acknowledging everything up to $y + 1$
  - Random initial $x$ and $y$ are chosen (instead of starting at zero) to deal with potential losses of internet at either end
- After everything has been sent, there is an orderly way of releasing a connection
  - There is a symmetric close where both sides are shut down independently
  - Active sends $FIN(x)$, passive ACKs
  - Passive sends $FIN(y)$, active ACKs
  - Each $FIN/ACK$ closes a direction of transfer
  - Buffers and resources for either side of these values are released upon finish
  - There are timeout values since loss of connectivity is still an issue

## Sliding Window
- Where is the *meat* of TCP?
- Remember ARQ is a stop/wait protocol which sends one frame at a time before waiting for an acknowledgment
  - As we discussed, allowing only a single message to be outstanding is pretty weak sauce for anything but LAN (where only one frame fits)
- Sliding window allows for a window of outstanding packets where up to $w$ packets are transmitted in a single $RTT$ (Round Trip Time)
  - $w = bandwidth*RTT = 2 bandwidth*delay$
- There are many variations which take different parameters into account
- You have to buffer $w$ segments until they are all acknowledged
- LFS (last frame sent) - LAR (last ack rec'd) $\leq w$
- Go-Back-N
  - Receiver has a one-packet buffer, and a LAS var (last ack sent)
  - If the receiver's next ACKed seq number is not $LAS + 1$, then you know something went wrong and where to go back
  - Kind of wasteful because anything following a loss is thrown out
- Selective Repeat
  - Receiver passes data *in order* to the application, regardless of what order it was received
  - Out of order frames are buffered to avoid retransmission
  - Each unpacked segment has a timer, and retransmits a segment whenever that timer counts down (until the segment has been ACKed)
  - Need $2w$ seq. numbers to differentiate between outstanding acks and current packets
## Flow Control
- Different computers have different capabilities/resources and so they need to be capable of communicating how much data should be "flowing" across the -network
  - Even a powerful computer might not have enough resources to support 100s of processes
- If the app doesn't call $recv()$ enough, the buffer allocated by the OS might not be able to hold everything, even though ACKs have already been sent back to the transmitter
- The receiver can send an available window size alongside an ACK, which the transmitter can use to determine a maximum amount of data which can be sent to the other end
## Timeouts
- How do you figure out when something has been lost and you need to retransmit?
- Set a timer when your segment is being sent
  - If the value is too long, you'll waste network capacity
  - Too short, and you'll waste resources
- For LAN, it is pretty easy to predict worst-case RTT
- Tricky bit is at scale (*the* internet)
- There is a ton of variation and unpredictability along the way
- You don't want to hardcode a timeout bc it will always be either too conservative or too low in some case - instead we *adapt* the timeout  value depending on what you see
- Adaptive Timeout
  - TCP keeps an eye on $\mu$ and $\sigma$ to decide how long its timeout value should be
  - Keep track of the smoothed (estimate) of RTT with $SRTT_{N+1} = 0.9*SRTT_N + 0.1*RTT_{N+1}$
  - Also note the variance as $Svar_{N+1} = 0.9*Svar_N + 0.1*|RTT_{N+1} - SRTT_{N+1}|$
  - Keeping track of these fields, with some knowledge of probability, lets the timeout be constantly updated to $SRTT_N + 4* Svar_N$ which will likely cover the upper RTT
## TCP
- Reliable Bytestream
  - Message boundaries not always preserved (smaller packets may end up being buffered) but the actual messages and the orders of them are preserved
  - Control information, such as ACKS, can piggyback on data segments in reverse direction to create a packet with more information
  - There are a lot of header values which go into TCP headers, which are stacked on top of other headers in a packet, which means sending low amounts of data very long distances is often not useful
- Sliding Window - Receiver
  - Can specify the next in-order packet received, as well as (up to) three ranges of already received packets which were out of order
- Sliding Window - Sender
  - Try to reason about loss without using timeouts (which aren't great to wait for)
  - Attempts to resend a packet before a timeout ever happens
  - EG, if the frames 101-199 are sitting on a timeout, but the sender has received multiple (three in the example) ACKs for 100 and ranges above 200, then it can reasonably infer that 101-199 was lost
- In the 1980s, early TCP had a fixed size sliding window
- Congestion collapse (described below) was a serious issue as  the ARPANET grew - links would be busy but transfer rates were abysmal
- Van Jacobson is credited with saving the internet by introducing practical congestion control
  - Added AIMD principles
  - TCP Tahoe ('88)/Reno ('90) were the two main names of what he created
- He proposed fixing timeout values and introducing congestion windows to limit queues/losses
- the cwnd (congestion window) is adapted using packet loss as the main parameter
## Congestion in Networks
- People and computers can generate varying amounts of traffic
- Routers have input/output queues which help absorb bursts, but these can fill up when there are a lot of requests coming through the network
- When a lot of routers are seeing the effects of congestion, they will increase their timeouts - which will have the impact of a "congestion collapse" where there is almost no traffic (which will cause senders to see increased performance and bring their timeouts back down)
- You want to push a network just to the point of congestion but not crossing that threshold to get maximum performance
## Efficiency vs Fairness (Bandwidth Allocation)
- Can't have them both
- Sometimes a fair network is not efficient and an efficient network is not fair
- Different links have different bottlenecks which will impact the performance of the network
- The policy people end up using is Max-Min fairness
  - You want to maximize the minimum amount of flow across links
  - Flows which are bottlenecked on one link get an equal share of the link
  - Algorithm is as follows  
    1. Start with all flows set at 0
    2. Increase flows (basically set a single link to the max capacity of the flow) until there is a new bottleneck in the network
    3. Fix the rate of all flows which are bottlenecked
    4. Go back to step 2 if there are any unfixed flows
  - This doesn't really work in practice because you don't have a singular graph for the whole network
- You can also adapt flows over time - as specific flows activate/deactivate, changes in flow allocation can share the resources dynamically
## Additive Increase Multiplicative Decrease (AIMD)
- Solution to decentralized flow allocation
- Network layer provides feedback, transport layer makes adjustments
- Bandwidth usage is rarely on 100% of the time - internet browsing is more punctuated and this factors into allocation
- When the network is not congested, nodes iteratively increase their rate
  - but when the network is bogged down, nodes dramatically decrease their rates
- Imagining a router with two nodes, the inequality $x + y \leq 1$ describes the area where the network is capable of functioning
- the line $y = x$ is maximally fair
- Moving the allocation $(x, y)$ to $(x + c, y+c)$ is parallel to the maximally fair allocation
- Moving along $(x, y)$ to $(\frac{x}{c}, \frac{y}{c})$ goes toward the origin
- Alternating between these will zigzag the allocations to the maximally fair line
  - There's a useful diagram in the transport layer slides for visualizing this
- Guaranteed to converge to a efficient/fair allocation
- Only needs binary feedback from the network - are we congested or not?
- There are actually more complex signals which can inform routers/senders about the details of what's going on
## ACK Clocking
- The rate at which ACKs come back give you a sense of the most congested link in your network  
- You can think of ACKs as clocking data segments because as they come in, the window is advanced by one and a new segment is able to be introduced
  - If you release the first window as a burst of segments, the rate at which the acks come back will be (at most) as fast as the bottleneck in the flow
  - Then as the window is advanced, new segments are released into the network only as fast as the link can handle, which means there will not be excess packets floating about which we can reasonably infer were released faster than the bottleneck could handle
  - In other words, ACKs will come back at the max level the network can handle, and releasing segments based on this "clock" prevents the network from being overburdened
## Implementing AIMD
- There is a "slow start" which is the additive increase portion
- We want to jump right to the ideal rate ($cwnd_{ideal}$) but there is a lot of variance so we need to pick a strategy carefully
- Instead of "additively increasing" the condition window size, we can grow it exponentially by doubling it every $RTT$
  - Fixed window size can get it right if you just guess correctly every time, but that's easy unless you're an oracle
  - The other issue to keep in mind with immediately large (even correct!) window sizes is that you will introduce a _burst_ of packets into the network instead of iteratively one at a time - this is not great for the poor queues who now have to handle all your business
- This exponential growth is basically guaranteed to go over and introduce loss
  - This is when you suspect your window is too large - swap to additive increase! After dropping your window size back down to the last size _without_ loss
  - This enables more fine tune (smaller step) adjustments since you now know you're close to the ideal
- TCP Tahoe Implementation
  - Slow start phase only increments $cwnd$ by one per ACK
  - Additive increase phases increments $cwnd$ by $\frac{1}{cwnd}$ packets per ACK which adds roughly 1 packet per $RTT$
  - The slow start threshold (before swapping to AI) is initially infinity, but set to $\frac{cwnd}{2}$ after a loss
- If you hit a timeout value, you will reset and set $cwnd$ to 1 and run the slow start phase until hitting your slow start threshold previously discovered
- The reason for going all the way back to 1 (instead of the threshold) is that you've lost your ACK clock and need to reset it
- This is not ideal - remember that we have some hints about data which arrived out of order
- Fast retransmit
  - Treat three duplicate ACKs as a loss, retransmit the lost range (hopefully before the timeout occurs!)
  - In the example, $[1, 13]$ and $[15, 20]$ was received but $14$ was lost, so you observe the following behavior:
  - $ACK_{10}, ACK_{11}, ACK_{12}, ACK_{13}, ACK_{13}, ACK_{13}, ACK_{13}$
  - At this point, you've seen three (duplicate) $ACK_{13}$ so you retransmit packet 13, and upon reception, you will receive $ACK_{20}$ because those packets had already been seen!
  - This enables you to fast-forward six packets to packet 21, which makes everyone happy :smile:
- TCP Tahoe is the one which goes all the way back to 0 whenever a loss occurs, which is not ideal
- TCP Reno uses fast retransmit to detect losses early without having to go all the way back to square one
- We have not (yet) discussed the multiplicative decrease part of the backoff which prevents further congestion
- Each subsequent ACK lets us know a new segment has arrived
- SACK sends ACK ranges so that senders can accurately retransmit without any guessing
## Explicit Congestion Notification
- Routers detect onset of congestion by looking at queue sizes
- Some packets are then marked as "congested" and the receiver will send a *special* kind of loss which acts a flag to start multiplicative decrease without actually having a loss
- Downside is a lot of routers need to have new fancy hardware/software to
- Will be adopted in years to come, already works at scale in data centers 
# Application Layer

# Quality of Service
