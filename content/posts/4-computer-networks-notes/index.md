---
date: '2025-05-24T20:48:59-04:00'
draft: false
title: 'CS6250 - Computer Networks - Notes'
summary: "My notes on Computer Networks from GTech's OMSCS."
tags: ["school", "OMSCS", "Computer Networks", "notes"]
cover:
    image: osi_model_7_layers.png
    alt: "The OSI model."
    caption: "The theoretical OSI model."
    hidden: false   
ShowToc: true
---

## Lesson 1 - Introduction, History, and Internet Architecture

### History of the internet

The internet began as many things in tech did, from DARPA.

Specifically:

1. J.C.R. Licklider proposed the "Galactic Network" (1962)
    - Attached a computer in Stanford to a computer at MIT, thus beginning computers talking to each other
2. The ARPANET (1969)
     - UCSB, UCLA, Stanford, and U of Utah interlink
3. Network Control Protocol (NCP), an initial ARPANET host-to-host protocol (1970)
    - The first protocol was designed to handle increasing number of computers, first app built on this was email
4. Internetworking and TCP/IP (1973)
    - NCP became TCP/IP, IP for addressing and forwarding packets, TCP for flow control and recovery from lost packets
5. The Domain Name System (DNS) (1983) and the World Wide Web (WWW) (1990)
    - As the internet exploded with content and endpoints, they needed a way to turn domains into IP addresses. In walks DNS.

### Internet Architecture

The internet was built in layers. Each layeer is supposed to have its own job, and not rely on any of the layers above or below it.

Think of a person as a bit of data, then watch them fly from one airport to another and you get the idea.

![Internet as a flight path diagram](<Screen Shot 2020-01-16 at 11.09.35 AM.png>)

So there is an originally designed theoretical layer named the OSI model, which had 7 layers, and then the layers that were actually implemented, known as the Internet Protocol Stack.

![OSI Model vs actual Internet Model](<Network Layers.jpg>)

These layers are not as perfect as we would hope, specifically sometimes they do rely on the other layers or they both have methods of solving the same problem like error recovery.

Notice that the Session and Presentation layers disappear in the Internet Protocol Stack, those are handled in the *ports* of each network device. The logic that is accomplished in those layers still happens, just all in one place.

#### Application Layer

The data here is a **message**.

This is the layer we all interact with all the time.

- HTTP (web)
- SMTP (e-mail)
- FTP (transfers files between two end hosts)
- DNS (translates domain names to IP addresses)

Takes the content that we want to ship around, and does all the encoding and decoding needed to actually USE it.

#### The Presentation Layer

The presentation layer plays the intermediate role of formatting the information that it receives from the layer below and delivering it to the application layer. For example, some functionalities of this layer are formatting a video stream or translating integers from big endian to little endian format.

#### The Session Layer

The session layer is responsible for the mechanism that manages the different transport streams that belong to the same session between end-user application processes. For example, in the case of a teleconference application, it is responsible to tie together the audio stream and the video stream.

#### The Transport Layer

The data here is a **segment**.

End to End communication between hosts.

- Transmission Control Protocol (TCP)
- User Datagram Protocol (UDP)

