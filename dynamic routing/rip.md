# Configuring RIP

```
R1(config)# router rip

# config the router to use RIPv2
R1(config-router)# version 2 

# disable classful
R1(config-router)# no auto summary

# configure interfaces to enable rip on
R1(config-router)# network 10.0.0.0

# Configure an interface to be passive mode
R1(config-router)#

# Stop the router from sending rip addvertisement out of the interface
R1(config-router)# passive-interface g0/0
```