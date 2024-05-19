# Open Shortest Path First

- Router store information about the network in LSAs (Link State Advertisements) which are organized in structure called LSDB (Link State Database)

- Routers will flood LSAs until all routers in the OSPF area develop the same map of the network (LSDB)

- Each LSA has an aging timer (30 min by default). The LSA will be flooded again fater the timer expires.

## OSP Areas
- Ospf uses areas to devide up the network
- small networks can be single-area without any negative effects on perfomance
- In large networks, a single-area design can have negative effects

## What is an Area?

- An area is a set of routers and links that share the same LSDB
- Backbone area is an areas where all other areas must connect to.
- routers with all interfaces in the same area are calle internal routers
- routers with interfaces in multiple areas are called area border routers (ABRs)
- ABRS mantain a separate LSDB for each are they are connected to. It is recommened that you connect ABR to a maximum of 2 areas. Connecting more than that can overburded the router.
- Routers connected to to the backbone area (area 0) are called backbone routers.
- An intra-area route is a route to a destination inside the same ospf area
- An interarea route is a route to a destination in a different ospf area


## Important rules about ospf
- Ospf areas should be contiguous
- All ospf areas must have at least one ABR connected to the backbone area.
- Ospf interfaces in the same subnet must be in the same area

## Configuring ospf
```
R1(config)# router ospf ?
R1(config-router)# network <netword-address> <bitwise mask> <area>

# always use this command on interfaces which don't have any ospf neigbours
R1(config-router)# passive-interface g0/2

# Advertise default route
R1(config-router)# default-information originate

# configure router id (in eigrp it's eigrp router-id)
R1(config-router)# router-id ?

# You need to reload the router for changes to take effect
R1# clear ip ospf process

R1(config)# do show ip protocols

R1(config-router)#  maximum-paths?
R1(config-router)# distance ?

```

> The network command requires you to specify the area  
    The command tells ospf to..
    - look for any interfaces with an IP address contained in the range specified in the network command.
    - Activate ospf on the interface in the specified area
    - Ther router will then ty to become ospf neighbors with other ospf-activated neighbor routers.


## OSPF Specific commands
```
# View ospf databases
R1# show ip ospf database

# show ospf neihbours
R1# show ip ospf neighbours

# current ospf settings on the interfaces
R1# show ip ospf interface


# Changing the reference bandwidth
R1(config-router)# auto-cost reference-bandwidth <megabits-per-second>

# Note: you should configure a reference bandwidth greater than the fastest links in your network (to allow for future upgrades)


# Manually configuring ospf cost of an interface
R1(config-if)# ip ospf cost ?
```

## Formulae to calculate ospf cost
> reference bandwidth / interface bandwidth

## OSPF Neighbors

- When OSPF is activated on an interface, the router starts sending ospf hello messages out the interface at regular interfals (determined by the hello timer). These are use to introduce the router to potential ospf enighbors.
- The default hello timer is 10 seconds on an ethernet connection.
- Hello messages are multicast to 224.0.0.5 (multicast address for all ospf routers)
- OSPF messages are encapsulated in an IP header, with the value of 89 in the protocol field.


##  ospf neighbor state
- The first state is the down state, the router doesn't know about any ospf neighbors yet, so the current neighbor state is Down
- Init state - Hello packet was received, but router-2 own router ID is not in the hello packet
- 2-way state - R2 will send a hello packet containing the RID of both routers. R1 will insert R2 into its ospf neigbor table in the 2 way state. R1 will send another hello message, this time containing R2's RID.
- Exstart - The router with the higher RID will become the Master and initiate an exchange. The router with lower RID will bocome the Slave. To decide the Master and Slave, they exchange DBD (Database Description) packets.
- Exchange state - In this state, the routers exchange DBDs which contain a list of the LSAs in their LSBD.
- Loading - In the loading state, routers send Link State Request (LSR) messages to request that neighbors send them any LSAs they don't have. (Used to request missing LSAs).
    - LSAs are sent in Link State Update (LSU) messages
    - The routers send LSAck messages to acknowledge that they recieved the LSAs.

- Full - In full state, the routers have a full ospf adjacency and identical LSDBs

## OSPF Messages summary
| Type | Name                          | Purpose                                                                                                                                  |
|------|-------------------------------|------------------------------------------------------------------------------------------------------------------------------------------|
| 1    | Hello                        | Neighbour discovery and maintenance                                                                                                      |
| 2    | Database Description (DBD)   | Summary of the LSDB of the router. Used to check if the LSDB of each router is the same                                                  |
| 3    | Link-State Request (LSR)     | Requests specific LSAs from neighbor                                                                                                     |
| 4    | Link-State Update (LSU)      | Sends specific LSAs to the neighbor                                                                                                      |
| 5    | Link-State Acknowledgement (LSAcks)   | Used to acknowledge that the router received a message                                                                                   |

## More ospf configuration
```
# directly enabling ospf on an interface
# <process id> <area>
R1(config-if)# ip ospf 1 area 0


# Configure all interfaces as OSPF passive interfaces
R1(config)# router ospf 1
R1(config-router)# passive-interface default

# Removing interfaces from passive mode
R1(config-router)# no passive-interface g0/0


# Change OSPF inteface priority
R2(config-if)# ip ospf priority ?

```

## Ospf network types
The ospf network type refers to the type of connection between ospf neighbors (Ethernet, etc)

There are three main ospf network types:
1. Broadcast
Enabled by default on Ethernet and FDDI (Fiber Distributed Data Interfaces) interfaces

2.  Point-to-Point
Enabled by default on PPP (Point-to-Point Protocol) and HDLC (High-Level Data Link Control) interfaces

3. Non-broadcast
- enabled by default on Frame Relay and X.25 interfaces

## DR/BDR election order of priority


- 1 - Router with Highest OSPF interface priority
```
# Change OSPF inteface priority
R2(config-if)# ip ospf priority ?
```
If you set the ospf interface priority to 0, the router CANNOT be the DR/BDR for the subnet no matter what.

- 2 - Highest OSPF Router ID


First place becomes the DR for the subnet and second place becomes the BDR

> All interfaces without ospf neighbors automaticaly becomes DR
The default OSPF interface priority is 1 on all interfaces. So router with the highest router ID will become the DR of the segment.