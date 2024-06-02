# Simple Network Management Protocol
- SNMP is an industry-standard framework and protocol that was originally released in 1988. 
- RFC 1065 - Structure and identification of management information for TCP/IP-based internets.
- RFC 1066 - Management Information base for network management of TCP/IP-based internets.
- RFC 1067 - A simple network management protocol.

Important: Don't let the 'Simple' in the name fool you!

- SNMP can be used to monitor the status of devices, make configuration changes, etc.
- There are two types of devices in SNMP:
    1. Managed Devices - These are the devices being managed using SNMP. For example, network devices like routers and switches.
    2. Network Management Station/System (NMS) - The device/devices managing the managed devices. This is the SNMP server.


## SNMP Operations
- There are three main operations used in SNMP. In other words, three ways SNMP can be used to help manage a network:
    1. Managed devices can notify the NMS of events.
    2. The NMS can ask the managed devices for information about their current status.
    3. The NMS can tell the managed devices to change aspects of their configuration.

## Components of SNMP

- The SNMP Manager is a software on the NMS that interacts with the managed devices. It recieves notifications, sends requests for information, sends configuration changes etc.
- SNMP Application provides an interface for the network admin to interact with. It displays alerts, statics, charts etc.
- SNMP Agent is the SNMP software running on the managed devices that interacts with the SNMP Manager on the NMS. It sends notifications to/recieves messages from the NMS.
- The Management Information Based (MIB) is the structure that contains the variables that are managed by SNMP.
    - Each variable is identified with an Object ID (OID)
    - Example variables: interface status, traffic throughput, CPU usage, temperature, etc.

## SNMP OIDS
- SNMP Object IDs are organized in a hierarchical struture.

```
.1      .       3.      6.      1.      2.      1.      1.      5
iso        Identified  dod   Internet   mgmt  mib-2     system  sysName
          Organization
```

### SNMP Versions
- Many versions of SNMP have been proposed/developed, however only three major versions have achieved wide-spread use:
- SNMPv1 - The original version of SNMP
- SNMPv2c - Allows the NMS to retrive large amounts of information in a single request, so it is more efficient. The c refers to the 'communnity strings' used as passwords in SNMPv1, removed from SNMPv2, and then added back for SNMPv2c
- SNMPv3 - A much more sucure version of SNMP that supports strong encryption and authentication. Whenever possible, this version should be used.

## SNMP Messages
| Message Class | Description | Messages |
| --- | --- | --- |
| Read | Messages sent by the NMS to read information from the managed devices. (ie. Whats your current CPU usage %?) | Get, GetNext, GetBulk |
| Write | Messages sent by the NMS to change information on the manged devices.
(ie. change an IP address) | Set |
| Notification | Messages sent by the managed devices to alert the NMS of a particular event. (ie. interface going down) | Trap, Inform |
| Response | Messages sent in response to a previous message/request. | Response |