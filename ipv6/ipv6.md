## Configuring IPv6
```
# enable router to perform IPv6 routing.
R1(config)# ipv6 unicast-routing
R1(config)# int g0/0
R1(config-if)# ipv6 address <ipv6> 
R1(config-if)# no shut
R1# show ipv6 int brief
```

## Configuring IPv6 Address using EUI
- EUI stands for Extended Unique Identifier
- (Modified) EUI-64 is a method of converting a MAC address (48 bits) into a 64-bit interface identifier.
- This interface identifier can then become the 'host port' of a /64 IPv6 address.

### How to convert the MAC Address
1. Divide the MAC address in half
```
ie. 1234 5678 90AB -> 1234 56 | 78 90AB
```

2. Insert FFFE in the middle
```
1234 56FF FE78 90AB
```

3. Invert the 7th bit (if the 7th bit is 0 make it 1 and vice vesa)
```
1234 56FF FE78 90AB      
 |
 |
 2---> 00[1]0 ----> 0000
                     |
                     |
            1034 56FF FE78 90AB
```

### Configuring an interface using EUi-64
```
# enable router to perform IPv6 routing.
R1(config)# ipv6 unicast-routing
R1(config)# int g0/0
R1(config-if)# ipv6 address  <ipv6> eui-64
R1(config-if)# no shut
R1# show ipv6 int brief
```