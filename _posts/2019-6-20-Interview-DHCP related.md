---
layout: post
title:  "DHCP RARP"
categories: Interview
---
 DHCP is based on the Bootstrap Protocol (BOOTP), adding the
 capability of automatic allocation of reusable network addresses and
 additional configuration options .  DHCP captures the behavior of
 BOOTP relay agents, and DHCP participants can interoperate
 with BOOTP participants.
 
##  Bootp
#### usage
Bootp allowsa diskless client machine to discover its own IP address, the addressof a server host, and the name of a file to be loaded into memory and
executed. 
   
#### Bootstrap Protocol


   The UDP header contains source and destination port numbers.  The
   BOOTP protocol uses two reserved port numbers, 'BOOTP client' (68)
   and 'BOOTP server' (67).  The client sends requests using 'BOOTP
   server' as the destination port; this is usually a broadcast.  The
   server sends replies using 'BOOTP client' as the destination port;
   depending on the kernel or driver facilities in the server, this may
   or may not be a broadcast (this is explained further in the section
   titled 'Chicken/Egg issues' below).  The reason TWO reserved ports
   are used, is to avoid 'waking up' and scheduling the BOOTP server
   daemons, when a bootreply must be broadcast to a client.  Since the
   server and other hosts won't be listening on the 'BOOTP client' port,
   any such incoming broadcasts will be filtered out at the kernel
   level.  We could not simply allow the client to pick a 'random' port
   number for the UDP source port field; since the server reply may be
   broadcast, a randomly chosen port number could confuse other hosts
   that happened to be listening on that port.

   The UDP length field is set to the length of the UDP plus BOOTP
   portions of the packet.  The UDP checksum field can be set to zero by
   the client (or server) if desired, to avoid this extra overhead in a
   PROM implementation.  In the 'Packet Processing' section below the
   phrase '[UDP checksum.]' is used whenever the checksum might be
   verified/computed.
   
 <hr>

## RARP
Network hosts such as diskless workstations frequently do not know
their protocol addresses when booted; they often know only their
hardware interface addresses.  To communicate using higher-level
protocols like IP, they must discover their protocol address from
some external source.  Our problem is that there is no standard
mechanism for doing so

 <hr>
   
## TFTP
Trivial File Transfer Protocol. For TFTP Each nonterminal packet is acknowledged separately.(No sliding windows). It has been implemented
   on top of the Internet **User Datagram protocol** (UDP or Datagram) 
   
#### Details
If the server grants the  request, the connection is opened and the file is sent in fixed length blocks of 512 bytes. 
Each data packet contains one block of data, and must be acknowledged by an acknowledgment packet before the next packet can be sent. 
A data packet of less than 512 bytes signals termination of a transfer.  
If a packet gets lost in the network, the intended recipient will timeout and may retransmit his last packet 
(which may be data or an acknowledgment), thus causing the sender of the lost packet to retransmit that lost packet.  
The sender has to keep just one packet on hand for retransmission, since the lock step acknowledgment guarantees that all older packets have been received.  
Notice that both machines involved in a transfer are considered senders and receivers. 
One sends data and receives acknowledgments, the other sends acknowledgments and receives data.

 Most errors cause termination of the connection.  An error is
   signalled by sending an error packet.  This packet is not
   acknowledged, and not retransmitted (i.e., a TFTP server or user may
   terminate after sending an error message), so the other end of the
   connection may not get it.  Therefore timeouts are used to detect
   such a termination when the error packet has been lost.
   
  ---------------------------------------------------
  |  Local Medium  |  Internet  |  Datagram  |  TFTP  |
  ---------------------------------------------------
          
    [TFTP Documents](https://tools.ietf.org/html/rfc1350) 
   
   <hr>
   
 ## DHCP 
 
 
     Server          Client          Server
            (not selected)                    (selected)

                  v               v               v
                  |               |               |
                  |     Begins initialization     |
                  |               |               |
                  | _____________/|\____________  |
                  |/DHCPDISCOVER | DHCPDISCOVER  \|
                  |               |               |
              Determines          |          Determines
             configuration        |         configuration
                  |               |               |
                  |\             |  ____________/ |
                  | \________    | /DHCPOFFER     |
                  | DHCPOFFER\   |/               |
                  |           \  |                |
                  |       Collects replies        |
                  |             \|                |
                  |     Selects configuration     |
                  |               |               |
                  | _____________/|\____________  |
                  |/ DHCPREQUEST  |  DHCPREQUEST\ |
                  |               |               |
                  |               |     Commits configuration
                  |               |               |
                  |               | _____________/|
                  |               |/ DHCPACK      |
                  |               |               |
                  |    Initialization complete    |
                  |               |               |
                  .               .               .
                  .               .               .
                  |               |               |
                  |      Graceful shutdown        |
                  |               |               |
                  |               |\ ____________ |
                  |               | DHCPRELEASE  \|
                  |               |               |
                  |               |        Discards lease
                  |               |               |
                  v               v               v
