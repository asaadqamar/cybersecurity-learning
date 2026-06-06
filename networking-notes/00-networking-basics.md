# Networking Basics

## Overview

Networking fundamentals form the foundation of cybersecurity. Understanding how data travels across networks is essential for identifying vulnerabilities and securing systems.

## The OSI Model

The **Open Systems Interconnection (OSI)** model is a 7-layer framework that standardizes network communication:

### Layer 1: Physical
- **What:** Physical transmission of raw bits
- **Components:** Cables, fiber optics, radio waves, hubs
- **Key Concept:** Electrical signals and physical infrastructure
- **Example:** An Ethernet cable carrying electrical pulses

### Layer 2: Data Link
- **What:** Moving frames between physically connected devices
- **Components:** Switches, MAC addresses, network interface cards (NICs)
- **Key Concept:** MAC (Media Access Control) addressing for local network delivery
- **Example:** Switches learning MAC addresses to forward frames to the right physical ports

### Layer 3: Network
- **What:** Routing packets across multiple networks
- **Components:** Routers, IP addresses, routing protocols
- **Key Concept:** IP addressing (IPv4/IPv6) for logical network identification
- **Example:** Your router deciding how to send packets to distant networks

### Layer 4: Transport
- **What:** End-to-end communication and reliability
- **Components:** TCP, UDP, ports
- **Key Concept:** Ensures data arrives correctly (TCP) or quickly (UDP)
- **Example:** TCP ensuring all web request packets arrive in order

### Layer 5: Session
- **What:** Managing conversations between applications
- **Components:** Session tokens, authentication
- **Key Concept:** Establishing, maintaining, and terminating connections
- **Example:** Keeping your login session active while browsing

### Layer 6: Presentation
- **What:** Data translation and formatting
- **Components:** Encryption, compression, character encoding
- **Key Concept:** Converting data into readable format for applications
- **Example:** Decrypting HTTPS traffic to readable content

### Layer 7: Application
- **What:** User applications and services
- **Components:** HTTP, FTP, DNS, SMTP, SSH
- **Key Concept:** Where users directly interact with network services
- **Example:** Your web browser requesting pages over HTTP

## Local Area Networks (LANs)

### Definition

A **LAN** is a network confined to a small geographic area (office, building, home). Devices connect directly via switches and share the same subnet.

### Key Characteristics

- **Limited Geographic Scope:** Typically within a few kilometers
- **High Speed:** Data transfer rates in Gbps
- **Shared Medium:** All devices connected to the same switch
- **Direct Connectivity:** No router needed for local communication
- **Broadcast Domain:** All devices hear broadcast traffic

### LAN Architecture

```
[Computer 1] --|
[Computer 2] --|-- [Switch] -- [Router] -- Internet
[Computer 3] --|
[Printer]    --|
```

### IP Addressing in LANs

LANs use **private IP addresses** from reserved ranges:

- **Class A:** 10.0.0.0 to 10.255.255.255
- **Class B:** 172.16.0.0 to 172.31.255.255
- **Class C:** 192.168.0.0 to 192.168.255.255

### Example LAN

A typical office LAN might be `192.168.1.0/24`, where:
- `192.168.1.0` = Network address
- `192.168.1.1` = Router (gateway)
- `192.168.1.2-254` = Usable host addresses
- `192.168.1.255` = Broadcast address

## Network Types

### Wide Area Network (WAN)
- Covers large geographic areas (cities, countries, global)
- Lower speeds than LANs
- Uses public IP addresses
- Connected via ISPs and telecommunications infrastructure
- **Example:** The Internet

### Metropolitan Area Network (MAN)
- Larger than LAN, smaller than WAN
- Covers a city or large campus
- **Example:** University network spanning multiple buildings

### Virtual Private Network (VPN)
- Secure tunnel over public networks
- Encrypts traffic between endpoints
- Allows remote access to private networks
- **Security Benefit:** Hides IP address and encrypts data from ISP

## Network Switching

### How Switches Work

1. **Learning:** Switch learns MAC addresses of devices on each port
2. **Forwarding:** When frame arrives, switch looks up destination MAC and forwards to appropriate port
3. **Flooding:** If destination MAC unknown, frame sent to all ports except incoming port

### VLAN (Virtual LAN)

VLANs logically segment a physical switch into multiple virtual networks:

```
Physical Switch with 3 VLANs:
├── VLAN 10 (Accounting): 192.168.10.0/24
├── VLAN 20 (Sales): 192.168.20.0/24
└── VLAN 30 (IT): 192.168.30.0/24
```

**Benefit:** Isolates traffic even on the same physical switch

## Routing Fundamentals

### What is Routing?

Routing is the process of selecting paths for traffic across networks. Routers use routing tables to make forwarding decisions.

### Routing Decision Process

```
1. Packet arrives at router interface
2. Router examines destination IP
3. Router consults routing table
4. Router forwards to appropriate next hop
5. Router repeats process at each hop until destination reached
```

### Default Gateway

The **default gateway** is the router's IP address used when sending traffic outside your local network.

- **Example:** Your computer has gateway `192.168.1.1`
- **When Used:** When destination IP is not on the same subnet as your computer

## Common Network Services

### DHCP (Dynamic Host Configuration Protocol)
- Automatically assigns IP addresses to devices
- Eliminates manual IP configuration
- Typically runs on router or dedicated server

### DNS (Domain Name System)
- Translates domain names to IP addresses
- **Example:** `google.com` → `142.250.185.46`
- Uses port 53 (UDP/TCP)

### ARP (Address Resolution Protocol)
- Maps IP addresses to MAC addresses
- **Purpose:** Finds MAC address when you only know IP address
- Works only on local network (LAN)

## Security Implications

### Network Reconnaissance
- Attackers map networks to find targets
- Tools: Ping, traceroute, nmap
- Defense: Network segmentation, firewall rules

### Man-in-the-Middle (MITM)
- Attacker intercepts communication between two parties
- Common on unsecured networks
- Defense: Encryption (HTTPS, VPN)

### ARP Spoofing
- Attacker sends fake ARP replies
- Can redirect traffic to attacker's machine
- Defense: Static ARP entries, ARP monitoring

---

## Key Takeaways

✅ OSI model provides framework for understanding network communication
✅ LANs connect local devices at high speed with private IPs
✅ Switches forward frames within LANs using MAC addresses
✅ Routers connect networks and forward packets using IP addresses
✅ Understanding network fundamentals is critical for security assessment

---

**Next:** Learn about protocols with [TCP/IP & UDP Ports](./01-tcp-ip-udp-ports.md)
