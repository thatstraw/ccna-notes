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

## VRRP (Virtual Router Redundancy Protocol)
- Open standard
- A master and backup router are elected.
- Multicast IPv4 address: 224.0.0.18
- Virtual Mac Address: 0000.5e00.01xx (xx= VRRP group number)
- In a situation with multiple subnets/vlans, you can configure a different master router in each subnet/vlan to load balance

## GLBP (Gateway Load Balancing Protocol)
- cisco proprietary
- load balances among routers withing a single subnet
- An AVG (Active Virtual Gateway) is elected.
- Up to four AVFs (Active virtual Forwarders) are assigned by the AVG (the AVG itself can be an AVF, too)
- Each AVF acts as the default gateway for a portion of the hosts in the subnet
- Multicast IPv4 address: 224.0.0.102
- Virtual Mac address: 0007:b400:XXYY (XX = GLBP group number, YY=AVF number)

## Configuring HSRP

```
# Virtual IP .254
# HSRP is configured directly on the interface with the default gateway
R1(config)# int g0/0

# Change the HSRP version number
R1(config-if)# standby version 2
R1(config-if)# standby <group-number> ip <virtul-ip>

# The active router is determined in this order
# 1 - Highest priority (default 100)
# 2 - Highest IP address

R1(config-if)# standy <group-number> priority <value>

# Prempty cause the router to take the role of active router, even if another router alread has the role.
# Note: only configure preepmt on the router you want to become the active router
# Even if you don't configure preempt on R2, R1 will still prompt R2 to take back the active role
R1(config-if)# standy <group-number> preempt

# ---------------------------R2--------------------------------
# HSRP is configured directly on the interface with the default gateway
R2(config)# int g0/0
R2(config-if)# standby version 2
R2(config-if)# standby <group-number> ip <virtul-ip>

# Not realy necessary..just for consistence between the routers
R2(config-if)# standy <group-number> priority <value>
R1(config-if)# standy <group-number> preempt

# Important: HSRP version 1 and version 2 are not compatible. If R1 uses version 2, R2 must use version 2 also.
# Again the HSRP group number and VIP must match between the routers.

# used to display HSRP configuration
R1# show standby
```