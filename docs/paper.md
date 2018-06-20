#cjdns Research paper

##What is cjdns?

cjdns is a mesh networking protocol built on-top of existing protocols. Its primary aims are to:

* Provide end-to-end encryption
* Limit the size of routing tables
* Make denial of service atacks much more difficult
* Simplify the configuration networks

By utilizing already existing network structures, cjdns has a better chance at gaining widespread adoption than several of its competitors. It is also worth noting that cjdns has been used to successfully build large-scale, currently operating networks such as Hyperboria.

At a glance, the stack for a typical implementation of cjdns looks like this:

| cjdns |
| IPv6  |
| Data Link Layer |
| Physical Layer |

##What prompted the creation of cjdns?

As the internet has evolved from its humble beginnings as a research project, an emphasis was placed on backwards compatibility as a means of getting and keeping adopters. This often means that older, insecure protocols continue to be used. Protocols have been built on top of IP, fully expecting that their information will be transmitted in plaintext unless they take the intitiative and add an encryption layer themselves. [Secure Shell](https://tools.ietf.org/html/rfc4253), [Transport Layer Security](https://tools.ietf.org/html/rfc5246), and [Simple Mail Transport Protocol Secure](https://www.ietf.org/rfc/rfc3207.txt) are all examples of this. To further complicate things, if one of those protocols is found to be insecure it requires a change to all of the individual protocols. The solution is not particularly modular. Likewise even protocls that attempt to be modular, such as TLS from the examples, are not adopted by all other protocols. If you push out a fix for TLS, SMTPS and HTTPS may now be more secure, but SSH may still be exploitable.

The solution is to move encryption further down the stack and build your additional protocols on top of it. This is what cjdns aims to do with its built-in encryption just above IPv6.

##What is IPv6?

##How does cjdns utilize cryptography?

##Routing in cjdns

##What are some current challenges cjdns faces? 
