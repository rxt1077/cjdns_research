# cjdns Research paper

## What is cjdns?

cjdns is a mesh networking protocol built on-top of existing protocols. Its primary aims are to:

* Provide end-to-end encryption
* Limit the size of routing tables
* Make denial of service atacks much more difficult
* Simplify the configuration of networks

By utilizing already existing network structures, cjdns has a better chance of gaining widespread adoption than several of its competitors. It is also worth noting that cjdns has been used to successfully build large-scale, currently operating networks such as [Hyperboria](https://hyperboria.net/).

The stack for a typical implementation of cjdns looks like this:

 Layer | Notes 
--- | ---
| cjdns | runs as a daemon on Windows / Mac / Linux  
| IPv6  | supported by most hardware
| Data Link Layer | usually ethernet frames 
| Physical Layer | radio / wire / fiber 

## What prompted the creation of cjdns?

As the internet evolved from its humble beginnings as a research project, an emphasis was placed on backwards compatibility as a means of getting and keeping adopters. This often meant that older, insecure protocols continued to be used. Protocols are now built on top of IP, fully expecting that their information will be transmitted in plaintext unless they take the intitiative and add an encryption layer themselves. [Secure Shell](https://tools.ietf.org/html/rfc4253), [Transport Layer Security](https://tools.ietf.org/html/rfc5246), and [Simple Mail Transport Protocol Secure](https://www.ietf.org/rfc/rfc3207.txt) are all examples of this. To further complicate things, if one of those protocols is found to be insecure it requires a change to all of the individual protocols. The solution is not particularly modular. Likewise even protocols that attempt to be modular, such as TLS from the examples, are not adopted by all other protocols. If you push out a fix for TLS, SMTPS and HTTPS may now be more secure, but SSH may still be exploitable.

The solution proposed by cjdns is to move encryption further down the stack. If the encyrption is found to be insecure and needs to change, a change to cjdns will secure all of the protocols above it. With cjdns sitting just above IPv6 an encryption layer is provided for all traffic above it.

Another challenge the classical internet faces is its ability to scale as more and more devices are connected. Routers are facing ever growing routing tables and clients have run out of IPv4 address. While updates to [BGP](https://tools.ietf.org/html/rfc4271) and the almost universal usage of [NAT](https://tools.ietf.org/html/rfc1631) by clients helps, they don't address the underlying issue. Likewise in a future where IoT marketers have promised us shoes with IP addresses, the standard way of doing things may not be up to the task.

cjdns addresses these problems by utilizing IPv6 and sharing the burden of maintaining a routing table through a distributed hash table. IPv6 provides significantly more addresses (one for every grain of sand on earth) and is largely supported by currently deployed hardware. DHTs have proven to be a viable for addressing groups of peers and maintaining large amounts of constantly changing information.

## How does cjdns work?

cjdns breaks down in to three main components: *switch*, *router*, and *CryptoAuth*

### switch

The switch maintains series of *interfaces* which are point-to-point links to other switches. Packets contain a *route label*, which is an ordered set of *directors* which tell the switch where to send packets. cjdns includes other fields in the packet including *encoding scheme*, *encoding form*, and *director prefix*, largely to support future changes to how the data in the packet is represented.

#### Example

Assume there are three switches: Switch 1, Switch 2, and Switch 3. The interfaces that each switch has are shown in the tables below:

Switch 1 Interfaces | Director
--- | ---
Self | 0001
Switch 1 -> Switch 2 | 1011

Switch 2 Interfaces | Director
--- | ---
Self | 000001
Switch 2 -> Switch 1 | 110000
Switch 2 -> Switch 3 | 110100

Switch 3 Interfaces | Director
--- | ---
Self | 01
Switch 3 -> Switch 2 | 11

If Switch 1 wants to send a packet to Switch 3 via Switch 2 it would set up the following route label:

**0000000000000000 01 110100 1011**

The segments from left to right are: empty space, Switch 3's self director, Switch 2's "Switch 2 -> Switch 3" director, and Switch 1's "Switch 1 -> Switch 2" director. The spaces are only shown for the clarity of this example, the actual bit sequence used is purposefully opaque. Without knowing all of the directors, there is very little information you can derive about how the packet will be routed.

The above route label is attached to a packet which is then sent to Switch 1. Switch 1 matches the four rightmost bits to its "Switch 1 -> Switch 2" interface and shifts them off of the route label:

**000000000000 01 110100**

Switch 1 also adds a reversed director to get back, in this case to self, to the left-hand side of the route label. Note that shifting off the director that was matched provides this space:

**1000 000000000000 01 110100**

Finally, Switch 1 uses its "Switch 1 -> Switch 2" interface to send this packet, with its carefully crafted route label, to Switch 2.

Switch 2 matches the six rightmost bits as a director to use interface "Switch 2 -> Switch 3". It shifts those bits off and adds a reversed director to get back, in this case "Switch 2 -> Switch 1", to the left-hand side of the route label:

**000011 1000 000000000000 01**

Finally, Switch 2 uses its "Switch 2 -> Switch 3" interface to send this packet to Switch 3.

Switch 3 matches the two rightmost bits as a director to use its self interface, meaning the packet was intended for it.

While this example worked out well, it is important to note that if for any reason a director cannot be matched, or a packet can't be sent down an interface a switch can just reverse the entire route label and the packet will have a route to get back to its original sender from whatever switch it is at. This can be most helpful for error reporting.

## A practical example

## What are some current challenges cjdns faces? 
