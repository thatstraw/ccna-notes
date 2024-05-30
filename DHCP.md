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
Discover        Client -> Server       Broadcast
Offer           Server -> Client       Broadcast or Unicast
Request         Client -> Server       Broadcast
ACK             Server -> Client       Broadcast or Unicast
Release         Client -> Server       Unicast
```

