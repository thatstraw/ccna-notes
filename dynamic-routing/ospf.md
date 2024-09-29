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
R1(config-router)# network <netword-address> <bitwise mask> area <area>

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
# This cost will take priority over the auto calculated cost.
R1(config-if)# ip ospf cost ?


# Configure ospf network type on an interface
R1(config-if)# ip ospf network ?

# Configure ospf directly on an interface
R1(config-if)# ip ospf 1 area 0
```

## Formulae to calculate ospf cost
> reference bandwidth / interface bandwidth

- Ospf metric is called cost
- it is automatically calculated based on the bandwidth (speed) of the interface.
- You can also manually configure the cost of each interface.
- It is calculated by deviding the reference bandwidht value by the interface's bandwidth.
- The default ospf reference bandwidth is 100mbs
      **Reference**: 100mbs / **interface**: 10mbs = cost of 10
      **Reference**: 100mbs / **interface**: 100mbs = cost of 1
      **Reference**: 100mbs / **interface**: 1000mbs = cost of 1
      **Reference**: 100mbs / **interface**: 10000mbs = cost of 1
  In ospf all values less than 1 will be converted to 1
  - Therefore fast ethernet, Gigabit ethernet, 10Gig Ethernet etc. are equal and all have the same cost of 1 by default.
  - You can (and should) change the reference bandwidth with this command.
 - loopback interfaces have a cost of 1 
  ```
  R2(config-router)# auto-cost reference-bandwidth <mega-bits-per-second>
  ```
  - You should configure the reference bandwidth greater than the fastest links in your network to allow for future upgrades.
  - You should configure the same reference bandwidth all the ospf routers in your network.

**One more option to change the OSPF cost of an interface is to change the bandwidth of the interface with the bandwidth command.**

- The formula to calculate OSPF cost is **reference bandwidth / interface bandwidth**.
- Although the bandwidth matches the interface speed by default, changing the interface bandwidth **doesn’t actually change the speed at which the interface operates.**
- The bandwidth is just a value that is used to calculate OSPF cost, EIGRP metric, etc.
- To change the speed at which the interface operates, use the **speed** command.
- Because the bandwidth value is used in other calculations, it is not recommended to change this value to alter the interface’s OSPF cost.
- It is recommended that you change the **reference bandwidth**, and then use the **ip ospf cost** command to change the cost of individual interfaces if you want.

```
# to change the interface bandwidth
R1(config-if)# bandwidth ? 
```

## OSPF Neighbors

- Making sure that routers successfully become OSPF neighbors is the main task in configuring and troubleshooting OSPF.
- Once routers become neighbors, they automatically do the work of sharing network information, calculating routes, etc.
- When OSPF is activated on an interface, the router starts sending ospf hello messages out the interface at regular interfals (determined by the hello timer). These are use to introduce the router to potential ospf enighbors.
- The default hello timer is 10 seconds on an ethernet connection.
- Hello messages are multicast to 224.0.0.5 (multicast address for all ospf routers)
- OSPF messages are encapsulated in an IP header, with the value of 89 in the protocol field to indicate ospf


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

## Configuring loopback address on a router
```
Router> enable
Router# configure terminal
Router(config)# interface loopback 0
Router(config-if)# ip address 192.168.1.1 255.255.255.0
Router(config-if)# no shutdown
```

Here, 0 is the interface number. You can create multiple loopback interfaces by incrementing the number (e.g., loopback 1, loopback 2, etc.).


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

- In the broadcast network type, routers will only form a full ospf adjacency with the DR and BDR of the segment.
- Therefore, routers only exchange LSAS with DR and BDR. DROthers will not exchange LSAs with each other.
- All routers will still have the same LSDB, but this reduces the amount of LSAs flooding the network.
- When routers want to send messages to the DR/BDR they use multicast address 224.0.0.6.

- The DR and BDR will form a FULL adjacency will ALL routers in the subnet. DROthers will form a FULL adjacency only with the DR/BDR

## OSPF Point-to-Point Network

- Enabled on serial interfaces using the PPP or HDLC encapsulations by default
- A DR and BDR are not elected.
- These encapsulations are used for point to point connections.
- Therefore there is no point in electing a DR and BDR
- The two routers will form a Full adjacency with each other.

### Serial Connections
- One side of a serial connection functions as DCE (Data Communication Equipment)
- The other side functions as DTE (Data Terminal Equipment)
- The DCE side needs to specify the clock rate (speed) of the connection

```
# configuring serial interface speed
R1(config)# int s2/0
R1(config-if)# clock rate <bs>

#The default encapsulation on a serial interface is HDLC
# configure an interface to use another encapsulation for serial connection
R1(config-if)# encapsulation ppp

# Important: If you change the encapsulation, it must match on both ends or the interface will go down.
```

*How to check which side is DCE and which side is DTE*
```
R1# show controllers s2/0
```

## OSPF Neighbor Requirements
- The area number must match between the two routers
- The interface must be in the same subnet to become neighbors
- The ospf process must not be shutdown
    ```
    # in the opsf configuration you can shutdown the ospf process
    R2(config)# router ospf 1
    R2(config-router)# shutdown
    ```
- The ospf router IDs must be unique
- The Hello and Dead timers must match
    ```
    # hello and dead timers can be configured on the interface
    R2(config-if)# ip ospf hello-interval ?
    R2(config-if)# ip ospf dead-interval ?

    # Here is how you can return the timers to their default values
    R2(config-if)# no ip ospf hello-interval
    R2(config-if)# no ip ospf dead-interval
    ```
- Authentication settings must match
    ```
    # ospf password can be configured on an interface too
    R2(config-if)# ip ospf authentication-key cisco
    R2(config-if)# ip ospf authentication

    # removing the authentication
    R2(config-if)# no ip ospf authentication-key cisco
    R2(config-if)# no ip ospf authentication
    ```
- IP MTU settings must match
    > Important: even if the IP MTU settings don't match the routers can become neighbors
    but OSPF doesn't operate properly

    ```
    # change ip mtu
    R2(config-if)# ip mtu ?
    ```
- The ospf network type must match

## OSPF LSA types
- The OSPF LSDB is made up of LSAs
- All routers in the same area share the same LSDB
- There are 11 types of LSA, but there are only 3 you show be aware of for the CCNA:
    - Type 1 (Router LSA)
    - Type 2 (Network LSA)
    - Type 5 (AS External LSA)

### Type 1 (Router LSA)
- Every ospf router generates this type of LSA
- It identifies the router using it's router ID
- It also lists networks attached to the router's ospf-activated interfaces

### Type 2 (Network LSA)
- Generated by the DR of each "multi-access" network (i.e the broadcast network)
- Lists the routers which are attached to the multi-access network

### Type 5 (AS-External LSA)
- Generated by ASBRs to describe routers to destination outside of the AS (OSPF domain).

```
# display the LSDB on the routers
R1# show ip ospf database

# Note: this command will display the same output on all routers, since routers share the same LSDB
```
