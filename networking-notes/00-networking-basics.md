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


## OSI Model (Open System Interconnection)

The OSI Model is a critical model that explains how network devices send, receive, and interpret data.

It has **7 layers**, from Layer 7 to Layer 1. As data moves down the layers, each layer adds its own information to the data. This process is called **encapsulation**.

### Physical Layer

- Refers to the physical components used in networking.
- Examples: Ethernet cables, fiber cables, switches, hubs.
- Sends data as binary (0s and 1s).

### Data Link Layer

- Focuses on physical addressing using **MAC addresses**.
- Receives data from the Network Layer.
- Adds source and destination MAC addresses.
- Prepares data in a suitable format for transmission over the physical medium.

### Network Layer

- Responsible for logical addressing and routing.
- Assigns and uses **IP addresses**.
- Finds the best path to the destination.
- Handles packet fragmentation when required.
- Routers operate at **OSI Layer 3**.

### Transport Layer

- Responsible for end-to-end communication between applications.
- Uses **TCP** and **UDP** protocols.
- Uses **port numbers** to identify services and applications.

#### TCP (Transmission Control Protocol)

- Reliable and connection-oriented.
- Guarantees accurate and ordered delivery of data.
- Used in web browsing, file downloads, and emails.

#### UDP (User Datagram Protocol)

- Fast and connectionless.
- Does not guarantee delivery of data.
- Commonly used in gaming, streaming, and VoIP.

### Session Layer

- Establishes, manages, and terminates connections between devices.
- Each active connection is called a **session**.
- Keeps communication organized between applications.

### Presentation Layer

- Makes data readable between different systems.
- Acts as a translator and formatter.
- Handles encryption and decryption of data.

### Application Layer

- The layer closest to the end user.
- Provides network services to applications.
- Examples: HTTP, HTTPS, DNS, FTP, SMTP.
- Applications such as browsers and FileZilla operate here.

## Packets & Frames

### Packet

- A packet is a small piece of data at the **Network Layer (Layer 3)**.
- Contains a header and payload.
- Uses IP addresses to identify source and destination devices.

### Frame

- When a packet moves to the **Data Link Layer (Layer 2)**, it is encapsulated into a frame.
- A frame contains source and destination **MAC addresses**.
- Frames are used for communication on the local network.

## TCP/IP (Three-Way Handshake)

- TCP is used to establish a reliable connection between devices.
- It ensures both client and server are reachable before data transfer begins.

### Three-Way Handshake

1. **SYN** → Client sends a synchronization request.
2. **SYN/ACK** → Server acknowledges the request.
3. **ACK** → Client confirms the connection.

After this, data transfer can begin.

### Other TCP Flags

- **SYN** → Start a connection.
- **ACK** → Acknowledge received data.
- **PSH** → Push data immediately.
- **FIN** → Gracefully close a connection.
- **RST** → Abort a connection due to an error.

## UDP/IP

- UDP is a **stateless** and **connectionless** protocol.
- No handshake is required before communication.
- Faster than TCP but does not guarantee delivery.
- Commonly used in gaming, streaming, VoIP, and DNS.

A UDP packet typically contains:

- Source Port
- Destination Port
- Length
- Checksum
- Data

## Ports 101

- Ports help identify which service or application should receive data.
- Each service usually listens on a specific port.

### Common Ports

| Port | Protocol |
|--------|----------|
| 21 | FTP |
| 22 | SSH |
| 23 | Telnet |
| 25 | SMTP |
| 53 | DNS |
| 80 | HTTP |
| 110 | POP3 |
| 143 | IMAP |
| 443 | HTTPS |

### Why Ports Matter in Cybersecurity

- Port scanning helps identify running services.
- Open ports can expose attack surfaces.
- Firewalls use ports to allow or block traffic.

## Expanding Your Network
 
### Intro to Port Forwarding
- An essential component in networking
- Without it web servers are only accessible to devices within the same network
- It is configured at the router of the network

### Firewall
- A device within a network responsible for deciding what traffic enters or exits
- Two categories of firewall: stateful and stateless
- Stateful: uses connection information to form and track connections, handshakes can still fail later if the host connection is bad
- Stateless: only determines if individual packets are allowed or not

