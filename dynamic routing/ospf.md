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
