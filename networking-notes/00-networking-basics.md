# Networking Basics

## Overview

A network is simply a group of connected devices.

In networking, we mainly have two types of networks:

- **Public Network** – Free Wi-Fi, Public Library Wi-Fi, Airport Wi-Fi
- **Private Network** – Home network, Office network, School network

## The Internet

The Internet is a **giant network of networks**. It consists of many smaller networks connected together.

## Identification on a Network (IP & MAC Address)

### IP Address (Internet Protocol)

- Just like networks, IP addresses can be **public** or **private**.
- There are two versions: **IPv4** and **IPv6**.
- IPv4 consists of **4 octets** separated by dots.
- Each octet contains **8 bits (1 byte)**.
- Since an octet has 8 bits, its value can range from **0 to 255**.
- Example: `192.168.1.1`

### MAC Address (Media Access Control)

- A physical address assigned to a device by the manufacturer.
- Consists of **12 hexadecimal characters** in pairs separated by colons.
- Example: `12:D3:87:87:A7:F3`
- The first 6 characters identify the manufacturer.
- The last 6 characters identify the device.

### Ping

- A basic networking tool used to check connectivity between devices.
- Uses **ICMP (Internet Control Message Protocol)** packets.
- Helps verify whether a device is reachable and measures response time.

## Decimal & Binary Basics

- Decimal numbers are based on powers of **10**.
- Binary numbers are based on powers of **2**.

### Decimal to Binary

To convert **192** to binary:

- The nearest power of 2 is **128**.
- `192 - 128 = 64`
- `64 - 64 = 0`

So we place `1` on the powers of 2 we used and `0` on the rest:

```text
128 64 32 16 8 4 2 1
 1   1  0  0 0 0 0 0
```

Result:

```text
192 = 11000000
```

### Binary to Decimal

For `11000000`:

```text
(1 × 2⁷) + (1 × 2⁶)
= 128 + 64
= 192
```

So:

```text
11000000 = 192
```

## LAN (Local Area Network)
### Topologies of LAN
- Star Toplogy, Most commonly used (Rely on Stwich)
- Bus & Ring Topology

### Stwich
- Modren version of Hub
- Sends data only to the intended device using MAC addresses (HUb used to send data from one to all other devices)
- Has ports each connected to a device

### Subnetting
- It's splitting network to smaller piece.
- To improve acess control, traffic managment and security
- IP addresses are divided into Network and Host portions using a subnet mask

### ARP (Address Resolution Protocol)
- It is used to find the MAC address associated with an IP address on a local network
- It send two types of messages ARP Request & ARP Reply
- ARP Request Ask for identification 
- ARP Reply contains the MAC address of the requested IP address

### DHCP (Dynamic Host configration protocol)
- If an IP is not assigned to a device manually, - DHCP Discover packet is broadcast by a client to find available DHCP servers
- DHCP Offer packet provides the IP
- DHCP Request packet asks to use the offered IP address
- DHCP ACK packet confirms the IP assignment and completes the process