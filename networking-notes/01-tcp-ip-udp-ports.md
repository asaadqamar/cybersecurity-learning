# TCP/IP & UDP Ports

## Overview

**TCP** and **UDP** are Layer 4 (Transport) protocols that enable end-to-end communication. They differ fundamentally in reliability, speed, and use cases.

## TCP (Transmission Control Protocol)

### Characteristics

| Feature | TCP |
|---------|-----|
| **Connection** | Connection-oriented (handshake required) |
| **Reliability** | Guaranteed delivery, error checking |
| **Ordering** | Maintains packet order |
| **Speed** | Slower (due to reliability checks) |
| **Overhead** | Higher (more headers and verification) |
| **Flow Control** | Yes, prevents overwhelming receiver |

### TCP Three-Way Handshake

Before any data transmission, TCP establishes connection:

```
Client                              Server
  |                                   |
  |-------- SYN (SEQ=x) ----------->  |
  |                                   |
  |  <----- SYN-ACK (SEQ=y, ACK=x+1)  |
  |                                   |
  |-------- ACK (ACK=y+1) ----------> |
  |                                   |
  |========= Connection Established ===|
```

**Step 1:** Client sends SYN ("synchronize")
**Step 2:** Server responds with SYN-ACK
**Step 3:** Client sends ACK (acknowledgment)

### Connection Termination

TCP gracefully closes connections:

```
Client: FIN ------>  Server
Client: <------ FIN+ACK
Client: ACK ------> Server
```

### Use Cases

- **HTTP/HTTPS** — Web browsing (reliability critical)
- **SMTP/POP3** — Email (accurate delivery required)
- **SSH** — Remote administration (secure, ordered delivery)
- **FTP** — File transfer (no corruption allowed)
- **Telnet** — Remote access (ordered delivery important)

### TCP Flags

TCP uses flags to control communication:

| Flag | Meaning | Purpose |
|------|---------|---------|
| **SYN** | Synchronize | Initialize connection |
| **ACK** | Acknowledgment | Confirm receipt of data |
| **FIN** | Finish | Terminate connection |
| **RST** | Reset | Abruptly close connection |
| **PSH** | Push | Send buffered data immediately |
| **URG** | Urgent | Data has high priority |

## UDP (User Datagram Protocol)

### Characteristics

| Feature | UDP |
|---------|-----|
| **Connection** | Connectionless (no handshake) |
| **Reliability** | No guarantee, best effort |
| **Ordering** | No guaranteed order |
| **Speed** | Very fast (minimal overhead) |
| **Overhead** | Low (8-byte header) |
| **Flow Control** | No, can overwhelm receiver |

### UDP Operation

```
Client sends datagram ------->  Server receives or loses it
(No connection, no acknowledgment, no guarantee)
```

### Use Cases

- **DNS** — Domain name resolution (fast is more important than perfect)
- **VoIP** — Voice calls (occasional lost packet acceptable)
- **Online Gaming** — Real-time multiplayer (speed critical, perfect delivery not essential)
- **Video Streaming** — Netflix, YouTube (buffering handles occasional losses)
- **NTP** — Network time synchronization (low overhead for frequent queries)
- **SNMP** — Network monitoring (lightweight protocol)

## Port Concepts

### What is a Port?

A **port** is a logical endpoint for network communication. Ports allow a single computer to run multiple network services simultaneously.

- **Port Range:** 0 to 65,535 (16-bit number)
- **Format:** IP:Port (e.g., `192.168.1.100:80`)

### Port Categories

#### Well-Known Ports (0-1023)
Assigned by Internet Assigned Numbers Authority (IANA) for specific services. Require administrative privileges to bind.

| Port | Protocol | Service |
|------|----------|---------|
| 20 | TCP | FTP Data |
| 21 | TCP | FTP Control |
| 22 | TCP | SSH |
| 23 | TCP | Telnet |
| 25 | TCP | SMTP (Email Sending) |
| 53 | TCP/UDP | DNS |
| 80 | TCP | HTTP (Web) |
| 110 | TCP | POP3 (Email) |
| 143 | TCP | IMAP (Email) |
| 443 | TCP | HTTPS (Secure Web) |
| 3306 | TCP | MySQL Database |
| 5432 | TCP | PostgreSQL Database |

