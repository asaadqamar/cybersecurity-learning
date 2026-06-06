# Nmap Cheatsheet

## Overview

**Nmap** (Network Mapper) is the industry-standard network reconnaissance tool used by security professionals for discovering hosts, services, and vulnerabilities on networks.

## Installation

### Windows
```powershell
# Using Chocolatey
choco install nmap

# Or download from: https://nmap.org/download.html
```

### Linux (Ubuntu/Debian)
```bash
sudo apt-get install nmap
```

### macOS
```bash
brew install nmap
```

### Verify Installation
```bash
nmap --version
```

## Basic Syntax

```bash
nmap [options] [target]
```

### Target Specification

```bash
nmap 192.168.1.1              # Single IP
nmap 192.168.1.0/24           # Entire subnet (CIDR notation)
nmap 192.168.1.1-254          # Range of IPs
nmap example.com              # Domain name
nmap -iL targets.txt          # From file (one per line)
nmap --exclude 192.168.1.50   # Exclude specific host
```

## Common Scan Types

### Ping Sweep (Host Discovery)
```bash
# Discover active hosts on subnet
nmap -sn 192.168.1.0/24
# -sn: Ping scan only (no port scanning)
```

### TCP Connect Scan (-sT)
```bash
nmap -sT 192.168.1.100
# Completes full TCP handshake
# Slower, but reliable and logs connections
# Default scan type
```

### TCP SYN Scan (-sS)
```bash
nmap -sS 192.168.1.100
# Sends SYN, waits for SYN-ACK, sends RST
# Faster than connect scan
# Stealthier (doesn't complete connection)
# Requires root/admin privileges
# Most common scan type for professionals
```

### UDP Scan (-sU)
```bash
nmap -sU 192.168.1.100
# Scans UDP ports (DNS, NTP, SNMP, etc.)
# Slower than TCP scans
# Can miss filtered ports
```

### ACK Scan (-sA)
```bash
nmap -sA 192.168.1.100
# Sends ACK packets
# Used to map firewall rules
# Doesn't determine if port is open
# Good for firewall analysis
```

### FIN/NULL/Xmas Scans (-sF, -sN, -sX)
```bash
nmap -sF 192.168.1.100   # FIN scan
nmap -sN 192.168.1.100   # NULL scan
nmap -sX 192.168.1.100   # Xmas scan
# Stealthy scans, may not work through firewalls
# Some systems don't follow RFC properly
```

### Idle Scan (-sI)
```bash
nmap -sI zombie_host 192.168.1.100
# Highly stealthy, uses zombie host as proxy
# Advanced technique
```

## Port Specification

### Port Selection

```bash
nmap -p 80 192.168.1.100              # Single port
nmap -p 22,80,443 192.168.1.100       # Multiple ports
nmap -p 80-443 192.168.1.100          # Port range
nmap -p 1-10000 192.168.1.100         # Range of 1000s
nmap -p- 192.168.1.100                # All 65535 ports (slow!)
nmap -p U:53,U:123 192.168.1.100      # UDP specific ports
nmap -p T:80,T:443 192.168.1.100      # TCP specific ports
```

### Service-Based Selection

```bash
nmap --top-ports 1000 192.168.1.100   # Top 1000 most common ports
nmap --top-ports 100 192.168.1.100    # Top 100 ports
```

### Port Ranges by Common Services

```bash
nmap -p 1-1024 192.168.1.100          # Well-known ports
nmap -p 1024-49151 192.168.1.100      # Registered ports
```

## Service Detection & Versions

### Service Version Detection (-sV)
```bash
nmap -sV 192.168.1.100
# Connects to open ports
# Queries for service/version information
# More time-consuming but provides useful info
```

### Operating System Detection (-O)
```bash
nmap -O 192.168.1.100
# Attempts to determine OS type
# Requires at least one open and one closed port
# Requires root/admin privileges
```

### Aggressive Scan (-A)
```bash
nmap -A 192.168.1.100
# Combines: OS detection, version detection, script scanning, traceroute
# Equivalent to: -sV -O -sC --traceroute
# More detectable, noisier
```

### Default Scripts (-sC)
```bash
nmap -sC 192.168.1.100
# Runs default set of NSE scripts
# Discovers vulnerabilities, banners, etc.
# Good balance of information and speed
```

## Output Options

### Standard Output
```bash
nmap 192.168.1.100                # Console output
nmap 192.168.1.100 > results.txt  # Redirect to file
```

### Different Output Formats

```bash
nmap -oN results.txt 192.168.1.100     # Normal format (human readable)
nmap -oX results.xml 192.168.1.100     # XML format
nmap -oG results.grep 192.168.1.100    # Greppable format (for parsing)
nmap -oA results 192.168.1.100         # All formats (generates .nmap, .xml, .gnmap)
```

### Verbose Output

```bash
nmap -v 192.168.1.100                  # Verbose (one level)
nmap -vv 192.168.1.100                 # Very verbose (two levels)
nmap -v0 192.168.1.100                 # Less verbose
```

### Debugging Output

```bash
nmap -d 192.168.1.100                  # Debug level 1
nmap -dd 192.168.1.100                 # Debug level 2 (very detailed)
```

## Performance & Timing

### Timing Templates

```bash
nmap -T0 192.168.1.100    # Paranoid (extremely slow, stealthy)
nmap -T1 192.168.1.100    # Sneaky (slow, stealthy)
nmap -T2 192.168.1.100    # Polite (normal speed, courteous)
nmap -T3 192.168.1.100    # Normal (default)
nmap -T4 192.168.1.100    # Aggressive (fast)
nmap -T5 192.168.1.100    # Insane (very fast, may lose accuracy)
```

