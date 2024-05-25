# Enhanced Interior Routing Protocol notes

EIGRP send multicast messages via 224.0.0.10



## Configuring EIGRP 
```

# The AS (Autonomous System) number must match between the routers, or they will form an adnacency and share routing information
R1(config)# router eigrp 1

# disable classful
R1(config-router)# no auto summary

# configure interfaces to activate  rip on
R1(config-router)# network 10.0.0.0

# You can also configure the actual network followed by bitwise mask

# <network address> and the inverted network prefix
R1(config-router)# network 172.16.1.0 0.0.0.15

# Advertise loopback address interfaces
R1(config-router)# network 1.1.1.1 0.0.0.0

# Configure an interface to be passive mode

# Stop the router from sending rip addvertisement out of the interface
R1(config-router)# passive-interface g0/0

# Tell routers about the default route to the internet

R1(config-router)# default-information originate 

# Configure maximum paths for load balancing routes (this command can be used on ospf, rip and eigrp)
R1(config-router)# maximum-paths 12


## show states of all routing protocols (rip, eirgp, ospf)
R1# show ip protocols
```

If you want eigrp routes to prefered over other routes learned from other routing protocols, change the rip distance:
```
R1(config-router)# distance 24
```

# Wildcard mask

- EIGRP uses wildcard mask instaed of a regular subnet mask
- A wildcard mask is basically an inverted subnet mask.
- All 1s in the subnet mask are 0 in the equivalent wildcard mask. All 0s in the subnet are 1 in the equivalent wildcard mask



## EiGRP Router ID order of priority
1. Manual Configuration
2. Highest IP address on a loopback interface
3. Highest IP address on a physical interface


## Configuring router ID
```
R1(config-router)# eigrp router-id ?

```

## Unequal-Cost Load-Balancing
R1(config-router)# variance ?


## Configuring Loopback interfaces

```

R1(config) interface loopback <loopback-number>

R1(config-if) ip address 1.1.1.1 255.255.255.255

# A loopback interface is always up up unless you manually shutdown it. 

```


## Another way to configure eigrp on all the interfaces
This shortcut is not recommended on real networks

```
R1(config)# eigrp 100 
R1(config-router)# network 0.0.0.0 255.255.255.255
```

## Calculate eigrp metric

```
metric = bandwidth of the slowest link + the delay of all links
```
Thanks
