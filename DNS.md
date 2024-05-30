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