# IPv6

## Overview
1. Features
   1. Expanded Addressing Capabilities
      1. from 4 Bytes to 16 Bytes for addressing.
      2. greater addressable nodes and auto-configuration of addresses.
      3. scalable multicast routing
      4. new address type "anycast address" defined to send packets to any one of a group of nodes.
   2. Header Format Simplification
   3. Improved Support for Extensions and Options
   4. Flow Labeling Capability
   5. Authentication and Privacy Capabilities
      1. Support authentication(pki), data integrity(hmac), and data confidentiality (encryption)
2. IPv6 Header Format
   1. 4-bit IP version number (0110 for 6)
   2. 8-bit traffic class field
   3. 20-bit flow label
   4. 16-bit unsigned integer payload length (length  of the packet following the header in octets + length of extension headers)
   5. 8-bit selector Next Header identifies the type of header immediately following the IPv6 header.
   6. 8-bit unsigned integer Hop Limit decremented by 1 each node that forwards the packet. Packet is discarded if decremented to zero.
   7. 128-bit Source Address
   8. 128-bit Destination Address
3. IPv6 Extension Headers
   1. An optional Internet-layer information is encoded in **separate headers** that may be placed between the IPv6 header and the upper layer header in a packet.
   2. There are a small number of such extension headers, each identified by a distinct Next Header value. 
   3. The extension headers are not examined or processed by any node along a packet's delivery path, until the packet reaches the node(or each set of nodes in case of multicast) identified in the destination address field of the IPv6
   4. The contents and semantics of each extension header determine whether or not to proceed to the next header. Therefore, extension header must be processed strictly in the order they appear in the packet.
      1. A receiver must not scan through a packet looking for a particular kind of extension header and process that header prior to processing all preceding ones.
      2. If the current header is unrecognized, an ICMP Parameter Problem message is sent back to the source of the packet.
   5. Recommended extension header order is IPv6 header => Hop-by-Hop Options header => Destination Options header => Routing header => Fragment header => Authentication header => Encapsulating Security Payload header => Destination Options header => Routing header => Fragment header
4. Options
   1. extensions headers has defined Options headers which has the format
      1. 8-bit identifier for Option Type
      2. 8-bit unsigned integer for Opt Data Len for length of the Option Data field in octets.
      3. variable-length field for Option Data with length specified option data length
   2. Must be processed in order
   3. Option Type identifiers are internally encoded such that their highest-order two bit specify the action must be taken if the processing IPv6 node does not recognize the Option Type.
      1. Third-highest-order bit of the Option Type specifies whether or not the Option Data of that option can change en-route to the packet's final destination.

## Auto-configuration
1. Stateless
   1. Definition
      1. An important feature for IPv6 as it allows the various devices attached to an IPv6 network to connect to the Internet using the Stateless Auto Configuration without requiring any intermediate IP support in the form of a Dynamic Host Configuration Protocol server. 
         1. A DHCP server holds a pool of IP addresses that are dynamically assigned for a specific amount of time to the requesting node in a LAN. This increases management overhead and a single point of failure. 
         2. IPv6 allows the network devices to automatically acquire IP addresses and also has provision for renumbering/reallocation of the IP addresses en masse. 
      2. Stateless because this method doesn't require the host to be aware of its present state so as to be assigned an IP address by the DHCP server.
   2. Stateless Auto Configuration Process
      1. Link-Local Address Generation
         1. The device is assigned a link-local address. It comprises of '1111111010' as the first ten bits followed by 54 zeroes and a 64 bit interface identifier.
      2. Link-Local Address Uniqueness Test
         1. In this step, the networked device ensures that the link-local address generated by it is not already used by any other device i.e. the address is tested for its uniqueness.
      3. Link-Local Address Assignment
         1. Once the uniqueness test is cleared, the IP interface is assigned the link local address. The address become usable on the local network but not over the Internet.
      4. Router Contact
         1. The networked device makes contact with a local router to determine its next course of action in the auto configuration process.
      5. Router Direction
         1. The node receives specific directions from the router on its next course of action in the auto configuration process.
      6. Global Address Configuration
         1. The host configures itself with its globally unique Internet address. The address comprises of a network prefix provided by the router together with the device identifier.
   3. Neighbor Discovery
      1. Neighbor Discovery Protocol (NDP) is an improvement over the ICMP. It is a messaging protocol that facilitates the discovery of neighboring devices over a network. It uses two kinds of addresses, unicast and multicast addresses. 
      2. It performs nine specific tasks that are divided into three function groups
         1. Host-Router Discovery Functions
            1. Router Discovery, Prefix Discovery, Parameter Discovery, Address Autoconfiguration
         2. Host-Host Communication Functions
            1. Address Resolution, Next-Hop Determination, Neighbor Unreachability Detection, Duplicate Address Detection
         3. Redirect Function
2. Stateful (DHCPv6)
   1. Definition
      1. Allows hosts to obtain addresses and other configuration information from a server.
      2. Allows network parameter assignment at a single DHCP server or a group of such server located across the network. It's made possible with the automatic assignment of IP addresses, default gateway, subnet masks and other IP parameters. 
         1. On connecting to a network, a DHCP configured node sends a broadcast query to the DHCP server requesting for necessary information. Upon receipt of a valid request, the DHCP server assigns an IP address from its pool of IP Addresses and other TCP/IP configuration parameters such as the default gateway and subnet mask. The broadcast query is initiated just after booting and must be completed before client initiates IP-based communication with other devices over the network.
   2. Three Address Allocation Modes
      1. Dynamic
         1. The client is allotted an IP address for specific period of time ranging from a few hours to a few months. At any time before the expiry of the lease, a DHCP client can request a renewal of the current IP address.
         2. Expiry of the lease during a session leads to a dynamic renegotiation with the server for the original or new IP address.
      2. Automatic (DHCP Reservation)
         1. An IP address is chosen from the range defined by the network admin and permanently assigned to the client.
      3. Manual
         1. The client manually selects the IP addresses and uses the DHCP protocol messages to inform the server of the choice of the IP address

## Terminology
1. Node - A device that implements IPv6
2. Router - A node that forwards IPv6 packets not explicitly addressed to itself
3. host - any node that is not a router
4. upper layer - a protocol layer immediately above IPv6.
5. link - a communication facility or medium over which nodes can communicate at the link layer. i.e., the layer immediately below IPv6 like Etherenets, PPP links, X.25, Frame relay, or ATM networks. Or the Internet (higher) layer "tunnels" such as tunnels over IPv4/6 itself.
6. neighbors - a node attached to the same link
7. interface - a node's attachment to a link
8. address - IPv6-layer identifier for an interface or a set of interfaces.
9.  packet - an IPv6 header plus payload
10. Link MTU - the maximum transmission unit, i.e., maximum packet size in octects, that can be conveyed over a link.
11. Path MTU - the minimum link MTU of all the links in a path between a source node and a destination node.

