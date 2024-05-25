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
- CDP is also enabled on each interface by default

To disable CDP globally:
```
R1(config)# no cdp run
```

To enable CDP globally:
```
R1(config)# cdp run
```

To enable/disable CDP on specific interfaces:
```
R1(config-if)# [no] cdp enable
```

Configure the CDP timer:
```
R1(config)# cdp timer <seconds>
```

Configure the CDP holdtime:
```
R1(config)# cdp holdtime <seconds>
```

Enable/Disable CDPv2:
```
R1(config)# [no] cdp advertise-v2
```

## Link Layer Discovery Protocol
- LLDP is an industry standard protocol (IEEE 802.1AB)
- It is usually disabled on Cisco devices by default, so it must be manually enabled.
- A device can run CDP and LLDP at the same time.
- LLDP messages are periodically sent to multicat address 0180.C200.000E
- When a device recieves an LLDP message, it processes and discards the message. It does NOT forward it to other devices. So, only directly connected devices can become LLDP neighbors
- By default, LLDP messages are sent once every 30 seconds, this is half the time of CDP default 60 seconds.
- By default, the LLDP holdtime is 120 seconds
- LLDP has an additinal timer called the 'reinitialization delay'. If LLDP is enabled (globally or on an interface), this timer will delay the actual initialization of LLDP. 2 seconds by default

### LDP configuration commands
- LLDP is ussually globally disabled by default
- LLDP is also dissabled on each interface by default

- To enable LLDP globally:
```
R1(config)# lldp run
```

- To disable LLDP globally:
```
R1(config)# no lldp run
```
Notice, this is just the same as CDP, just replace the word cdp with LLDP

- To enable LLDP on specific interfaces in the trasmission direction (tx):
```
R1(config-if)# lldp transmit
```

This causes the interface to start senting LLDP messages, however it won't recieve messages.
- To enable LLDP on a specific interface in the recieve direction (rx):
```
R1(config-if)# lldp receive
```

- Configure the LLDP timer:
```
R1(config)# lldp timer <seconds>
```

- Configure the LLDP holdtime
```
R1(config)# lldp holdtime <seconds>
```

- Configure the LLDP reinit timer
```
R1(config)# lldp reinit <seconds>
```


## LLDP basic Commands
```
# show basic information about lldp (timers, version)
R1# show lldp

# displays how may lldb messages/frames have been sent and recieved
R1# show lldp traffic

# displays whether transmit or recieve are enable/disabled on each interface
R1# show lldp interface

# Lists llbp neighbours and some basic information about each neighbor
R1# show lldp neighbors 

# Lists each lldp neighbor with more detailed information
R1# show lldp neighbors detail

# The output of the above command can get quite long with more devices
# Displays the same info as above command, but for the specifed neighbor only
R1# show lldp entry R2
```