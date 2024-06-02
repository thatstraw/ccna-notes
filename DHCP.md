# Dynamic Host Configuration Protocol
- DHCP is used to allow hosts to automatically/dynamically learn various aspects of their network configuration, such as IP address, subnet mask, default geteway, DNS server, etc, without manual/static configuration.
- It is an essential part of modern networks.
    - When you connect a phone/laptop to WiFi, do you ask the network admin which IP address, subnet mask, default gateway, etc, the phone/laptop should use?
- DHCP is typically used for 'client devices' such as workstations (PCs), phones, etc.
- Devices such as routers, servers, etc, are ussually manually configured. This is because they need to have a fixed IP address to perform their function.
- In small networks (such as home networks) the router typically acts as the DHCP server for hosts in the LAN.
- In larger networks, the DHCP server is usually a Windows/Linux server.
- DHCP server 'lease' IP address to clients. These leases are usually not permanent, and the client must give up the address at the end of the lease.

### Release an IP address 
```
> ipconfig /release
```

### Renew an IP address 
```
> ipconfig /renew
```

- DHCP servers use UDP port 67
- DHCP clients use UDP port 68

### How DHCP works
1. DHCP Discover: Are they any DHCP servers in this network? I need an IP address.
2. DHCP Offer: How about this IP address?
3. DHCP Request: I want to use the IP address you offered me.
4. DHCP Ack: Okay, you may use it.

```
Discover        Client -> Server    
Offer           Server -> Client       Broadcast or Unicast
Request         Client -> Server       Broadcast
ACK             Server -> Client       Broadcast or Unicast
Release         Client -> Server       Unicast
```

## DHCP Relay
- Some network engineers might choose to configure each router to act as the DHCP server for its connected LANs.
- However, large enterprises often choose to use centralized DHCP server.
- If the server is centralized, it won't recieve the DHCP client's broadcast DHCP messages. (broadcast messages don't leave the local subnet).
- To fix this, you can configure the router to act as a DHCP relay agent.
- If you do that, the router will forward the clients' broadcast DHCP messages to the remote DHCP server as unicast messages.

## DHCP Server Configuration in IOS
```
# Psecify a range of addresses that won't be give to DHCP clients
R1(config)# ip dhcp excluded-address 192.168.1.1 192.168.1.10

# Create a DHCP pool (A DHCP pool is subnet of IP addresses that can be assigned to DHCP clients as well as other info such as DNS server and Default gateway)
# You should create a different DHCP pool for each subnet the router is acting as DHCP server for
R1(config)# ip dhcp pool LAB_POOL

# specify the subnet of addresses to be assigned to clients (except the excluded addreses)
R1(dhcp-config)# network 192.168.1.0 <network-mask or prefix-length>

# Specify the DNS server that DHCP clients should use
R1(dhcp-config)# dns-server 8.8.8.8

# Specify the domain name of the network (ie. PC1 = pc1.cisco.com)
R1(dhcp-config)# domain-name cisco.com

# Specify the default gateway
R1(dhcp-config)# default-router 192.168.1.1

# Specify the lease time.
# lease days hours minutes or
# lease infinite
R1(dhcp-config)# lease 0 5 30

```

Here is another useful command for a cisco router acting as a DHCP server:
```
R1# show ip dhcp binding
```
It shows all the dhcp clients that are currently assigned IP addreses.

## DHCP Relay Agent Configuration
```
# configure the interface connected to the subnet of the client devices
R1(config)# int g0/1

# Configure the IP address of the DHCP server as the helper address.
R1(config-if)# ip helper-address 192.168.10.10

```

## Configuring a Network Device as a DHCP Client
```
# The interface to automatically assign the IP Address
R2(config)# int g0/1

# Use the ip address dhcp mode to tell the router to use DHCP to learn it's IP address
R1(config-if)# ip address dhcp

```

### another command
show ip dhcp pool
show ip dhcp server statics