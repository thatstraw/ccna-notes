# Network Time Protocol
- All devices have an internal clock (routers, switches, your PC, etc)
- In Cisco IOs, you can view the time with the `show clock` command
- The default time zone is UTC (Coordinated Unversal Time).
- if you use the `show clock detail` command, you see the time source. The hardware calender is the default time source.

```
* = time is not considered authoratative
```
- The internal harware clock of a device will drift over time, so it is not the ideal time source.
- From a CCNA perspective, the most important reason to have accurate time on a device is to have accurate logs for troubleshooting.
- Syslog, the protocol used to keep device logs, will be in later.

The command to view device logs is:
```
R1# show logging 
```

## Manual Software Time Configuration
- You can mannually configure the time on the device with the `clock set` command

```
R1# clock set ?
R1# show clock detail
```

Although the hardware calender (built-in clock) is the default time-source, the hardware clock and software clock are separate and can be configured separately.

## Hardware Clock (Calender) Configuration
- You can manually configure the hardware clock with the `calender set` command.
```
R1# calender set ?
```

- Typically you will want to synchronize the clock (software clock) and calender (hardware clock). Use the command `clock update-calender` to sync the calender to the clock's time.
- Or use the command `clock read-calender` to sync the clock to the calender's time.

## Configuring Time Zone
- You can configure the time zone with the `clock timezone` command.

Important: Time zone is configured from global config mode and becomes part of the running-config of the device.

```
R1(config)# clock timezone ?
```

## Daylight Saving Time (Summer Time)
```
R1(config)# clock summer-time ?
```

## Network Time Protocol
- Manually configuring the time on devices in not scalable.
- The manually configured clock will drift, resulting in inaccurate time.
- NTP (Network Time Protocol) allows automatic syncing of time over a network.
- NTP clients request the time from NTP servers then they synchronize their time with the time of the server.
- A device can be an NTP server and an NTP client at the same time. It will sync it's time to a server and other devices will sync to it. 
- NTP allows accuracy of time within ~1 millisecond if the NTP server is in the same LAN, or within ~50 milliseconds if connecting to the NTP server over a wan/internet.
- Some NTP servers are better than others. The distance of an NTP server from the original reference clock is called stratum. The further away from the reference clock the higher the stratum.
- If the stratum level of a server is high it is considered less accurate. 
- NTP uses UDP port 123 to communicate.

## Reference Clocks
- A reference clock is usually a very accurate time device like an atomic clock or a GPS clock.
- Reference clocks are stratum 0 within the NTP hierarchy. They are as close to the time source as possible, becuase they are the time source.
- NTP servers directly connected to a reference clocks are stratum 1.

---------------NTP Hierarchy Image-------------------------------

- Rerence clocks are stratum 0.
- Stratum 1 NTP servers get their time from reference clocks. 
- Stratum 2 NTP servers get their time from  Stratum 1 NTP servers.
- Stratum 3 NTP servers get their time from  Stratum 2 NTP servers.
- Stratum 15 is the maximum. Anything above that is considered unreliable.
- Devices can also peer with devices at the same stratum  to provide more accurate time. This is called symmetric active mode. Cisco devices can operate in three NTP modes:
    - Server mode
    - Client mode
    - Symmetric active mode
They can be in all of those three modes at the same time too.
- An NTP client can sync to multiple NTP servers.

> NTP servers which get their time directly from reference clocks are also called primary servers
> NTP servers which get their time from other NTP servers are called secondary servers. They operate in server mode and client mode at the same time.

## Configuring NTP

```
# prefer can be used to make the device prefer the specified ntp server.
R1(config)# ntp server <server-ip> prefer
R1(config)# ntp server <server-ip>
```

Show configured NTP servers
```
R1# show ntp associations
```

Show Configured NTP server status
```
R1# show ntp status
```

Note: NTP uses only the UTC time zone. You must configure the appropriate time zone on each device.

```
R1(config)# clock timezone JST 9
```

NTP doesn't update the calender, only update the clock (software time), so we need to make ntp update the calender:
```
R1(config)# ntp update-calender
```

You might be wondering why you would want to sync the hardware clock (calender). The hardware clock tracks the date and time on the divece even if it restarts, power is lost etc. When the system is restarted, the hardware clock is used to initialize the software clock.

Configuring R2 to use R1 as the NTP server
```
R1(config)# interface loopback 0
R1(config-if)# ip address 10.1.1.1 255.255.255.255
R1(config-if)# exit

# Configure R1 interface to be used as the source of NTP messages.
R1(config)# ntp source loopback0


R2(config)# ntp server <R1 loopback IP>
```

## Configuring NTP server mode
Configure a device to operate as NTP server even though it isn't synced to another NTP server:
```
R1(config)# ntp master ?
```
The default stratum of the `ntp master` command is 8.

## Configuring NTP Symetric active mode
Configuring ymetric active mode between R2 and R3

```
R2(config)# ntp peer  <R3 IP Address>
R3(config)# ntp peer  <R2 IP Address>
```

## NTP Authentication
- NTP authentication can be configured, aulthough it is optional
- It allows NTP clients to ensure they only sync to the intended servers
- To configure NTP authentication:

```
# enable NTP authentication
npt authenticate

# create the NTP authentication key(s)
ntp authentication-key <key-number> md5 <key/password-itself>

# specify the trusted key(s)
ntp trusted-key <key-number>

# specify which key to use for each server
ntp server <ip-address> key <key-number>

# The above command isn't need on the server itself (R1) .

```

Actual Configuration
```
R1(config)# ntp authenticate
R1(config)# ntp authentication-key 1 md5 ciscolabs
R1(config)# ntp trusted-key 1


R2(config)# ntp authenticate
R2(config)# ntp authentication-key 1 md5 ciscolabs
R2(config)# ntp trusted-key 1
R2(config)# ntp server <R1 IP Address> key 1
R2(config)# ntp peer <R3 IP address> key 1

R3(config)# ntp authenticate
R3(config)# ntp authentication-key 1 md5 ciscolabs
R3(config)# ntp trusted-key 1
R3(config)# ntp server <R1 IP Address> key 1
R3(config)# ntp peer <R2 IP address> key 1

```
