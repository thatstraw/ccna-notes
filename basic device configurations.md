**Switch Virtual Interface Configuration**

```notion
Sw-Floor-1# configure terminal
Sw-Floor-1(config)# interface vlan 1
Sw-Floor-1(config-if)# ip address 192.168.1.20 255.255.255.0
Sw-Floor-1(config-if)# no shutdown
Sw-Floor-1(config-if)# exit
Sw-Floor-1(config)# ip default-gateway 192.168.1.1
```

**Configuring static route**

```bash
R1(config)# ip route *ip-address netmask (next-hop-address|interface)*
```

**View routing table**

```bash
R1(config)# show ip route
```

## Basic Router Configuration Steps

The following tasks should be completed when configuring initial settings on a router.

1. Configure the device name.

```
Router(config)#hostnamehostname
```

2. Secure privileged EXEC mode.

```
Router(config)#enable secret password
```

3. Secure user EXEC mode.

```
Router(config)#line console 0
Router(config-line)#password password
Router(config-line)#login
```

4. Secure remote Telnet / SSH access.

```
Router(config-line)#line vty 0 4
Router(config-line)#password password
Router(config-line)#login
Router(config-line)#transport input {ssh |telnet}
```

5. Secure all passwords in the config file.

```
Router(config-line)#exit
Router(config)#service password-encryption
```

6. Provide legal notification.

```
Router(config)#banner motddelimiter message delimiter
```

7. Save the configuration.

```
Router(config)#end
Router#copy running-config startup-config
```

**Configure Router Interfaces**

```
Router(config)#interfacetype-and-number
Router(config-if)#descriptiondescription-text
Router(config-if)#ip addressipv4-address subnet-mask
Router(config-if)#ipv6 addressipv6-address/prefix-length
Router(config-if)#no shutdown
```

**Verify Interface Configuration**

`R1#  **show ip interface brief**`

## Configuration Verification Commands

The table summarizes the more popular **show** commands used to verify interface configuration.

| Commands | Description |
| --- | --- |
| show ip interface brief

show ipv6 interface brief | The output displays all interfaces, their IP addresses, and their current 
status. The configured and connected interfaces should display a Status 
of “up” and Protocol of “up”. Anything else would indicate a problem 
with either the configuration or the cabling. |
| show ip route

show ipv6 route | Displays the contents of the IP routing tables stored in RAM. |
| show interfaces | Displays statistics for all interfaces on the device. However, this command will only display the IPv4 addressing information. |
| show ip interface | Displays the IPv4 statistics for all interfaces on a router. |
| show ipv6 interface | Displays the IPv6 statistics for all interfaces on a router. |

**IPv6 GUA Configuration on Router R1**

```bash
R1(config)# interface gigabitethernet 0/0/0
R1(config-if)# ipv6 address 2001:db8:acad:1::1/64
R1(config-if)# no shutdown
R1(config-if)# exit
R1(config)# interface gigabitethernet 0/0/1
R1(config-if)# ipv6 address 2001:db8:acad:2::1/64
R1(config-if)# no shutdown
R1(config-if)# exit
R1(config)# interface serial 0/1/0
R1(config-if)# ipv6 address 2001:db8:acad:3::1/64
R1(config-if)# no shutdown
```

**Static Configuration of a Link-Local Unicast Address**

```bash
R1(config)# interface gigabitethernet 0/0/0
R1(config-if)# ipv6 address fe80::1:1 link-local
R1(config-if)# exit
R1(config)# interface gigabitethernet 0/0/1
R1(config-if)# ipv6 address fe80::2:1 link-local
R1(config-if)# exit
R1(config)# interface serial 0/1/0
R1(config-if)# ipv6 address fe80::3:1 link-local
R1(config-if)# exit
```

**Enable Router IPv6 routing**

```bash
R1(config)# ipv6 unicast-routing
```