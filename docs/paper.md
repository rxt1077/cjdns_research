# cjdns Research paper

## What is cjdns?

cjdns is a mesh networking protocol built on-top of existing protocols. Its primary aims are to:

* Provide end-to-end encryption
* Limit the size of routing tables
* Make denial of service atacks much more difficult
* Simplify the configuration networks

By utilizing already existing network structures, cjdns has a better chance of gaining widespread adoption than several of its competitors. It is also worth noting that cjdns has been used to successfully build large-scale, currently operating networks such as Hyperboria.

The stack for a typical implementation of cjdns looks like this:

 Layer | Notes 
--- | ---
| cjdns | runs as a daemon on Windows / Mac / Linux  
| IPv6  | supported by most hardware
| Data Link Layer | usually ethernet frames 
| Physical Layer | radio / wire / fiber 

## What prompted the creation of cjdns?

As the internet evolved from its humble beginnings as a research project, an emphasis was placed on backwards compatibility as a means of getting and keeping adopters. This often meant that older, insecure protocols continued to be used. Protocols are now built on top of IP, fully expecting that their information will be transmitted in plaintext unless they take the intitiative and add an encryption layer themselves. [Secure Shell](https://tools.ietf.org/html/rfc4253), [Transport Layer Security](https://tools.ietf.org/html/rfc5246), and [Simple Mail Transport Protocol Secure](https://www.ietf.org/rfc/rfc3207.txt) are all examples of this. To further complicate things, if one of those protocols is found to be insecure it requires a change to all of the individual protocols. The solution is not particularly modular. Likewise even protocls that attempt to be modular, such as TLS from the examples, are not adopted by all other protocols. If you push out a fix for TLS, SMTPS and HTTPS may now be more secure, but SSH may still be exploitable.

The solution proposed by cjdns is to move encryption further down the stack. This way if it needs to change, you change cjdns and the protocols above it remain secure. With cjdns sitting just above IPv6, and encryption layer is provided for all traffic.

Another challenge the classical internet faces is its ability to scale as more and more devices are connected. Routers are facing ever growing routing tables and clients have run out of IPv4 address. While updates to [BGP](https://tools.ietf.org/html/rfc4271) and the almost universal usage of [NAT](https://tools.ietf.org/html/rfc1631) by clients helps, they don't address the underlying issue. Likewise in a future where IoT marketers have promised us shoes with IP addresses, the standard way of doing things may not be up to the task.

cjdns addresses these problems by utilizing IPv6 and sharing the burden of maintaining a routing table through a distributed hash table. IPv6 provides significantly more addresses (one for every grain of sand on earth) and is supported by most hardware. DHTs have proven to be a viable distributed data structure for maintaining large amounts of constantly changing information amongst peers.

## Why use IPv6?

## Routing in cjdns

## How does cjdns utilize cryptography?

##What are some current challenges cjdns faces? 
