---
layout: post
title:  "cmd104 Variables"
categories: cmd
---

## ipconfig
Displays all current TCP/IP network configuration values and refreshes Dynamic Host Configuration Protocol (DHCP) and Domain Name System (DNS) settings. `Used without parameters, ipconfig displays Internet Protocol version 4 (IPv4) and IPv6 addresses, subnet mask, and default gateway for all adapters.`

### Frequently Used
* /all	Displays the full TCP/IP configuration for all adapters. Adapters can represent physical interfaces, such as installed network adapters, or logical interfaces, such as dial-up connections.
* /flushdns	Flushes and resets the contents of the DNS client resolver cache. During DNS troubleshooting, you can use this procedure to discard negative cache entries from the cache, as well as any other entries that have been added dynamically.
* /renew [<Adapter>]	Renews DHCP configuration for all adapters (if an adapter is not specified) or for a specific adapter if the Adapter parameter is included. 
<hr>

## netstat
Used without parameters, netstat displays active TCP connections. [docs](https://docs.microsoft.com/en-us/windows-server/administration/windows-commands/netstat)

netstat [-a] [-e] [-n] [-o] [-p <Protocol>] [-r] [-s] [<Interval>]

*Users '-' not '/'*

### Frequently Used
* -a Displays all active TCP connections and the TCP and UDP ports on which the computer is listening.
<hr>

## tracert
Determines the path taken to a destination by sending Internet Control Message Protocol (ICMP) echo Request or ICMPv6 messages to the destination with incrementally increasing time to Live (TTL) field values. The path displayed is the list of near/side router interfaces of the routers in the path between a source host and a destination. The near/side interface is the interface of the router that is closest to the sending host in the path. Used without parameters, tracert displays help.
[docs](https://docs.microsoft.com/en-us/windows-server/administration/windows-commands/tracert)
`tracert [/d] [/h <MaximumHops>] [/j <Hostlist>] [/w <timeout>] [/R] [/S <Srcaddr>] [/4][/6] <TargetName>  `


<hr>

## ping
The ping command verifies IP-level connectivity to another TCP/IP computer by sending Internet Control Message Protocol (ICMP) echo Request messages. The receipt of corresponding echo Reply messages are displayed, along with round-trip times. ping is the primary TCP/IP command used to troubleshoot connectivity, reachability, and name resolution. Used without parameters, ping displays help.
[docs](https://docs.microsoft.com/en-us/windows-server/administration/windows-commands/ping)

### Frequently Used
*/t	Specifies that ping continue sending echo Request messages to the destination until interrupted.

<hr>

## arp
Displays and modifies entries in the Address Resolution Protocol (ARP) cache. The ARP cache contains one or more tables that are used to store IP addresses and their resolved Ethernet or Token Ring physical addresses. There is a separate table for each Ethernet or Token Ring network adapter installed on your computer. Used without parameters, arp displays help information.\
### Frequently Used
* /a Displays current arp cache tables for all interfaces. The /n parameter is case-sensitive.

<hr>

## pathping
Provides information about *network latency and network loss* at intermediate hops between a source and destination. pathping sends multiple echo Request messages to each router between a source and destination over a period of time and then computes results based on the packets returned from each router. Because pathping displays the degree of packet loss at any given router or link, you can determine which routers or subnets might be having network problems.

pathping performs the equivalent of the tracert command by identifying which routers are on the path. *It then sends pings periodically to all of the routers over a specified time period and computes statistics based on the number returned from each. Used without parameters, pathping displays help.*

[docs](https://docs.microsoft.com/en-us/windows-server/administration/windows-commands/pathping)

When pathping is run, the first results list the path. This is the same path that is shown using the tracert command. Next, a busy message is displayed for approximately 90 seconds (the time varies by hop count). During this time, information is gathered from all routers previously listed and from the links between them. at the end of this period, the test results are displayed.

<hr>

## telnet
Communicates with a computer running the telnet Server service.<br>
`telnet [/a] [/e <EscapeChar>] [/f <FileName>] [/l <UserName>] [/t {vt100 | vt52 | ansi | vtnt}] [<Host> [<Port>]] [/?]`


<hr>

## nslookup
Displays information that you can use to diagnose Domain Name System (DNS) infrastructure. Before using this tool, you should be familiar with how DNS works. The nslookup command-line tool is available only if you have installed the TCP/IP protocol.

[docs](https://docs.microsoft.com/en-us/windows-server/administration/windows-commands/nslookup)
<hr>
 
## Netsh
NetshNetsh is a command-line scripting utility that allows you to, either locally or remotely, display or modify the network configuration of a currently running computer.
### Frequently Used
* `netsh winsock reset` reset network 

<hr>