TCP is better *connection*-oriented services, guarantees delivery, manages flow control, and controls congestion. This is the USPS, slow but reliable (except TCP doesn't lose mail like The USPS).

UDP is fast and guarantees *nothing*. Recievers and Senders using UDP must be able to handle segments getting dropped, delayed, or not making it entirely. But they should be faster when everything is working compared to TCP

#### The Network Layer

The data here is a **datagram**.

This is where IP comes into play. This layer is responsible for connecting one computer to another, via the IP address that everyone has.

#### The Data Link Layer

The data here is a **frame**.

This layer is responsible for moving the frames from one node (host or router/switch) to the next node. This uses things like:

1. Ethernet
2. Point-to-point Protocol (PPP)
3. Wi-Fi

#### The Physical Layer

The actual hardware translation. Translating electricity into bits based on ethernet, coax, fiber, etc.

### Encapsulation

These layers work on the idea of encapsulation and de-encapsulation. So encapsulation is  take a chunk of data, package it up, add a header to it and pass it on. Then de-encapsulation is using the headers to decode each chunk of data, untill all you're left with is the data/message.

![encapsulation vs de-encapsulation](0_tDSGidTRt7KrG6BR.webp)

This method is what allows people to build upon the existing infrastructure of the internet, but still make new things.

### The End to End (e2e) Principle

The e2e principle shaped the internet as we know it today.

Essentially the principle is that 99% of the complexity should be at the ends of the communications. This allows the underlying infrastructure to be simple, but expandable, and allows people working at the different ends to iterate and create new things quickly.

If they had to change the underlying architecture everytime they wanted to change anything it would bring development speed to a crawl.

Beneficial exceprt from the lecture:

Many people argue that the e2e principle allowed the internet to grow rapidly because evolving innovation took place at the network edge, in the form of numerous applications and a plethora of services, rather than in the middle of the network, which could be hard to modify later.  

>What were the designers’ original goals that led to the e2e principle?  
>>Moving functions and services closer to the applications that use them increases the flexibility and the autonomy of the application designer to offer these services to the needs of the specific application.

>Thus, the higher-level protocol layers are more specific to an application. Whereas the lower-level protocol layers are free to organize the lower-level network resources to achieve application design goals more efficiently and independently of the specific application.

#### Violations of e2e

All rules are meant to be broken.

Firewalls, NAT boxes, and traffic filters all break the e2e principle, and usually for good reason.

NAT routers provide a way to make up for the fact that there aren't that many IP addresses available in IPv4. Instead of EVERY device having it's own worldwide public IP address, you give your home 1 IP address, and then every device behind that has its own local address.

This means that whenever a message is sent to your PC it goes:

Web > Router (Public IP)> End Device (local IP)

The router is breaking some rules of e2e, becacuse it is intervening and inspecting data.

This is the notes reasoning for why:

> Why do NAT boxes violate the e2e principle?
>The hosts behind NAT boxes are not globally addressable or routable. As a result, it is not possible for other hosts on the public Internet to initiate connections to these devices. So, if we have a host behind a NAT and a host on the public Internet, they cannot communicate by default without the intervention of a NAT box.
>Some workarounds allow hosts to initiate connections to hosts that exist behind NATs. For example, Session Traversal Utilities for NAT, or STUN, is a tool that enables hosts to discover NATs and the public IP address and port number that the NAT has allocated for the applications for which the host wants to communicate. Also, UDP hole punching establishes bidirectional UDP connections between hosts behind NATs.

### The Hourglass Shape of the Internet

The internet is curvy.

No really, there's a ton on each end of it, but the middle is pretty narrow. Specifically, TCP, UDP, IP are really the backbone of literally everything. See below:

![hourglass internet](<L1-1 Evolutionary Architecture Model.jpg>)

All roads lead to IP, and TCP/UDP. Researchers have actually done a lot of work to see why this is. They called it the evolutionary architecture model. They did a bunch of in depth quanitifications of why/what the internet is and how they got there and drew some interesting conclusions.

In an ideal world where we could do it all over they came up with the following:

>Finally, in terms of future and entirely new Internet architectures, the EvoArch model predicts that even if these brand-new architectures do not have the shape of an hourglass initially, they will probably do so as they evolve, which will lead to new ossified protocols. The model suggests that one way to proactively avoid these ossification effects that we now experience with TCP/IP is for a network architect to design the functionality of each layer so that the waist is wider, consisting of several protocols that offer largely non-overlapping but general services, so that they do not compete with each other. 

### Interconnecting Hosts and Networks

Talking about how to theoretically communicate between computers is important, but hardware actually makes the physical connections.

Those are:

#### Repeaters and Hubs

They operate on the physical layer (L1) as they receive and forward digital signals to connect different Ethernet segments. They provide connectivity between hosts that are directly connected (in the same network). The advantage is that they are simple and inexpensive devices, and they can be arranged in a hierarchy. Unfortunately, hosts that are connected through these devices belong to the same collision domain, meaning that they compete for access to the same link.

#### Bridges and Layer-2 Switches

 These devices can enable communication between hosts that are not directly connected. They operate on the data link layer (L2) based on MAC addresses. They receive packets and forward them to the appropriate destination. A limitation is the finite bandwidth of the outputs. If the arrival rate of the traffic is higher than the capacity of the outputs, then packets are temporarily stored in buffers. But if the buffer space gets full, then this can lead to packet drops.

 #### Routers and Layer-3 Switches
 
These are devices that operate on the network layer (L3).

### Learning Bridges

A bridge is a piece of hardware that manages the connection between many devices, connected to the same piece of hardware.

![learning bridge](<L1-4 Illustration of a Learning Bridge.jpg>)

The bridge is able to understand what devices are on port 1, what devices are on port 2, and so on. This is like a home network switch. You can connect many devices to it, and it handles knowing where the data needs to end up.

#### The Looping Problem

The problem with these bridges, is that you can create a loop.

if A connects to B which connects to C which connects to A, you've created a loop. This can result in never ending looping!

This is handled by running a spanning tree algorithm. The goal of the spanning tree algorithm is to identify which ports when used will eliminate any endless looping.

This works by operating in rounds, and then by removing bridges from the network until you only have one path to every node. This is accomplished by running in rounds the following process:

1. Every node sends:
    - Sender Node ID
    - Root ID as percieved by sender
    - Distance from root node
2. Each node selects the best configuration in order of
    - If root of the one configuration has a smaller ID
    - If roots have equal IDs choose one with smaller distance to the root
    - If they have the same distance, choose configuration with smallest sender ID
3. A node stops sending configuration messages over a link (port) when it receives a configuration message from a neighbor that is:
    - either closer to the root
    - has the same distance from the root, but it has a smaller ID.

We can see this process completed in the following images:

![pre-tree-span](<Screen Shot 2020-01-16 at 11.36.30 AM.png>)

Then after tree spanning:

![post-tree-spanning](<Screen Shot 2020-01-16 at 11.38.09 AM.png>)

## Lesson 2

Coming soon....