#### Registered Ports (1024-49151)
Available for application use. Often reserved for specific services but can be used by any application with proper permissions.

| Port | Service |
|------|---------|
| 3389 | Remote Desktop Protocol (RDP) |
| 5900 | Virtual Network Computing (VNC) |
| 8080 | Alternative HTTP |
| 8443 | Alternative HTTPS |
| 27017 | MongoDB |

#### Dynamic/Private Ports (49152-65535)
Available for ephemeral use. Operating system assigns these to client applications temporarily.

### Socket

A **socket** is the combination of IP address and port that uniquely identifies an endpoint.

```
Socket = IP Address + Port
Example: 192.168.1.100:443

Complete connection (two sockets):
Client:  192.168.1.100:54321  <--->  Server: 8.8.8.8:443
```

## TCP vs UDP Comparison

| Aspect | TCP | UDP |
|--------|-----|-----|
| **Setup** | Requires handshake | Immediate |
| **Reliability** | Guaranteed | Best-effort |
| **Ordering** | In-order delivery | Out-of-order possible |
| **Speed** | Slower | Faster |
| **Data Checking** | Comprehensive checksums | Basic checksums |
| **Broadcasting** | No | Yes |
| **When to Use** | Accuracy critical | Speed critical |

## Port Scanning

### Why Scan Ports?

- **Reconnaissance:** Identify running services
- **Vulnerability Assessment:** Discover exposed services
- **Security Audit:** Verify only needed ports are open

### Port States

| State | Meaning |
|-------|---------|
| **Open** | Service is listening, accepted connection |
| **Closed** | Port responded but no service listening |
| **Filtered** | Firewall blocked the probe |
| **Stealth** | No response received |

### Common Port Scanning Tools

- **nmap** — Most powerful, flexible scanner
- **netstat** — View local listening ports
- **ss** — Modern Linux connection utility
- **telnet** — Basic TCP connectivity test

### Example nmap Scan

```bash
nmap -p 80,443,22 192.168.1.100
```

## Network Protocol Stack

### How TCP/UDP Fit

```
Layer 7: Application (HTTP, SMTP, DNS, SSH)
         |
Layer 4: Transport (TCP, UDP)  <-- WE ARE HERE
         |
Layer 3: Network (IP)
         |
Layer 2: Data Link (Ethernet, WiFi)
         |
Layer 1: Physical (Cables, Radio)
```

## Security Implications

### TCP Vulnerabilities
- **Port Scanning:** Easy to enumerate services
- **SYN Floods:** Attacker overwhelms server with SYN packets (DoS)
- **Connection Hijacking:** Session takeover after handshake
- **Mitigation:** Firewalls, IDS/IPS, SYN cookies

### UDP Vulnerabilities
- **UDP Floods:** Overwhelming target with packets (DoS)
- **DNS Spoofing:** Fake DNS responses (uses UDP 53)
- **DDoS Amplification:** Attackers use public UDP services to amplify attacks
- **Mitigation:** Rate limiting, DNS DNSSEC, ingress filtering

### Port-Based Defense

- **Firewall Rules:** Block unnecessary ports
- **Non-Standard Ports:** Move services to non-standard ports (security through obscurity—not sufficient alone)
- **VPN/Bastion Hosts:** Require authentication before accessing ports
- **Network Segmentation:** Restrict which devices can reach which ports

---

## Key Takeaways

✅ TCP provides reliable, ordered delivery—use for critical data
✅ UDP provides fast, connectionless delivery—use for speed-critical applications
✅ Ports enable multiple services on single IP address
✅ Understanding port services is essential for network security
✅ Port scanning is a fundamental reconnaissance technique

---

**Previous:** [Networking Basics](./00-networking-basics.md)