### VPN Basics
- Virtual Private Network
- Allows devices to communicate over separate networks by creating a dedicated path called a tunnel
- VPN technologies: PPP, PPTP, IPsec
- PPP: used by PPTP for authentication and encryption of data, not capable of sending data over the Internet by itself
- PPTP: Point-to-Point Tunneling Protocol, allows data to travel over the Internet
- IPsec: encrypts data using the existing IP framework

### Routers
- Used to connect networks and pass data between them
- Operate on OSI model Layer 3
- Often provide interactive interfaces to configure rules such as port forwarding and firewall settings

### Switch
- A dedicated networking device, provides means to connect multiple devices
- Two types of switch: Layer 2 and Layer 3
- Layer 2: forwards frames to their MAC addresses within the network
- Layer 3: more sophisticated, can perform tasks like routers, through VLANs allow devices to split up while using the same physical network and be treated differently, so Layer 3 switches can send data between those networks

## DNS (Domain Name System)
It provides us a simple way to communicate with devices on the internet without remembering complex numbers.

### Domain Hierarchy
- In hierarchy we have root level, top-level, and second-level domain

### TLD (Top Level Domain)
- TLD: It's the most right-hand part of a domain, e.g: '.com', '.edu'
- Two types of TLD are gTLD and ccTLD
- gTLD: Generic TLDs are used to tell the purpose of a domain e.g: '.com' for commercial, '.edu' for education
- ccTLD: Country Code TLDs are used for geographical purposes e.g: '.pk' and '.uk'

### Second Level Domain
- It's usually the organisation/business name e.g: 'codebysavvy.com' the 'codebysavvy' is the second-level domain
- It can have up to 63 characters including a-z and 0-9, can't have hyphens at the start or end

### Subdomains
- Sits on the left side of the second-level domain
- 'admin.codebysavvy.com' follows the same naming rules as second-level domains
- Can be up to 253 characters long

### DNS Record Types
- A: Resolves to IPv4 address
- AAAA: Resolves to IPv6 address
- CNAME: Resolves to another domain name
- MX: Resolves to mail server
- TXT: Can store anything like domain ownership verification, SPF records, or backup email server information

### How DNS Request Works
- You make a request, computer looks up in local cache
- If not found request is made to recursive DNS server, usually provided by your ISP, can setup your own
- Again if not found request is made to root DNS server, which redirects to the TLD server
- TLD server holds records of where to find the authoritative server, also known as nameserver
- Depending on DNS record type data is sent back to recursive DNS server, and a copy of the response is saved in cache

## HTTP (HyperText Transfer Protocol) in detail
Its a set of rules used for communication with web servers for transmission of web page data

## HTTPS (HyperText Transfer Protocol Secure)
- Secure version of HTTP
- It encrypts data, which stops people from seeing what you're sending or receiving
- Gives you assurance you're talking to the correct server

## Request and Response

### URL (Uniform Resource Locator)
- Its an instruction on how to access data on the internet
- URL has many parts e.g: https://tryhackme.com/blog?id=55#tasks
- URL parts are scheme (https), host (tryhackme.com), path (/blog), query string (id=55)
- Other parts of URL are port and fragment
- To send more sophisticated requests we have headers

### HTTP Methods
Its a way users show their intended action when making a request, HTTP methods are
- GET: to fetch data
- POST: to create new data
- PUT: to update data
- DELETE: to delete data

### HTTP Status Codes
Status code of a request tells the outcome of the request, status codes have 5 ranges
- 100 to 199: informational responses, not very common
- 200 to 299: request is successful
- 300 to 399: being redirected to another resource
- 400 to 499: error on client side
- 500 to 599: reserved for server side errors

Most common status codes are:
- 200: OK
- 201: Created
- 301: Moved Permanently
- 302: Found
- 400: Bad Request
- 401: Unauthorized
- 403: Forbidden
- 405: Method Not Allowed
- 404: Page Not Found
- 500: Internal Server Error
- 503: Service Unavailable

### Headers
Headers are additional bits of data you can send to web servers, common request headers are
- Host (name of website)
- User-Agent (browser details)
- Content-Length
- Accept-Encoding
- Cookie

Common response headers are
- Set-Cookie
- Cache-Control
- Content-Type
- Content-Encoding

