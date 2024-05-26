## Ethernet

Ethernet is a collection of network standards and protocols.

## Network speeds

Connections between devices on a network operates at set speeds, these speeds are measured in bits per second.

- a bit - a value represented by either a 0 or a 1.
- byte - a combination of 8 bits.

### Measurements

- 1 Kilobit (Kb) - 1000 bits
- 1 Megabit (Mb) - 1000,000 bits
- 1 Gigabit (Gb) - 1000,000,000 bits
- 1 Terabit (Tb) - 1000,000,000,000 bits

## Ethernet Starndard(copper)

- defined in IEEE 802.3 standard in 1983
- IEEE - stands for Institute of Electrical and Electronic Engineers

| speed | common name | IEEE standard | informal name | maximum length |
| --- | --- | --- | --- | --- |
| 10 Mbps | Ethernet | 802.3i | 10BASE-T | 100 m |
| 100 Mbps | Fast Ethernet | 802.3u | 100BASE-T | 100 m |
| 1 Gbps | Gigabit Ethernet | 802.3ab | 1000BASE-T | 100 m |
| 10 Gbps | 10 Gig Ethernet | 802.3an | 10GBASE-T | 100 m |

<aside>
💡 Base means  baseband signaling and T stands for twissted pair.

</aside>

## Physical Cables

- The copper cables used in *ethernet* standards are unshielded twisted pair cables (UTP).
- Unshielded means the wires have no metalic shield which makes them vulnerable to electrical interference.
- The twist helps to protect against Electromagnetic Interference(EMI).

<aside>
💡 Not all twisted pair calbes uses all 8 wires:
- 10BASE-T and 100BASE-T uses 2 pairs (4 wires)
- 1000BASE-T and 1GBASE-T uses 4 pairs(8 wires)

</aside>

### UTP Cables (10BASE-T and 100BASE-T)

- The network interface card on a PC or Server transmit data on pins 1 and 2, whilst the network interfaces on a switch receives data on pins 1 and 2.
- On the other side the network interfaces on a switch sends data on pins 3 and 6, whilst the the network interface card on a PC or server receives data on pins 3 and 6.
- This allows what is called full-duplex transmission - meaning both devices can send and receive data at the same time and no problems like conclusion will occur since they use separate wires to transmit and receive data.

**Data transmission between a PC and a switch using a straight-through cable.**

![10baset and 100 base t..png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/b7be5176-d4ab-48ee-bcf1-d1f0791522f2/10baset_and_100_base_t..png)

**Data transmission between a router and a switch using a straight-through cable.**

![10baset and 100 base t router and a switch.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/ac1df2bc-f928-4320-8783-50009d792a7c/10baset_and_100_base_t_router_and_a_switch.png)

<aside>
💡 This type of twisted pair cable used above is called a straight-through cable because pin 1 on one end connects  straight-through to pin 1 on the other end, pin 2 connects to pin 2, pin 3 connects to pin 3 etc.

</aside>

What if we want to transmit data between a pc and a pc , a switch and a switch and router and router? To do that we use a cross-over cable:

![swtich and a switch.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/102deabc-c59b-4d20-8f17-a961068e43f1/swtich_and_a_switch.png)

**Here is table which shows different device types and the pins they use to transmit and receive data.**

| Device Type | Transmission (Tx) Pins | Receive (Rx) Pins |
| --- | --- | --- |
| router | 1 and 2 | 3 and 6 |
| PC | 1 and 2 | 3 and 6 |
| Firewall | 1 and 2 | 3 and 6 |
| Switch | 3 and 6 | 1 and 2 |

## Auto MDI-X

Auto MDI-X allows devices to automatically detects which pins their neighbouring devices are using to transmit and receive data on and adjust which pins they transmit and receive data.

### UTP Cables (1000BASE-T and 10GBASE-T)

On this cable all the pins are bi-directional, meaning any pins can be used to transmit or receive data, therefore they’re fast.

## Fiber-Optic Connections

- SFP Transceiver - Small Form-Factor Pluggable.

### Typeos of Optical-Fiber Cables

- single-mode
    - core diameter is narrower than multimode fibre
    - light enters at single angle (mode) from a laser based transmitter
    - allows longer cables than both UTP and multimode
    - more expensive (due to laser-based SFP transmitters)
- multimode
    - core diameter is wide than single mode fibre
    - allows multi angles (modes) of light waves to enter the fibre-glass core
    - allows longer cables than UTP, but shorter cables than single-mode
    - cheaper than single-mode (due to cheaper LED-based SFP transmitters)

## Fibre Optic Standard

| Informal Name | IEEE Standard | Speed | Cable Type | Maximum Lenght |
| --- | --- | --- | --- | --- |
| 1000BASE-LX | 802.3z | 1 Gbps | Multimode or Single-Mode | 550 m (MM)
5 km (SM) |
| 10GBASE-SR | 802.3ae | 10 Gbps | Multimode | 400 m |
| 10GBASE-LR | 802.3ae | 10 Gbps | Single-Mode | 10 km |
| 10BASE-ER | 802.3ae | 10 Gbps | Single-Mode | 30 km |

## UTP vs Fibre Optic Cabling

| UTP | Fiber-Optic |
| --- | --- |
| lower cost than fibre-optic | higher cost than UTP |
| shorter maximum distance than fibre-optic (~100m) | Longer maximum distance than UTP |
| can be vulnerable to EMI | No vulnerable to EMI |
| the RJ45 used with UTP are cheaper than SFP ports | SFP ports are more expensive than RJ45 (single-mode is more expensive than multimode) |
| emit(leak) a faint signal outside the cable which can be copied (=security risk!) |  |