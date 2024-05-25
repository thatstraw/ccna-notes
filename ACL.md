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

You can also configure numbered ACL like you are configuring named ACLs for example:
```
R1(config)# ip acess-list standard <number>
```
These has several advantages.

### Advantages of named ACL config mode
- You can easily delete individual entries in the ACL with "no entry-number" command in the ACL config mode.

Imortant: When configuring/editing numbered ACLs from global config mode, you can't delete individual entries, you can only delete the entire ACL! 

If you try to delete a single entry with the following command, it will delete the entire ACL:
```
R1(config)# no access-list 1 deny 1.1.1.1 0.0.0.0
R1(config)# do show access-lists
R1(config)# do show running-config | section access-list
```

So if you want to edit ACLs, should definetely use named ACL config mode. If you want you can use global config mode to configure a numbered ACL and when you want to edit, just use named ACL config mode.

- You can insert new entries in between other entries by specifying the sequence number.

### Resequencintg ACLs
- There is a resequencing function that helps edit ACLs.
- The command ip access-list resequence acl-id starting-seq-num increment

```
# This command is done in global config mode, and it works for all the ACL types, standard and extended ACLs

R1(config)# ip  access-list resequence 1 10 10
```

### Extended ACLs
- Extended ACLs function mostly the same as standard ACLs
- They can be numbred or name, just like standard ACLs
    - Extended Numbered ACLs
    - Extended Named ACLs
- Numbered ACLs use the following ranges: 100-199, 2000-2699.
- They are processed from top to bottom, just like standard ACLs
- However, they can match traffic based on more parameters, so they are more precise (and more complex) than standard ACLs.
- They can match based on Source/Destination IP, Source/Destination port, etc.

To configured  extended numbered ACL:
```
R1(config)# access-list number [permit | deny] protocol src-ip dest-ip
```

To configure extended name ACL:
```
R1(config)# ip access-list  extended {name|number}
R1(config-ext-nacl)# [seq-num] [permit | deny] protocol src-ip dest-ip
```

Important: In extended ACLs, to specify a /32 source or destination you have to use the host option or specify the wildcard-mask. You can't just write the adddress without either of those. But remember in standard ACLs we can ommit either of those.

> tcp protocol, tells the router to deny all packets that encapsulate a TCP segment, from any source, to destination specified (10.0.0.0/24)

### Matching the TCP/UDP port numbers
- When matching tcp/udp, you can optionally specify the source and/or detination port numbers to match.
```
R1(config-ext-nacl)# deny tcp src-ip eq src-port-num dest-ip eq dst-port-num
```
- eq  80 = equal to port 80
- gt  80 = greater than port 80 (81 and greater)
- lt  80 = less than port 80 (79 and less)
- neq 80 = NOT 80
- range 80 100 = from port 80 to port 100

After the destination ip address  and/or destination port numbers, there are many more options you can use to match (not necessary for the CCNA):
- ack: match the TCP ACK flag
- fin: match the TCP FIN flag
- syn: match the TCP SYN flag
- ttl: match packets with a specific TTL value
- dscp: match packets with a specific Diferentiated Service Code Point (DSCP) value  in the IPv4 header

If you specify the protocol, source IP, source port, destination IP, destination port, etc, a packet must match ALL of those values to match the ACL entry. Even if it matches all exept one the parameters, the packet won't match that entry in the ACL.

Important: Extented ACLs should be applied as close the source as possible, to limit how the packets travel in the network before being denined.
(Standard ACLs are less specific, so they are applied close to the source there is a rist of blocking more traffic than intended.)

Show ACLs applied to an interface:
```
R1# show ip interface g0/0
```