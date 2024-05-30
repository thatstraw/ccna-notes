# Domain Name Services
- DNS is used to reseolve human-readable names (google.com) to IP addresses.
- Machines such as PCs don't use names, they use addresses (i.e IPv4/IPv6).
- Names are much easier for us to use and remember than IP addresses.
    - What is the IP address of youtube.com? Chances are you have no idea. Thanks to DNS you can acess youtube.com without having to remember the IP address.
- When you type 'youtube.com' into a web browser, your device will ask a DNS server for the IP address of youtube.com.
- The DNS server(s) your device uses can be manually configured or learned via DHCP.


## DNS Records types 
- A - Used to map names to IPv4 addresses
- AAAA- Used to map names to IPv6 addreses
- CNAME - Canonical Name is another DNS record that basically map a name to another name.
Note: Standard DNS queries/responses typically use UDP. TCP is used for DNS messages greater than 512 bytes. In either case, port 53 is used.

## DNS Cache
Devices will save the DNS server's responses to a local DNS cance. this means they don't have to query the server every single time they want to access a particular destination.

To view the DNS cache on a Windows PC, use:
```
> ipconfig /displaydns
```

To clear the DNS cache on Windows PC, use:
```
> ipconfig /flushdns
```

## Host file
An alternative to DNS

## DNS in Cisco IOs
- For hosts in a network to use DNS, you don't need to configure DNS on the routers. They will simply forward the DNS messages like any other packets.
- However, a Cisco router can be configured as a DNS server, although it's rare.
    - If an internal DNS server is used, ussually its a Windows or Linux server.
- Also, a cisco router can also be configured as a DNS client.

### Configuring a Cisco router as a DNS server
```
# Configure R1 to act as a DNS server
R1(config)# ip dns server

# Configure a list of hostane/ip address mapping
R1(config)# ip host R1 192.168.0.1
R1(config)# ip host PC1 192.168.0.4
R1(config)# ip host PC2 192.168.0.5

# Configure an External DNS server that R1 will query if the requested record isn't it its host table.
R1(config)# ip name-server 8.8.8.8

# Enable R1 to perform DNS queries (enabled by default) (old version of the command is ip domain-lookup)
R1(config)# ip domain lookup
```

To view the hosts configured and the ones cached use:
```
R1# show hosts
```

Optional: configure the default domain name
```
R1(config)# ip domain name cisco.com

# or old version
R1(config)# ip domain-name cisco.com

```
This will be automatically appended to any hostnames without a specified domain.
For example:

```
> ping pc1
```
Will become:
```
> ping pc1.cisco.com
```


