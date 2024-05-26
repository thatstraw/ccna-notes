## IPv4 Address Classes

| Class | First Octet | First Octet Numeric range | Prefix Length |
| --- | --- | --- | --- |
| A | 0XXXXXXX | 0-127 | /8 |
| B | 10XXXXXX | 128-192 | /16 |
| C | 110XXXXX | 192-223 | /24 |
| D | 1110XXXX | 224-239 |  |
| E | 1111XXXX | 240-255 |  |

Addresses in class D are reserved for multicast addresses.

Address in class E are reserved for research purposes.

## Netmask

- Netmask - determines the number of octects representing the network potion of an IP address and the host potion of an IP Address.
- The netmask is also written in dotted notation where the network potion is all ones and host potion is all zeros.

**Class A : /8** -  255.0.0.0 (11111111 00000000 00000000 0000000)

**Class B: /16** - 255.225.0.0 (11111111 11111111 00000000 0000000)

**Class C: /24** - 255.255.255.0 (11111111 11111111 11111111 0000000)

The Host potion of the address is 0s = Network ID 

The host potion of the  address is 1s = Broadcast address

These two addresses cannot be assigned to a host.

Broadcast address is the layer 3 address used to sent a packet to all hosts on a Local Area Network (LAN)

Network ID - is used to identify the network itself.