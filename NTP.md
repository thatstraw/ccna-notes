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