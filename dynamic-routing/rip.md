# Configuring RIP
RIPv1 Broadcast messages
RIPv2 send multicast messages via 224.0.0.9


## Configuring RIPv2
```
R1(config)# router rip

# config the router to use RIPv2
R1(config-router)# version 2 

# disable classful
R1(config-router)# no auto summary

# configure interfaces to activate  rip on
R1(config-router)# network 10.0.0.0

# Configure an interface to be passive mode
R1(config-router)#

# Stop the router from sending rip addvertisement out of the interface
R1(config-router)# passive-interface g0/0

# Tell routers about the default route to the internet

R1(config-router)# default-information originate 

# Configure maximum paths for load balancing routes (this command can be used on ospf, rip and eigrp)
R1(config-router)# maximum-paths 12


## show states of all routing protocols (rip, eirgp, ospf)
R1# show ip protocols
```

If you want rip routes to prefered over other routes learned from other routing protocols, change the rip distance:
```
R1(config-router)# distance 24
```