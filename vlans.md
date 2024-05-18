# Configuring native vlan on ROAS

```groovy
# encapsulation dot1q vlan-id native

or configuring the router's interface with subinterface ip addres
```

# Return an interface to its default settings

```groovy
(config)# default interface g0/1
```

## Inter-VLAN routing via SVI

### Enable Layer 3 routing on a switch

```groovy
sw2(config)# ip routing
sw2(config)# interface g0/1
sw2(config-if)# no switchport #config interface routed port (layer 3 port)
sw2(config-if)#  ip address *address netmask*
sw2(config-if)# do show ip interface brief
sw2(config-if)# exit

### configuring the default route or gateway of last resort
sw2(config)# ip route 0.0.0.0 0.0.0.0 192.168.1.194
sw2(config)# do show ip route
sw2(config)# show interface status
```

**Switch Virtual Interface Configuration**

```notion
Sw-Floor-1# configure terminal
Sw-Floor-1(config)# interface vlan 1
Sw-Floor-1(config-if)# ip address 192.168.1.20 255.255.255.0
Sw-Floor-1(config-if)# no shutdown
Sw-Floor-1(config-if)# exit
Sw-Floor-1(config)# ip default-gateway 192.168.1.1
```

Conditions for the SVI to be up up

1. The VLAN must exist on the switch
2. The switch must have at least one access port in the VLAN in an up/up state, AND/OR one trunk port that allows the VLAN that is in an up/up state.
3. The VLAN must not be shutdown ( you can use the shutdown command to disable a VLAN)