### Cookie
Cookies are used to remind web servers who you're, mostly used to save auth details in token form not easily human readable
- As HTTP is stateless, cookies are used to store state information between requests

## How Website Works
Client makes a request and server sends a response. A website has majorly two components:
- Front-end (client side)
- Backend (server side)

### HTML Injection
Suppose a website has a form that updates post comments and the input fields allow HTML/JS, the user can enter a link to malware and store it in the database, and every user of that site can click on that link which can be dangerous.

# Inside a Computer

## Physical Parts of Computer
- Motherboard: Every component plugs into or connects through the motherboard.
- CPU (Central Processing Unit): Often called processor, executes instructions and performs calculations.
- RAM (Random Access Memory): Stores data temporarily.
- Storage (SSD/HDD): Stores data long-term.
- Network Adapter: Enables the computer to communicate with other computers and networks.
- Power Supply Unit: Supplies electricity to all components.
- Graphics Card: Primarily handles processing and outputting visual information to displays.
- I/O Devices: Used to send and receive data from the computer.

## How Computer Starts

### Press the Power Button
Once you press the power button, it sends a signal to the PSU to allow power flow.

### Firmware Start
Computer systems contain firmware that allows components to start up. The central system that manages it is called Unified Extensible Firmware Interface (UEFI). We'll often see BIOS mentioned instead of UEFI, BIOS serves the same purpose.

### Power On Self Test (POST)
One of the routines that UEFI loads is Power On Self Test.
It tests if required components are present, configured correctly, and functioning.

### Select Boot Device
UEFI holds a boot order list, it prioritizes which devices to look at first to boot the OS.

### Initiate Bootloader
On the selected device the bootloader is initiated, it transfers the OS kernel from storage into RAM. Once loaded, the bootloader hands control over to the operating system.

## Computer Types
Most common computer types are laptops, desktops, workstations, servers, smartphones, tablets, IoT devices and embedded computers.

### IoT & Embedded Computers
- Both can be small and single purpose
- The difference is connectivity
- IoT devices connect to networks to report data and receive commands
- Embedded computers might not connect to anything, they do their job inside a machine, often for years without users knowing they exist.

# Virtualization
- Imagine if every website required its own server, how costly and inefficient that would be
- To solve this exact problem virtualization was introduced

## Hypervisor
- It's the virtualization layer
- A software that acts as a referee to allow multiple VMs to behave independently
- Lab machines are virtual computers created using a hypervisor
- There are two types of implementation: Type 1 and Type 2

### Type 1
- Runs directly on physical hardware, ideal for servers and professional environments

### Type 2
- Runs within an operating system, easier to install, ideal for learning, testing and small setups

## Lab Machine (VM)
- It's a computer created by a hypervisor
- It behaves like a real machine

## Containers
- A lightweight, isolated environment that runs a single application and all the necessary components to support it
- Unlike VM they share the host OS kernel

# Cloud Computing Origin
- AWS simplified how servers are accessible to startups and developers
- Before AWS cloud, if you needed servers for a startup, either you bought them from Dell/HP or rented from a service provider, but you had to configure everything yourself like load balancers etc, and it could take months
- Amazon solved it by making servers accessible through APIs and if your traffic suddenly skyrockets you don't have to wait months to set up servers or rent from a provider, and when traffic goes back to normal you can scale down instead of letting servers sit idle
- Amazon basically automated the infrastructure

## Type of Cloud Deployments
- Public Cloud: Used by startups and global apps, easy to scale, affordable
- Private Cloud: Used by banks, healthcare and government who care more about security, compliance, control and customization
- Hybrid Cloud: Used by e-commerce and enterprises by keeping sensitive data private yet scaling publicly

## Cloud Service Models
- Infrastructure as a Service (IaaS): You rent virtual servers, storage and networking, manage your own OS and applications while the provider takes care of the hardware
- Platform as a Service (PaaS): You only focus on building, deploying and running your application, the rest is taken care of by the provider
- Software as a Service (SaaS): You use a complete application as a service through an app or browser e.g: email, Zoom You use a complete application as a service through an app or browser e.g: email, Zoom

## Basics of AWS
- EC2 (virtual computer/server): Represents a virtual machine in the cloud, has CPU and memory, can run applications
- Instance Types (t2, t3, m5): These describe how powerful the virtual computer is, you choose an instance based on your needs