### Manual Performance Tuning

```bash
nmap --min-rate 1000 192.168.1.100         # Minimum packets/sec
nmap --max-rate 5000 192.168.1.100         # Maximum packets/sec
nmap --min-parallelism 10 192.168.1.100    # Min parallel probes
nmap --max-parallelism 50 192.168.1.100    # Max parallel probes
nmap -P0 192.168.1.100                     # Skip ping (assume host up)
```

## Host Discovery Options (-P options)

```bash
nmap -PS 192.168.1.100    # TCP SYN ping
nmap -PA 192.168.1.100    # TCP ACK ping
nmap -PU 192.168.1.100    # UDP ping
nmap -PM 192.168.1.100    # ICMP timestamp ping
nmap -PP 192.168.1.100    # ICMP netmask ping
nmap -PE 192.168.1.100    # ICMP echo ping
nmap -Pn 192.168.1.100    # No ping (assume host up)
```

## Real-World Examples

### Basic Network Scan

```bash
nmap 192.168.1.0/24
# Discovers active hosts and open ports on subnet
# Uses default TCP connect scan on common ports
```

### Comprehensive Host Assessment

```bash
nmap -sV -sC -O -p- 192.168.1.100
# -sV: Service version detection
# -sC: Default scripts
# -O: OS detection
# -p-: All ports
# Result: Complete picture of target system
```

### Quick Service Discovery

```bash
nmap -sV --top-ports 1000 192.168.1.100
# Identifies services on top 1000 ports
# Faster than full scan
```

### Stealthy Reconnaissance

```bash
nmap -sS -T1 -p- 192.168.1.100
# -sS: TCP SYN scan (stealthy)
# -T1: Slow timing (avoid detection)
# -p-: All ports
# Slower but more evasive
```

### Firewall Mapping

```bash
nmap -sA -p- 192.168.1.100
# ACK scan to identify filtered ports
# Shows firewall/IDS filtering rules
```

### Subnet Sweep (Find Active Hosts)

```bash
nmap -sn 192.168.1.0/24 -oG hosts.txt
# -sn: Ping scan only
# -oG: Greppable output
# Result: List of active hosts on subnet
```

### DNS Reverse Lookup

```bash
nmap -sL 192.168.1.0/24
# Lists IPs and reverse DNS names
# No actual scanning
# Good for reconnaissance
```

### Save Results for Later Analysis

```bash
nmap -A -p- 192.168.1.100 -oA results
# Creates: results.nmap, results.xml, results.gnmap
# Can reprocess XML with other tools
```

## NSE (Nmap Scripting Engine)

### Running Specific Scripts

```bash
nmap --script http-title 192.168.1.100
# Retrieves HTTP page titles

nmap --script ssl-enum-ciphers -p 443 192.168.1.100
# Enumerates SSL/TLS ciphers

nmap --script smb-os-discovery 192.168.1.100
# Discovers Windows OS details
```

### Script Categories

```bash
nmap --script vuln 192.168.1.100       # Vulnerability detection
nmap --script default 192.168.1.100    # Default scripts (safer)
nmap --script discovery 192.168.1.100  # Service discovery
nmap --script all 192.168.1.100        # All scripts (slow!)
```

### Common Useful Scripts

```bash
--script http-title                    # HTTP page titles
--script ssl-cert                      # SSL certificate info
--script smb-enum-shares              # Windows shares
--script ssh-hostkey                  # SSH host keys
--script dns-brute                     # DNS enumeration
--script whois                         # WHOIS info
--script ftp-anon                      # FTP anonymous access
```

## Common Port Reference

| Port | Service |
|------|---------|
| 21 | FTP |
| 22 | SSH |
| 23 | Telnet |
| 25 | SMTP |
| 53 | DNS |
| 80 | HTTP |
| 110 | POP3 |
| 143 | IMAP |
| 443 | HTTPS |
| 445 | SMB/NetBIOS |
| 3306 | MySQL |
| 3389 | RDP |
| 5432 | PostgreSQL |
| 5900 | VNC |
| 8080 | Alternative HTTP |
| 8443 | Alternative HTTPS |

## Tips & Best Practices

### Ethical Use
- **Only scan systems you have permission to test**
- Port scanning without authorization is illegal in many jurisdictions
- Always get written permission before testing

### Efficiency
- Use `--top-ports` for quick scans
- Use timing templates appropriate for your needs
- Start with -p- only after confirming target is up

### Accuracy
- Combine multiple scan types for confirmation
- Use version detection (-sV) for reliable service identification
- Don't rely solely on default port numbers

### Evasion (with permission during authorized tests)
- Use slower timing (-T1, -T2)
- Use fragmentation (--mtu)
- Use decoys (--decoy)
- Spoof source IP if network allows

---

## Key Takeaways

✅ Nmap is essential for network reconnaissance
✅ SYN scan (-sS) is most popular for advanced scanning
✅ Service detection (-sV) reveals running applications
✅ Timing templates adjust speed vs accuracy
✅ NSE scripts automate vulnerability detection
✅ Always obtain proper authorization before scanning

---

## Common Command Combinations

```bash
# Quick reconnaissance
nmap -sV --top-ports 100 192.168.1.0/24

# Full vulnerability assessment
nmap -sV -sC -O -p- 192.168.1.100

# Stealthy assessment
nmap -sS -T1 -p- 192.168.1.100

# Find web servers
nmap -p 80,443 -sV 192.168.1.0/24 | grep open

# Identify Windows systems
nmap -O 192.168.1.0/24 | grep "OS details: Windows"
```
