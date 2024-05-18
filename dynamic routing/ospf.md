# Open Shortest Path First

- Router store information about the network in LSAs (Link State Advertisements) which are organized in structure called LSDB (Link State Database)

- Routers will flood LSAs until all routers in the OSPF area develop the same map of the network (LSDB)

- Each LSA has an aging timer (30 min by default). The LSA will be flooded again fater the timer expires.

## OSP Areas
- Ospf uses areas to devide up the network
- small networks can be single-area without any negative effects on perfomance
- In large networks, a single-area design can have negative effects

What is an Area?

- An area is a set of routers and links that share the same LSDB
- Backbone area is an areas where all other areas must connect to.
- routers with all interfaces in the same area are calle internal routers
- routers with interfaces in multiple areas are called area border routers (ABRs)
- ABRS mantain a separate LSDB for each are they are connected to. It is recommened that you connect ABR to a maximum of 2 areas. Connecting more than that can overburded the router.
- Routers connected to to the backbone area (area 0) are called backbone routers.
- An intra-area route is a route to a destination inside the same ospf area
- An interarea route is a route to a destination in a different ospf area