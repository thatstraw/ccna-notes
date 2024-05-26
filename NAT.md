# NAT

## Private IPv4 Addresses (RFC 1918)
- IPv4 doesn't provide enough addresses for all devices that need an IP address in the modern world.
- The long-term solution is to switch to IPv6
- There are three main short-term solution:
    1. CIDR
    2. Private IPv4 addreses
    3. NAT

- RFC 1918 specifies the following IPv4 address ranges as private:
    1. 10.0.0.0/8 (10.0.0.0 to 10.255.255.255)
    2. 172.16.0.0/12 (172.16.0.0 to 172.31.255.255)