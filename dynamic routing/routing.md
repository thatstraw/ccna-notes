

## Routing Protocols

## IGP - Interior Gateway Protocol
    - Distace Vector
        Routing Information Protocol (RIP)
        Enhanced Interior Gateway Routing Protocol (EIGRP)

    - Link State
        Open Shortest Path First (OSPF)
        Intermediate System to Intermediate System (IS-IS)

## EGP - Exterior Gateway Protocol
    - Path Vector
        Border Gateway Protocol(BGP)

## Displaying avaible routes

```

# show all the routers
R1(config)# show ip route

# show only static route
R1(config)# show ip route static

# show only ospf route
R1(config)# show ip route ospf

# show eigrp routes
R1(config)# show ip route eigrp


# show routing topology for example eigrp
R1(config)# show ip eigrp topology
```
