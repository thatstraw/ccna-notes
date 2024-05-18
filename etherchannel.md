```bash
# show etherchannel load-balance
```

```bash
# conf t
# port-channel load-balance src-dst-mac
```

```bash
configuring etherchannel on interfaces

# int range g0/1 - 4

[1] one is the group number
# channel-group 1 mode ? 
```

## PAgP

```bash
auto + auto = no EtherChannel
desirable + auto = EtherChannel
desireable + desirable = Etherchannel
```

## LACP

```bash
passive + passive = No EtherChannel
passive + active = EtherChannel
active + active = EtherChannel
```

## Once interface are configured as port channels:

```bash
# int port-channel 1
# switchport trunk encapsulation dot1q
# switchport mode trunk
# do shw int trunk
```

## Very ehterchannel summary

```bash
show etherchannel summary
show etherchannel port-channel
```

## Layer 3 Etherchannel

```bash
# int range g0/1 - 4
# no switchport # confeting this interfaces to layer 3 routed port
# channel-group 1 mode active
```