# Layer 2 Discovery Protocols
- Layer 2 discovery protocols such as CDP and LLDP share information with an discover information about neighbouring (connected) devices.
- They are called Layer 2 dicovery protocols because the protocols themselves operate at Layer 2, they don't use IP Addresses
- Although they are layer 2 discovery protocols they can be used to share layer 3 information suchs IP Address.
- The shared information includes host name, IP address, device type, etc.
- CDP is a cisco proprietary protocol
- LLDP is an industry standard protocol (IEEE 802.1AB)
- Beucase they share information about the devices in the network, they can be considered a secrutiy risk and are oten not used. It is up to the network engineer/admin to decide if they want to use them in the network or not.

## Cisco Discovery Protocol
- CDP is a cisco proprietary protocol
- It's enable on Cisco devices (routers, switches, firewalls, IP phones, etc) by default.
- CDP messages are periodically sent to multicast address 0100:0CCC.CCCC
- When a device recieves a CDP message, it processes and discards the message. It does not forward it to other devices. So only directly connected devices can become CDP neighbors
- By default, CDP messages are sent once every 60 seconds out of all interfaces which are in an up state.
- By default, the CDP holdtime is 180 seconds. If a message isn't recieved from a  neighbor for 180 seconds, the nighbor is removed from the CDP neighbor table.
- CDPv2 messages are sent by defaulf.
- CDPv1 is very old, so you probably don't need to use it.

## CDP basic Commands
```
# show basic information about CDP (timers, version)
R1# show cdp

# displays how may CDP messages have been sent and recieved
R1# show cdp traffic

# displays which interfaces CDP is enabled on
R1# show cdp interface

# Lists CDP neighbours and some basic information about each neighbor
R1# show cdp neighbors 

# Lists each CDP neighbor with more detailed information
R1# show cdp neighbors detail

# The output of the above command can get quite long with more devices
# Displays the same info as above command, but for the specifed neighbor only
R1# show cdp entry R2
```

### CDP configuration commands
- CDP is globally enabled by default.