---
{"dg-publish":true,"permalink":"/information-heap/ovation-network-architecture-eric-angstadt/","noteIcon":"","created":"2025-07-29T09:44:25.517-05:00"}
---

Date: [[-Daily Activity Log-/2025 07-July 29\|2025 07-July 29]]

## topics

- Basic Ovation architecture
- data links
- Scada
- connections
- third-party protocol types
- client/server traffic flow
- subnetting
- gateways
- cisco routers
- access lists
- troubleshooting

## Eric
- senior network engineer
- process-focused
## Terminology
- "Ovation System" is the control system. One Ovation server and everything associated with it.
- "Ovation network(s)"
- "Ovation multi-network system"
- "Ovation multi-network"
## Parts
- Network components
- AV/ standalone domain controller(s) on a separate field network

# Forms
## Minimum version vs basic version
- root switch (basic: with backup root switch, for redundancy)
## Expanded forms
- fan out switches, fan out areas
- local switch connections where controllers and HMI's actually are in the plant. to minimize number of cables running back to the controllers.
- There can be as many fanout areas as there are ports on the switch. 24 ports is possible, with 4 uplink ports.
## Ovation network expanded with "Rings"
- Each ring connects back the root area.
- Allows for root and backup root switch.
- Can be mixed with "regular" fanouts on the same root.
- Switches must support ring protocol.
- Nesting fanout areas is no bueno.

# Port roles on Ovation Switches
- Drop
- Router
- Switch-to-switch
- IP only
- VCH only


| Port Role        |     |
| ---------------- | --- |
| router           |     |
| switch to switch |     |
| drop             |     |
| ip only          |     |
| vch only         |     |

# Comparing topologies
- 3rd generation and gigabit Ovation networks: four uplinks from fanout area to root area.
- Now (3rd gen, gigabit), media converters are no required for fiber switch-to-switch connections

# Gigabit Ovation network: details, pros, and cons
## Details
- only necessary in a system with 250,000 points or more
## Disadvantages
- higher cost
	- switches
	- SFP's and media converters
- distance limitations
	- multimode fiber distance limit is 2km at 100 Mbps, 220 m at 1 Gbps
	- subject to power limitations

# Ring architecture: details, pros, and cons
- Interesting failure modes are introduced
- REP features are advancing

# Network Hardware approved by Ovation
- Categories
	- Multinetwork over LAN
	- Multinetwork over WAN
	- Virtual Ovation
	- Field LAN and DMZ routers
	- 2nd gen, 3rd gen, and gigabit
- Manufacturers
	- Cisco
	- IEM (expansion modules)

### Favorites
- IRK1101-A-K9 (DIN rail)
- ISR1121X-8PLTEP (1U shelf)
- ISR8200-1N-4T (1U shelf)
- IR8340-K9 (2U shelf)


# Communicating to remote and 3rd party networks
- Layer 2 vs Layer 3.
- One Layer 2 network cannot talk to another without a Layer 3 device.
	- Gateways
	- Static routers
- Uses with Ovation systems
	- Core switches, OOW routers, infrastructure switches, Field LAN and DMZ routers
- Jargon
	- Network Access Translation

### Layer 2 vs Layer 3 communication
- MAC address to MAC address

# Subnetting
- Taking a defined address base and segmenting it into more granular domains
- How many bits are reserved for the host address?
- IP addresses are made up of 32 bit numbers
- Jargon:
	- [[Information Heap/CIDR notation\|CIDR notation]]
	- subnet mask
	- host addresses

# Getting Out!(side of your local network)
- A gateway needs to be added
- do not put 2 default gateways on an HMI
- don't add conflicting static routes
- Internet Protocol Version 4 (TCP/IPv4) Properties
- Controller networking
# Common industrial protocols
- modbus
- allen bradley
- EIP
- DNP3
- OPC
- GSM
- TCI
- BACnet
- IEC 61850

OPC DA has a dynamic nature that makes it harder to firewall. OPC DA is legacy, outdated. OPC UA is the successor.
### Communication to enterprise networks
- Aspen
- etc .
- ...

# VLANs and Access Control

- keyword: ACL
- ACL's can block communication as per protocol

---


# Troubleshooting

###  Common issues
- system not wired as per configuration
- speed and duplex mismatched
- subnet masks/IP conflicts
- WAN interruptions
- default gateways / static routes not set or incorrect
### Tips & tricks
- compare drawings and configurations
- confirm end device settings
- review logging
- confirm NTP settings
- Gather UI
- system viewer / system status
- check WAN and VPN tunnel status
- Flash drives are good for loading, instead of console to terminal.
- Console cable
- Programs
	- Tera Term
	- PuTTy
	- Windows Open SSH Client
	- Wireshark
	- Gather UI
	- Ovation Error Log


### router troubleshooting
- show standby brief (with redundant routers)
- show ip route
- show dmvpn (when using vpn tunneling)

### switches and routers troubleshooting
- show version
- show ip int brief
- show run
- show int status
- show log
### HMI command prompt troubleshooting
- ipconfig
- route print
- netstat -a
- tracert (windows version of trace route)
