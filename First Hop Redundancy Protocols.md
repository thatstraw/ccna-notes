# First Hop Redundancy Protocols 

A first hop redundancy protocol (FHRP) is a computer networking protocol which is designed to protect the default gateway used  on a subnetwork by allowing two or more routers to provide backup for that address; in the event of failure of an active router, the backup router will take over the address, usually within a few seconds.

- Gratuitous ARP - are ARP replies sent without being requested (no ARP request message was recieved). The frames are broadcast to FFFF.FFFF.FFFF (normal ARP replies are unicast)

### How FHRPs works
- A virtual IP is configured on the two routers, and a virtual MAC is generated for the virtual IP (each FHRP uses a different fformat for the virtual MAC )
- An active router and a standby router are elected. (different FHRPs use different terms.)
- End hosts in the network are configured to use the virtual IP as their default gateway.
- The active router replies to ARP requests using the virtual mac address, so the traffic destined for other networks will be sent to it.
- If the active router fails, the standby becomes the next active router. The new active router will send gratuitous ARP messages so that switches update their MAC address tables. It now functions as the default gateway
- If theold active router comes back online, by default it won't take back its role as the active router. It will become the standby router.


## HSRP (Hot Standby Router Protocol)
- Cisco proprietary
- An active and standby router are elected
- There are two versions: version 1 and version 2. Version 2 ands IPv6 support and increases the number of groups that can be configured.
- Multicast IPv4 address: v1 = 224.0.0.2 and v2  = 224.0.0.102
- Virtual MAC  address: v1 = 0000.0c07.acxx (xx=HSRP group number) and v2 = 0000.0c9f.fxxxx (xxx= HSRP group number)
- In a situation with multiple subnets/vlans, you can configure a different active router in each subnet/vlan to loadbalance