# Open Shortest Path First

- Router store information about the network in LSAs (Link State Advertisements) which are organized in structure called LSDB (Link State Database)

- Routers will flood LSAs until all routers in the OSPF area develop the same map of the network (LSDB)

- Each LSA has an aging timer (30 min by default). The LSA will be flooded again fater the timer expires.

## OSP Areas
- Ospf uses areas to devide up the network
- small networks can be single-area without any negative effects on perfomance
- In large networks, a single-area design can have negative effects