# ACL
ACLs (Acess Control Lists) have multiple uses
ACLS function as a packet filter, instructing the router to permit or discard specific traffic.
ACLs can filter traffic based on the source/destination IP address, source/destination Layer 4 ports etc.

## How ACLs work
- Configuring an ACL in global config mode will not make the ACL take effect.
- The ACL must be applied to an interface.
- ACLs are applied either inbound or outbound.
- ACLs are made up of one or more ACEs (Access Control Entries)
- When  the router checsk a packet against the ACL, it processes the ACEs in order, from top to bottom.
- If the packet mateches one of the ACEs in the ACL, the router takes the action and stops processing the ACL. All etnries below the matching entry will be ignored.

> A maximum of one ACL can be applied to a single interface per direction.
  Inbound: Maximum one ACL
  Outbound: Maximum one ACL
  If you apply a second ACL to an interface in the same direction as the other one it will replace the previous one

### Implicit Deny
- What will happen if a packet doesn't match any of the entries in an ACL?
- There is an implicity denay at the end of all ACLs.
- The implicity deny tells the router to deny all traffic that doesn't match any of the configured entries in the ACL.
- Always be aware of the implicity deny when configuring ACLs, you might deny traffic that you din't want to deny.

## ACL Types
There are two types of ACLs and those two types have two subtypes

### Standard ACLs
Match based on Source IP address only
 - Standard Numbered ACLs
 - Standard Named ACLs

#### Standard Numbered ACL
- Standard ACLs match traffic based only on the source IP address of the packet.
- The router doesn't check the destination IP, the Layer 4 port, the destination port etc.It just looks at the source IP address of the packet and deside to forward it or block.
- Numbered ACLs are identified with a number (ie ACL 1, ACL 2, etc)
- Different types of ACLs have a different range of numbers that can be used.

    - Standard ACLs can use 1-99 and 1300-1999

- The basic command to configure a standard numbered ACL is:
```
R1(config)# access-list number {deny | permit} ip wildcard-mask

# Examples
# This deny 1.1.1.1/32 (a single host)
R1(config)# access-list 1 deny 1.1.1.1 0.0.0.0

# When you specify a /32 mask in an ACL you don't actually have to specify a wildcard-maks
R1(config)# access-list 1 deny 1.1.1.1

# Here is another method to specify a single hosts (/32), it's considred old method though.
# Using the host keyword before the IP address
R1(config)# access-list 1 deny host 1.1.1.1

# Permit trafic with any source IP
R1(config)# access-list 1 permit any

# This is the same as
R1(config)# access-list 1 permit 0.0.0.0 255.255.255.255

# Configuring a Remark, this is like an interface description, it doesnt' have any effect on the ACL it's just a description that helps you remember the purpose of an ACL when looking at it in the configuration

R1(config)# access-list 1 remark ## BLOCK BOB FROM ACCOUNTING ##

# show all types of configured access-lists
R1# show access-lists

# show only ip access lists
R1# show ip access-lists

# since the remark doesn't appear in the show ip acess-list command
R1# show running-config | include access-lists

## apply ACL tro an interface
R1(config-if)# ip access-group number {in | out}


```
> Rule of thumb to applying standard ACLs to interfaces: Standard ACLs should be applied as close to the destination as possible.

#### Standard Numbered ACL
- Standard ACLs match traffic based on the source IP address of a the packet
- Named ACLs are identified with a name (ie 'BLOCK_BOB')
- Standard named ACLs are configured by entering 'standard named ACL config mode', and then configuring each entry within that config mode.

Here is how you enter that config mode:
```
R1(config)# ip acess-list standard <acl-name>

# Remember to use ip in the command
R1(config-std-nacl)# [entry-number] {deny | permit} ip wildcard-mask


# Notice you can now specify entry numbers, although you don't have to. If you don't entries will be name 10, 20, 30 and so on. Each entry number will be 10 more than the previous one. But with this function you can manually specify the entry number to control the order of the entries


R1(config)# ip acess-list standard BLOCK_BOB
R1(config-std-nacl)# 5 deny 1.1.1.1
R1(config-std-nacl)# 10 permit any
R1(config-std-nacl)# remark ## CONFIGURED NOV 21 2020 ##
R1(config-std-nacl)# int g 0/0
R1(config-if)# ip access-group BLOCK-BOB in

# section display everything whilst include display only the match

R1# show running-config | section access-list

```

### Extended ACLs
Match based on Source/Destination IP, Source/Destination port, etc.
 - Extended Numbered ACLs
 - Extended Named ACLs
# Practical Networking ACLs