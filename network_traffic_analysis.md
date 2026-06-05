# Network Traffic: Sources and Flows

To practically analyze a corporate network, it is essential to understand the specific sources generating traffic and the directional flows of that traffic. 

---

## 1. Traffic Sources
Network devices in a LAN or WAN can be categorized into two primary types based on how they interact with traffic.

### Intermediary Sources
Devices that traffic primarily **passes through**. They generate significantly less traffic than endpoints, mostly limited to operational protocols.
* **Examples:** Firewalls, switches, web proxies, IDS/IPS, routers, access points, and ISP infrastructure.
* **Generated Traffic:** Routing (EIGRP, OSPF, BGP), Management (SNMP, PING), Logging (SYSLOG), and Infrastructure support (ARP, STP, DHCP).

### Endpoint Sources
Devices where traffic **originates and ends**. These devices consume the bulk of network bandwidth.
* **Examples:** Servers, hosts/workstations, IoT devices, printers, mobile devices, and cloud resources.

---

## 2. Traffic Flows
Network flows are determined by the services running within the network. They are typically grouped into two main directions.

### North-South (NS) Traffic
Traffic that enters or exits the LAN, crossing the network perimeter (firewall) to the WAN.
* **Key Characteristics:** Highly monitored. Relies heavily on firewall rules and logging for visibility.
* **Common Protocols:** HTTPS, DNS, SSH, VPN, SMTP, RDP. 
* **Streams:** Consists of ingress (inbound) and egress (outbound) traffic.

### East-West (EW) Traffic
Traffic that stays entirely within the corporate LAN (or extended cloud LAN).
* **Key Characteristics:** Often under-monitored compared to NS traffic, but critical for security. Attackers exploit EW flows to move laterally across a compromised network.

#### East-West Service Categories
| Category | Services & Protocols |
| :--- | :--- |
| **Directory & Identity** | Kerberos/LDAP (Active Directory), RADIUS/TACACS+ (Access Control), Internal Certificate Authorities. |
| **File & Print** | SMB/CIFS (Network drives), IPP/LPD (Network printing). |
| **Infrastructure** | DHCP, ARP broadcasts, Internal DNS, Routing protocol messages. |
| **App Communication** | SQL (Database connections), REST/gRPC (Microservices APIs). |
| **Backup & Replication** | File replication between datacenters, Database replication (MySQL binlog, etc.). |
| **Monitoring & Management** | SNMP (Health metrics), Syslog (Central logging), NetFlow/IPFIX (Telemetry). |

---

## 3. Flow Examples

### HTTPS (with TLS Inspection)
When a Next-Generation Firewall (NGFW) with a web proxy is used, HTTPS traffic involves two separate sessions:
1. **Host to Proxy:** The client requests a website, establishing a session with the proxy.
2. **Proxy to Web Server:** The proxy establishes a separate session with the actual web server.
*The proxy inspects the returning payload and, if safe, forwards it to the host. To the client, this appears as a single session.*

### External DNS
1. Host sends a query (Port 53) to the **Internal DNS server**.
2. The Internal DNS checks its cache. If no record is found, it acts on behalf of the host.
3. The query is forwarded through the router and firewall to an **External DNS server**.
4. The response follows the same path back to the Internal DNS, which finally hands it to the host.

### SMB with Kerberos (File Sharing)
When accessing a network share (e.g., `\\FILESERVER\MARKETING`), authentication happens before the SMB session opens:
1. **Authentication:** The host authenticates with the Domain Controller's Key Distribution Center (KDC) to receive a *Ticket Granting Ticket (TGT)*.
2. **Service Request:** The host uses the TGT to request a specific service ticket for the file server.
3. **Connection:** Using the new service ticket, the host establishes the SMB session and accesses the share.

# Network Visibility: Logs, Captures, and Statistics

To truly understand what is happening on a network, security professionals rely on three main pillars of visibility: system logs, full packet captures, and network flow statistics.

---

## 1. Network Logs
Logs are the first line of visibility, but they come with a major caveat: **there is no universal standard** for how logging is implemented. 

* **Vendor Variations:** Each vendor decides what to log. Most do not log full packets; they log specific fields (e.g., Source/Destination IPs, ports).
* **Format Examples:** Microsoft uses *Windows Event Logs*, while Linux might use *Syslog*, and web servers often use *Common Log Format (CLF)*.
* **Log Forwarding:** While log formats vary, protocols like **Syslog** and **SNMP** offer standardized ways to send these messages to central collectors (like a SIEM).

> **Note:** When basic logs don't provide enough context, analysts must correlate data, inspect full packets, or review network statistics.

---

## 2. Full Packet Capture (PCAP)
When deep inspection is required, capturing the entire packet (headers and payload) is necessary. There are two primary methods to achieve this:

### Network Taps vs. Port Mirroring

| Feature | Network Tap | Port Mirroring (e.g., SPAN) |
| :--- | :--- | :--- |
| **Type** | Physical hardware device | Software/Configuration based |
| **Operation** | Placed inline; copies electrical/light signals at the link layer. | Duplicates packets from one switch port to a monitoring port. |
| **Network Impact** | **Zero.** Causes no delay or performance reduction. | **Variable.** Can impact switch performance if traffic volume is heavily mirrored. |
| **Addressing** | Requires no MAC or IP address. | Configured on existing network interfaces. |
| **Environments** | Physical datacenters and wiring closets. | Physical switches, virtual switches (vSwitch), and Cloud (AWS VPC Traffic Mirroring). |


# Network Security and Traffic Analysis Guide

To practically analyze a corporate network, it is essential to understand the specific sources generating traffic, the directional flows of that traffic, and the tools used to capture and analyze it.

---

## 1. Network Traffic Sources

Network devices in a LAN or WAN can be categorized into two primary types based on how they interact with traffic.

### Intermediary Sources
Devices that traffic primarily **passes through**. They generate significantly less traffic than endpoints, mostly limited to operational protocols.
* **Examples:** Firewalls, switches, web proxies, IDS/IPS, routers, access points, and ISP infrastructure.
* **Generated Traffic:** Routing (EIGRP, OSPF, BGP), Management (SNMP, PING), Logging (SYSLOG), and Infrastructure support (ARP, STP, DHCP).

### Endpoint Sources
Devices where traffic **originates and ends**. These devices consume the bulk of network bandwidth.
* **Examples:** Servers, hosts/workstations, IoT devices, printers, mobile devices, and cloud resources.

---

## 2. Network Traffic Flows

Network flows are determined by the services running within the network. They are typically grouped into two main directions.

### North-South (NS) Traffic
Traffic that enters or exits the LAN, crossing the network perimeter (firewall) to the WAN.
* **Key Characteristics:** Highly monitored. Relies heavily on firewall rules and logging for visibility.
* **Common Protocols:** HTTPS, DNS, SSH, VPN, SMTP, RDP. 
* **Streams:** Consists of ingress (inbound) and egress (outbound) traffic.

### East-West (EW) Traffic
Traffic that stays entirely within the corporate LAN (or extended cloud LAN).
* **Key Characteristics:** Often under-monitored compared to NS traffic, but critical for security. Attackers exploit EW flows to move laterally across a compromised network.

#### East-West Service Categories
| Category | Services & Protocols |
| :--- | :--- |
| **Directory & Identity** | Kerberos/LDAP (Active Directory), RADIUS/TACACS+ (Access Control), Internal Certificate Authorities. |
| **File & Print** | SMB/CIFS (Network drives), IPP/LPD (Network printing). |
| **Infrastructure** | DHCP, ARP broadcasts, Internal DNS, Routing protocol messages. |
| **App Communication** | SQL (Database connections), REST/gRPC (Microservices APIs). |
| **Backup & Replication** | File replication between datacenters, Database replication (MySQL binlog, etc.). |
| **Monitoring & Management** | SNMP (Health metrics), Syslog (Central logging), NetFlow/IPFIX (Telemetry). |

### Flow Examples

#### HTTPS (with TLS Inspection)
When a Next-Generation Firewall (NGFW) with a web proxy is used, HTTPS traffic involves two separate sessions:
1. **Host to Proxy:** The client requests a website, establishing a session with the proxy.
2. **Proxy to Web Server:** The proxy establishes a separate session with the actual web server.
> *The proxy inspects the returning payload and, if safe, forwards it to the host. To the client, this appears as a single session.*

#### External DNS
1. Host sends a query (Port 53) to the **Internal DNS server**.
2. The Internal DNS checks its cache. If no record is found, it acts on behalf of the host.
3. The query is forwarded through the router and firewall to an **External DNS server**.
4. The response follows the same path back to the Internal DNS, which finally hands it to the host.

#### SMB with Kerberos (File Sharing)
When accessing a network share (e.g., `\\FILESERVER\MARKETING`), authentication happens before the SMB session opens:
1. **Authentication:** The host authenticates with the Domain Controller's Key Distribution Center (KDC) to receive a *Ticket Granting Ticket (TGT)*.
2. **Service Request:** The host uses the TGT to request a specific service ticket for the file server.
3. **Connection:** Using the new service ticket, the host establishes the SMB connection and accesses the share.

---

## 3. Network Visibility: Logs, Captures, and Statistics

Security professionals rely on three main pillars of visibility: system logs, full packet captures, and network flow statistics.

### Network Logs
Logs are the first line of visibility, but **there is no universal standard** for how logging is implemented. 
* **Vendor Variations:** Each vendor decides what to log. Most log specific fields (e.g., Source/Destination IPs, ports) rather than full packets.
* **Format Examples:** Microsoft uses *Windows Event Logs*, Linux uses *Syslog*, and web servers use *Common Log Format (CLF)*.
* **Log Forwarding:** Protocols like **Syslog** and **SNMP** offer standardized ways to send messages to central collectors (like a SIEM).

### Full Packet Capture (PCAP)
When deep inspection is required, capturing the entire packet is necessary. 

#### Network Taps vs. Port Mirroring

| Feature | Network Tap | Port Mirroring (e.g., SPAN) |
| :--- | :--- | :--- |
| **Type** | Physical hardware device | Software/Configuration based |
| **Operation** | Placed inline; copies electrical/light signals at the link layer. | Duplicates packets from one switch port to a monitoring port. |
| **Network Impact** | **Zero.** Causes no delay or performance reduction. | **Variable.** Can impact switch performance if traffic volume is heavy. |
| **Addressing** | Requires no MAC or IP address. | Configured on existing network interfaces. |
| **Environments** | Physical datacenters and wiring closets. | Physical switches, virtual switches (vSwitch), and Cloud (AWS VPC Traffic Mirroring). |

#### Configuration Example: Cisco SPAN

Switch(config)# monitor session 1 source interface fastEthernet0/1
Switch(config)# monitor session 1 destination interface fastEthernet0/2

## 5. Packet Dissection

Packet dissection (also known as protocol dissection) is the process of investigating packet details by decoding the available protocols and fields. Wireshark natively supports a massive list of protocols for dissection, and it even allows you to write custom dissection scripts if needed.

When you select a packet in Wireshark, the Packet Details pane breaks down the captured data. This breakdown closely mirrors the layers of the OSI model:

### Wireshark OSI Layer Breakdown

* **The Frame (Layer 1 - Physical):** Displays overarching details about the specific frame/packet you are looking at, including capture length, arrival time, and interface specifics.
* **Ethernet II (Layer 2 - Data Link):** Shows hardware addressing, specifically the **Source and Destination MAC Addresses**.
* **Internet Protocol (Layer 3 - Network):** Displays routing information, including the **Source and Destination IP Addresses** (IPv4 or IPv6).
* **Transport Protocol (Layer 4 - Transport):** Shows details of the transport mechanism used (TCP or UDP), including the **Source and Destination Ports**.
    * *Protocol Errors:* A continuation of Layer 4 that highlights specific segments (especially in TCP) that needed to be reassembled or experienced transmission issues.
* **Application Protocol (Layer 5+ - Application):** Displays details specific to the highest-level protocol being used (e.g., HTTP, FTP, SMB, DNS).
    * *Application Data:* An extension of the Application layer that displays the actual payload, contents, or application-specific data being transferred over the network.



## 6. Protocol Filters

Wireshark supports over 3,000 protocols. To make sense of the massive amount of captured data, analysts rely heavily on **Protocol Filters** to isolate specific traffic based on protocol fields.

### IP Filters (Network Layer)
IP filters help you narrow down traffic based on Network Layer information (OSI Layer 3). These are among the most commonly used filters and target data like IP addresses, versions, TTL (Time to Live), and flags.

| Filter Expression | Description |
| :--- | :--- |
| `ip` | Show all IP packets. |
| `ip.addr == 10.10.10.111` | Show all packets containing the IP address `10.10.10.111` (both source and destination). |
| `ip.addr == 10.10.10.0/24` | Show all packets containing IP addresses within the `10.10.10.0/24` subnet. |
| `ip.src == 10.10.10.111` | Show all packets originating *from* `10.10.10.111`. |
| `ip.dst == 10.10.10.111` | Show all packets sent *to* `10.10.10.111`. |

> **Note on Directionality:** The `ip.addr` filter is direction-agnostic; it will catch the IP whether it is the sender or the receiver. Use `ip.src` or `ip.dst` when you need to track traffic moving in a specific direction.

---

### TCP and UDP Filters (Transport Layer)
Transport Layer filters (OSI Layer 4) allow you to target specific ports, sequence numbers, acknowledgment numbers, window sizes, and protocol errors.

| Filter Expression | Description |
| :--- | :--- |
| `tcp.port == 80` | Show all TCP packets communicating over port 80. |
| `udp.port == 53` | Show all UDP packets communicating over port 53. |
| `tcp.srcport == 1234` | Show all TCP packets originating from port 1234. |
| `udp.srcport == 1234` | Show all UDP packets originating from port 1234. |
| `tcp.dstport == 80` | Show all TCP packets sent to port 80. |
| `udp.dstport == 5353` | Show all UDP packets sent to port 5353. |

---

### Application-Level Filters (HTTP & DNS)
Application Layer filters (OSI Layer 5+) allow you to search for protocol-specific payloads and linked data, such as HTTP methods or DNS query types.

#### HTTP Filters
| Filter Expression | Description |
| :--- | :--- |
| `http` | Show all HTTP packets. |
| `http.response.code == 200` | Show all packets with an HTTP "200 OK" response code. |
| `http.request.method == "GET"` | Show all HTTP GET requests. |
| `http.request.method == "POST"`| Show all HTTP POST requests. |

#### DNS Filters
| Filter Expression | Description |
| :--- | :--- |
| `dns` | Show all DNS packets. |
| `dns.flags.response == 0` | Show all DNS requests (queries). |
| `dns.flags.response == 1` | Show all DNS responses. |
| `dns.qry.type == 1` | Show all DNS "A" record queries. |

---

### Display Filter Expressions (The Built-In Cheat Sheet)
It is impossible to memorize all the display filters and accepted values for 3,000+ protocols. If you cannot recall a specific filter syntax, Wireshark includes a built-in builder guide. 

* **How to access it:** Navigate to `Analyse` --> `Display Filter Expression`.
* **What it does:** It displays all available protocol fields, the accepted value types (integers or strings), and any predefined values.

> **Pro-Tip: Coloring Rules** > Once you master display filters, you can use them to create custom visual alerts. Navigate to `View` --> `Coloring Rules` to assign custom colors to specific filter matches, making events of interest instantly pop out in your packet list!

---

## Wireshark: Traffic Analysis

**ARP (Address Resolution Protocol)** allows devices to identify themselves on a network by mapping IP addresses to physical MAC addresses. 

**ARP Poisoning / Spoofing (Man-In-The-Middle):** An attack involving the transmission of malicious ARP packets to the default gateway. The goal is to manipulate the "IP to MAC address table" to intercept and sniff traffic intended for a target host.

## ARP Protocol Characteristics
* Operates strictly on the **local network** (Layer 2).
* Bridges communication between MAC addresses and IP addresses.
* **Insecure:** Lacks built-in authentication mechanisms.
* **Non-routable:** Traffic does not pass through routers to other networks.
* **Common Patterns:** Requests, responses, announcements, and gratuitous packets.
* *Legitimate traffic:* A broadcast asking who owns an IP, followed by a direct reply from the IP owner providing their MAC address.

---

## Wireshark Analysis Filters
Use the following display filters to isolate specific ARP behaviors during an investigation:

| Investigation Goal | Wireshark Filter |
| :--- | :--- |
| **Global ARP Search** | `arp` |
| **ARP Requests** | `arp.opcode == 1` |
| **ARP Responses** | `arp.opcode == 2` |
| **Hunt: ARP Scanning** | `arp.dst.hw_mac==00:00:00:00:00:00` |
| **Hunt: ARP Poisoning** | `arp.duplicate-address-detected` or `arp.duplicate-address-frame` |
| **Hunt: ARP Flooding** | `((arp) && (arp.opcode == 1)) && (arp.src.hw_mac == [target-mac-address])` |

---

## Investigation Workflow & Anomaly Detection

### 1. Identifying Spoofing (Conflicts)
A primary indicator of ARP spoofing is **two different MAC addresses claiming the same IP address** (often the gateway IP). 
* Wireshark's Expert Info tab warns of duplicate IP addresses configured, highlighting the second occurrence of the conflict.
* *Action:* Identify which MAC is the legitimate owner and which is the malicious actor.

### 2. Identifying Flooding
An attacker may generate a massive volume of ARP requests to map the network or overwhelm the switch.
* *Indicator:* A single MAC address crafting multiple ARP requests against a wide range of IP addresses (e.g., `192.168.1.xxx`).

### 3. Confirming the MITM Attack
Attackers will route traffic through their machine. This is visible by analyzing higher-level protocol traffic (like HTTP) combined with Layer 2 data.
* *Action:* Add MAC address columns to the Wireshark packet list pane. 
* *Indicator:* IP addresses in the HTTP traffic appear normal, but the destination MAC address for the victim's traffic points to the attacker's MAC instead of the actual router/gateway.

---
# Identifying Hosts

When investigating a compromise or malware infection activity, a security analyst should know how to identify hosts on the network beyond simply matching IP and MAC addresses. One of the best methods is identifying hosts and users on the network to determine the investigation's starting point and identify systems associated with malicious traffic or activity.

Enterprise networks often use predefined naming conventions for users and hosts. While this makes inventory management easier, it has both advantages and disadvantages:

- **Advantage:** Hosts and users can be easily identified by name.
- **Disadvantage:** Adversaries can replicate naming patterns to blend into the environment.

Although there are various mitigation strategies, security analysts must still develop strong host and user identification skills.

## Protocols Used for Host and User Identification

- Dynamic Host Configuration Protocol (DHCP)
- NetBIOS Name Service (NBNS)
- Kerberos

---

# DHCP Analysis

**Dynamic Host Configuration Protocol (DHCP)** is responsible for automatically assigning IP addresses and network configuration parameters to devices.

## DHCP Investigation in a Nutshell

| Notes | Wireshark Filter |
|---------|----------------|
| Global Search | `dhcp or bootp` |

### Important DHCP Packet Types

- **DHCP Request** packets contain hostname information.
- **DHCP ACK** packets indicate accepted requests.
- **DHCP NAK** packets indicate denied requests.

Since only **Option 53 (Request Type)** has predefined static values, analysts should first filter by packet type before examining other options.

#### DHCP Request Types

| Type | Filter |
|--------|----------|
| Request | `dhcp.option.dhcp == 3` |
| ACK | `dhcp.option.dhcp == 5` |
| NAK | `dhcp.option.dhcp == 6` |

### DHCP Request Options

Useful fields for investigation:

- **Option 12:** Hostname
- **Option 50:** Requested IP Address
- **Option 51:** Requested IP Lease Time
- **Option 61:** Client MAC Address

**Filter Example:**

```wireshark
dhcp.option.hostname contains "keyword"
```

### DHCP ACK Options

Useful fields for investigation:

- **Option 15:** Domain Name
- **Option 51:** Assigned IP Lease Time

**Filter Example:**

```wireshark
dhcp.option.domain_name contains "keyword"
```

### DHCP NAK Options

Useful fields for investigation:

- **Option 56:** Message (rejection details/reason)

> Since rejection messages vary depending on the situation, analysts should read the message content instead of filtering on it. Understanding the reason for rejection helps build a more accurate hypothesis.

---

# NetBIOS (NBNS) Analysis

**NetBIOS Name Service (NBNS)** enables applications on different hosts to communicate across a network.

## NBNS Investigation in a Nutshell

| Notes | Wireshark Filter |
|---------|----------------|
| Global Search | `nbns` |

### Useful NBNS Fields

#### Queries

Query details may include:

- Hostname
- Time To Live (TTL)
- IP Address

**Filter Example:**

```wireshark
nbns.name contains "keyword"
```

---

# Kerberos Analysis

**Kerberos** is the default authentication protocol for Microsoft Windows domains. It authenticates service requests between systems over untrusted networks while securely verifying identity.

## Kerberos Investigation in a Nutshell

| Notes | Wireshark Filter |
|---------|----------------|
| Global Search | `kerberos` |

## User Account Search

### CNameString

Represents the username.

**Important Note:**

Some packets may contain hostnames in this field.

- Values ending with `$` represent **hostnames**.
- Values without `$` represent **user accounts**.

**Filter Examples:**

```wireshark
kerberos.CNameString contains "keyword"
```

```wireshark
kerberos.CNameString and !(kerberos.CNameString contains "$")
```

## Useful Kerberos Fields

### pvno

Protocol version.

```wireshark
kerberos.pvno == 5
```

### realm

Domain name associated with the generated ticket.

```wireshark
kerberos.realm contains ".org"
```

### sname

Service and domain name associated with the generated ticket.

### addresses

Contains:

- Client IP Address
- NetBIOS Name

> Note: Address information is only available in request packets.

### Service Name Search

```wireshark
kerberos.SNameString == "krbtg"
```

# Tunnelling Traffic: ICMP and DNS

Traffic tunnelling (also known as **port forwarding**) is a technique used to securely transfer data and resources between network segments and zones. It can facilitate communication between:

- Internet → Private Networks
- Private Networks → Internet

Tunnelling works by encapsulating data inside legitimate network protocols, allowing the traffic to appear normal while securely transporting hidden data to its destination.

Tunnelling provides:

- Anonymity
- Traffic security
- Data confidentiality

Because of these benefits, tunnelling is widely used in enterprise environments. However, attackers also exploit tunnelling techniques to bypass security controls using trusted protocols such as **ICMP** and **DNS**. Therefore, security analysts must be able to identify anomalies associated with these protocols.

---

# ICMP Analysis

**Internet Control Message Protocol (ICMP)** is designed for diagnosing and reporting network communication issues. It is commonly used for:

- Error reporting
- Network diagnostics
- Connectivity testing (e.g., ping)

Since ICMP is a trusted network-layer protocol, it is sometimes abused for:

- Denial-of-Service (DoS) attacks
- Data exfiltration
- Command and Control (C2) tunnelling

## ICMP Investigation in a Nutshell

ICMP tunnelling activity often appears after:

- Malware execution
- Successful vulnerability exploitation

ICMP packets can carry additional payload data, allowing attackers to:

- Exfiltrate files or information
- Establish C2 channels
- Encapsulate protocols such as:
  - TCP
  - HTTP
  - SSH

Although ICMP provides an effective covert channel, it also has limitations:

- Many enterprise networks block custom ICMP packets.
- Creating custom ICMP packets often requires administrative privileges.

### Indicators of ICMP Tunnelling

Common signs include:

- High volumes of ICMP traffic
- Unusually large packet sizes
- Repeated communication with suspicious destinations
- Signs of encapsulated protocols within ICMP payloads

> Attackers may attempt to evade detection by crafting packets that match the normal ICMP packet size (approximately 64 bytes).

Security analysts should understand both normal and abnormal ICMP behavior to identify possible tunnelling activity and escalate incidents for deeper investigation.

## ICMP Investigation Filters

| Notes | Wireshark Filter |
|---------|----------------|
| Global Search | `icmp` |

### Useful ICMP Indicators

- Packet length
- Destination IP addresses
- Encapsulated protocol signatures within ICMP payloads

### Example Filter

```wireshark
data.len > 64 and icmp
```

This filter identifies ICMP packets carrying payloads larger than the typical size.

---

# DNS Analysis

**Domain Name System (DNS)** translates domain names into IP addresses and is often referred to as the internet's phonebook.

Because DNS is:

- Essential for web services
- Widely trusted
- Constantly used

it is frequently targeted by attackers for:

- Data exfiltration
- Command and Control (C2) communications

## DNS Investigation in a Nutshell

Like ICMP tunnels, DNS tunnelling activity often appears after:

- Malware execution
- Successful exploitation

Attackers typically:

1. Register or control a malicious domain.
2. Configure it as a C2 server.
3. Use malware to send specially crafted DNS queries.

Unlike normal DNS requests, these malicious queries often contain encoded data within subdomains.

### Example

```text
encoded-commands.maliciousdomain.com
```

The encoded subdomain may contain:

- Commands
- Data being exfiltrated
- System information

When the query reaches the attacker's DNS server, it can respond with malicious instructions for the infected host.

Since DNS traffic is a normal part of network activity, these communications often bypass perimeter security devices.

## Indicators of DNS Tunnelling

Security analysts should look for:

- Excessively long DNS queries
- Encoded or unusual subdomains
- High volumes of requests to a single domain
- Known tunnelling tool signatures
- Irregular naming patterns

Common tunnelling tools include:

- dnscat
- dns2tcp

## DNS Investigation Filters

| Notes | Wireshark Filter |
|---------|----------------|
| Global Search | `dns` |

### Useful DNS Indicators

- Query length
- Non-standard domain names
- Long encoded subdomains
- Known tunnelling patterns
- Statistical anomalies in DNS request volume

### Excluding Multicast DNS (mDNS)

```wireshark
!mdns
```

### Detecting Known DNS Tunnelling Tools

```wireshark
dns contains "dnscat"
```

### Detecting Long DNS Queries

```wireshark
dns.qry.name.len > 15 and !mdns
```

This filter highlights DNS queries with unusually long domain names while excluding local multicast DNS traffic.

---

# Key Indicators of Tunnelling Activity

## ICMP Tunnelling

- Large ICMP payloads
- High ICMP traffic volume
- Suspicious destination hosts
- Embedded TCP, HTTP, or SSH data

## DNS Tunnelling

- Long DNS query names
- Encoded subdomains
- Frequent DNS requests to a single domain
- Known tunnelling tool signatures
- Abnormal DNS traffic patterns

Understanding normal ICMP and DNS behavior enables analysts to quickly identify suspicious traffic and investigate potential data exfiltration or command-and-control channels.

# HTTP Analysis

**Hypertext Transfer Protocol (HTTP)** is a cleartext, request-response, client-server protocol used for web communication. Since HTTP traffic is generally allowed through network perimeters and is unencrypted by default, it is one of the most important protocols for network traffic analysis.

HTTP analysis can help detect:

- Phishing pages
- Web application attacks
- Data exfiltration
- Command and Control (C2) traffic

---

# HTTP Analysis in a Nutshell

## Global Search Filters

| Notes | Wireshark Filter |
|---------|----------------|
| HTTP Traffic | `http` |
| HTTP/2 Traffic | `http2` |

> **Note:** HTTP/2 is an improved version of HTTP that supports binary data transfer, multiplexed requests, and enhanced performance and security.

---

# HTTP Request Methods

Request methods indicate the action a client wants the server to perform.

## Common HTTP Methods

### GET

Used to retrieve resources from a web server.

```wireshark
http.request.method == "GET"
```

### POST

Used to submit data to a server.

```wireshark
http.request.method == "POST"
```

### Display All HTTP Requests

```wireshark
http.request
```

---

# HTTP Response Status Codes

Response codes help analysts understand how servers respond to requests.

| Status Code | Description | Filter |
|------------|-------------|----------|
| 200 | OK – Request successful | `http.response.code == 200` |
| 301 | Moved Permanently | |
| 302 | Moved Temporarily | |
| 400 | Bad Request | |
| 401 | Unauthorized | `http.response.code == 401` |
| 403 | Forbidden | `http.response.code == 403` |
| 404 | Not Found | `http.response.code == 404` |
| 405 | Method Not Allowed | `http.response.code == 405` |
| 408 | Request Timeout | |
| 500 | Internal Server Error | |
| 503 | Service Unavailable | `http.response.code == 503` |

---

# HTTP Parameters Analysis

HTTP headers and parameters provide valuable context during investigations.

## User-Agent

Identifies the browser, operating system, or application making the request.

```wireshark
http.user_agent contains "nmap"
```

## Request URI

Identifies the requested resource.

```wireshark
http.request.uri contains "admin"
```

## Full URI

Displays the complete requested URL.

```wireshark
http.request.full_uri contains "admin"
```

---

# HTTP Response Parameters

## Server Header

Identifies the web server software.

```wireshark
http.server contains "apache"
```

## Host Header

Identifies the target hostname.

```wireshark
http.host contains "keyword"
```

Exact match:

```wireshark
http.host == "keyword"
```

## Connection Header

Displays the connection state.

```wireshark
http.connection == "Keep-Alive"
```

## Cleartext Data Search

Search for specific strings within transmitted text data.

```wireshark
data-text-lines contains "keyword"
```

---

# User-Agent Analysis

Attackers often attempt to disguise malicious traffic by modifying the **User-Agent** field to resemble legitimate browser traffic.

While User-Agent analysis is useful, it should never be the sole indicator of malicious activity.

## Indicators of Suspicious User-Agents

### Multiple User-Agents from the Same Host

A host rapidly switching between different browsers or operating systems may indicate suspicious activity.

### Non-Standard or Custom User-Agents

Examples include:

- Custom scripts
- Malware-generated requests
- Automated tools

### Spelling Variations

Examples:

- `Mozilla`
- `Mozlila`
- `Mozlilla`

Minor spelling differences may indicate attempts to evade detection.

### Security Assessment Tools

Look for User-Agents associated with common offensive tools:

- Nmap
- Nikto
- Wfuzz
- sqlmap

### Payload Data in User-Agent Fields

Encoded commands or suspicious characters may indicate exploitation attempts.

## User-Agent Investigation Filter

```wireshark
(http.user_agent contains "sqlmap") or
(http.user_agent contains "Nmap") or
(http.user_agent contains "Wfuzz") or
(http.user_agent contains "Nikto")
```

---

# Log4j Analysis

Before investigating any attack, analysts should understand known attack patterns and indicators.

The **Log4Shell (Log4j)** vulnerability is a Remote Code Execution (RCE) vulnerability that often leaves identifiable traces in HTTP traffic.

---

# Log4j Investigation in a Nutshell

## Known Indicators

- Attack typically begins with an HTTP POST request.
- Common attack strings include:
  - `jndi:ldap`
  - `Exploit.class`
- Encoded payloads often appear in headers such as:
  - User-Agent
  - Referer
  - X-Forwarded-For

---

# Log4j Detection Filters

## Identify POST Requests

```wireshark
http.request.method == "POST"
```

## Search for JNDI or Exploit References

```wireshark
(ip contains "jndi") or (ip contains "Exploit")
```

Or search the entire frame:

```wireshark
(frame contains "jndi") or (frame contains "Exploit")
```

## Detect Suspicious User-Agent Payloads

```wireshark
(http.user_agent contains "$") or
(http.user_agent contains "==")
```

These patterns may indicate encoded payloads or Log4j exploitation attempts.

---

# Key Indicators During HTTP Investigations

## Web Attacks

- Requests targeting `/admin`
- Large numbers of 404 or 403 responses
- Unusual HTTP methods
- Automated scanning tool User-Agents

## Data Exfiltration

- Large POST requests
- Suspicious destinations
- Unusual outbound traffic patterns

## Phishing Activity

- Suspicious hostnames
- Redirect chains (301/302 responses)
- Newly observed domains

## Command & Control (C2)

- Repetitive HTTP requests
- Beaconing patterns
- Encoded parameters or payloads
- Unusual User-Agent values

## Log4j Exploitation

- POST requests containing `${jndi:ldap://...}`
- References to `Exploit.class`
- Encoded payloads in HTTP headers
- Suspicious User-Agent strings

Understanding normal HTTP behaviour and analysing request methods, response codes, headers, URIs, and User-Agent values allows security analysts to quickly identify malicious web activity and potential compromises.

# Decrypting HTTPS Traffic

When investigating web traffic, security analysts frequently encounter encrypted communications. This is typically due to the use of **Hypertext Transfer Protocol Secure (HTTPS)**, which protects data against:

- Spoofing attacks
- Packet sniffing
- Interception attacks

HTTPS relies on the **Transport Layer Security (TLS)** protocol to encrypt communications. Without the appropriate encryption/decryption keys, analysts cannot view the contents of the transferred data.

While HTTPS enhances privacy and security, attackers and malicious websites also use HTTPS to conceal their activities. Therefore, analysts must understand how to decrypt HTTPS traffic using key log files when available.

---

# HTTPS Traffic Characteristics

Unlike standard HTTP traffic, HTTPS packets appear encrypted in packet captures.

As a result:

- URLs are not fully visible.
- Request and response contents are hidden.
- Packet information appears as TLS traffic rather than HTTP traffic.
- Packet colouring in Wireshark differs from standard HTTP traffic.

---

# HTTPS Investigation in a Nutshell

## Useful Wireshark Filters

| Notes | Wireshark Filter |
|---------|----------------|
| HTTP Requests | `http.request` |
| Global TLS Search | `tls` |
| TLS Client Hello | `tls.handshake.type == 1` |
| TLS Server Hello | `tls.handshake.type == 2` |
| SSDP Traffic | `ssdp` |

> **Note:** SSDP (Simple Service Discovery Protocol) is used for advertisement and discovery of network services on local networks.

---

# TLS Handshake Analysis

Just as TCP uses a three-way handshake, TLS has its own handshake process to establish secure communications.

The first two critical messages are:

1. **Client Hello**
2. **Server Hello**

These packets identify which hosts are participating in the encrypted session.

---

## Client Hello Filter

Displays TLS Client Hello packets while excluding SSDP traffic.

```wireshark
(http.request or tls.handshake.type == 1) and !(ssdp)
```

### Purpose

- Identify TLS clients
- Determine initiating hosts
- Discover target servers

---

## Server Hello Filter

Displays TLS Server Hello packets while excluding SSDP traffic.

```wireshark
(http.request or tls.handshake.type == 2) and !(ssdp)
```

### Purpose

- Identify responding servers
- Confirm successful TLS negotiation
- Investigate encrypted sessions

---

# TLS Handshake Overview

## Step 1: Client Hello

The client initiates communication and sends:

- Supported TLS versions
- Supported cipher suites
- Random session values
- Additional security parameters

---

## Step 2: Server Hello

The server responds with:

- Selected TLS version
- Selected cipher suite
- Server certificate information
- Session parameters

---

## Step 3: Key Exchange

The client and server establish cryptographic session keys.

---

## Step 4: Encrypted Communication

Once the handshake completes:

- HTTP requests become HTTPS traffic.
- Application data is encrypted.
- Payload contents are no longer readable without session keys.

---

# SSL/TLS Key Log Files

An **SSL/TLS Key Log File** contains session-specific cryptographic keys that allow Wireshark to decrypt encrypted HTTPS traffic.

These keys are generated automatically during the establishment of each TLS session.

## Important Characteristics

- Keys are generated **per session**.
- Keys must be captured **during the browsing session**.
- Keys cannot usually be recreated after the session ends.
- Traffic cannot be decrypted without the corresponding keys.

---

# Creating SSLKEYLOGFILE

Modern browsers such as:

- Google Chrome
- Mozilla Firefox

support exporting TLS session keys.

To capture these keys:

1. Create an environment variable named:

```text
SSLKEYLOGFILE
```

2. Assign it to a writable file location.

3. Launch the browser.

4. Browse while capturing network traffic.

5. The browser automatically writes TLS session keys to the specified file.

---

# Why Timing Matters

TLS keys are generated when the connection is established.

If traffic is captured **before** enabling key logging:

❌ Session keys will not exist.

If key logging is enabled **during** the capture:

✅ Session keys can be used to decrypt traffic.

Therefore, the key log file must be generated while the TLS session is active.

---

# Loading Key Log Files in Wireshark

Once the SSLKEYLOGFILE has been created, it can be imported into Wireshark.

## Method 1: Right-Click Menu

1. Open the capture file.
2. Right-click the TLS packet.
3. Select the option to add or configure the key log file.

---

## Method 2: Preferences Menu

Navigate to:

```text
Edit
 └── Preferences
      └── Protocols
           └── TLS
```

Then:

1. Locate **(Pre)-Master-Secret Log Filename**.
2. Browse to the SSLKEYLOGFILE.
3. Apply the settings.
4. Reload the capture.

---

# Viewing Decrypted HTTPS Traffic

## Without Key Log File

Analysts can only view:

- Source and destination IP addresses
- TLS handshake information
- Certificates
- Metadata

The actual web content remains encrypted.

---

## With Key Log File

Wireshark can decrypt and display:

- HTTP requests
- HTTP responses
- URLs
- Headers
- Form submissions
- Cookies
- Downloaded content

This effectively converts encrypted HTTPS traffic into readable HTTP traffic within the packet capture.

---

# Useful TLS Investigation Filters

## Display All TLS Traffic

```wireshark
tls
```

## Display TLS Client Hello Packets

```wireshark
tls.handshake.type == 1
```

## Display TLS Server Hello Packets

```wireshark
tls.handshake.type == 2
```

## Exclude SSDP Traffic

```wireshark
!(ssdp)
```

## Client Hello Analysis

```wireshark
(http.request or tls.handshake.type == 1) and !(ssdp)
```

## Server Hello Analysis

```wireshark
(http.request or tls.handshake.type == 2) and !(ssdp)
```

---

# Key Takeaways

- HTTPS uses TLS encryption to protect web traffic.
- Encrypted HTTPS traffic cannot be inspected without session keys.
- TLS handshakes begin with **Client Hello** and **Server Hello** messages.
- SSL/TLS session keys can be exported using the **SSLKEYLOGFILE** environment variable.
- Wireshark can decrypt HTTPS traffic when the correct key log file is provided.
- Decrypted traffic enables analysts to inspect URLs, requests, responses, cookies, and transferred data for security investigations.
  


# NetworkMiner in Network Forensics

The primary goal of **Network Forensics** is to identify malicious activities, security breaches, and network anomalies through the analysis of network traffic.

**NetworkMiner** is a valuable network forensic analysis tool that helps investigators quickly identify useful information and determine where to begin an investigation.

It provides insights such as:

- Context about captured hosts (IP addresses, MAC addresses, hostnames, operating systems)
- Potential attack indicators and anomalies (traffic spikes, port scans, suspicious connections)
- Identification of tools used by attackers (e.g., Nmap)

---

# Supported Data Types

Network forensic investigations commonly involve three types of data:

## Live Traffic

Traffic captured directly from a network interface in real time.

## Traffic Captures

Previously recorded packet capture files such as:

```text
.pcap
.pcapng
```

## Log Files

Network and system logs used to support forensic investigations.

---

# NetworkMiner in a Nutshell

| Capability | Description |
|------------|-------------|
| Traffic Sniffing | Captures and logs packets traversing the network. |
| PCAP Parsing | Parses packet capture files and displays detailed packet information. |
| Protocol Analysis | Identifies protocols used within captured traffic. |
| OS Fingerprinting | Determines operating systems based on network traffic characteristics. |
| File Extraction | Extracts files such as images, HTML pages, and emails from captures. |
| Credential Grabbing | Identifies and extracts credentials transmitted in network traffic. |
| Cleartext Keyword Parsing | Extracts readable strings and keywords from captures. |

---

# OS Fingerprinting

One of NetworkMiner's strongest features is **Operating System Fingerprinting**.

It identifies operating systems based on packet characteristics and network stack behavior.

NetworkMiner primarily relies on:

- Satori
- p0f

These tools analyse TCP/IP stack behaviour and packet signatures to estimate the operating system of communicating hosts.

---

# File Extraction

NetworkMiner can automatically extract:

- Images
- HTML files
- Documents
- Email attachments
- Other transferred files

This feature significantly reduces the time required to locate transferred artifacts during investigations.

---

# Credential Extraction

NetworkMiner can identify credentials transmitted across the network.

Examples include:

- Usernames
- Passwords
- Authentication tokens

This capability is especially useful when analysing:

- Legacy protocols
- Misconfigured services
- Unencrypted communications

---

# Cleartext Keyword Parsing

NetworkMiner can automatically extract readable text found within network traffic.

Examples include:

- URLs
- Usernames
- Search terms
- Commands
- Error messages

This helps investigators quickly identify relevant information without manually inspecting packets.

---

# NetworkMiner Editions

This guide focuses on the **NetworkMiner Free Edition**.

The **Professional Edition** provides additional capabilities, including:

- Enhanced file extraction
- Improved reporting
- Advanced protocol support
- Additional forensic analysis features

---

# Operating Modes

NetworkMiner supports two primary operating modes:

---

## 1. Sniffer Mode

NetworkMiner can capture live traffic directly from the network.

### Characteristics

- Available primarily on Windows
- Supports packet capture and logging
- Less reliable than dedicated packet capture tools

### Important Note

Although NetworkMiner includes a packet capture feature, it is not intended to replace dedicated packet analyzers such as:

- Wireshark
- tcpdump

The developers describe it primarily as a:

> Network Forensic Analysis Tool with packet sniffing capabilities.

---

## 2. Packet Parsing / Processing Mode

This is the recommended mode for most investigations.

### Purpose

Analyse previously captured packet files to obtain a rapid overview of:

- Hosts
- Services
- Credentials
- Files
- Network relationships

### Benefits

- Quickly identifies low-hanging fruit
- Reduces investigation time
- Helps prioritise deeper analysis

---

# Pros and Cons of NetworkMiner

## Advantages

### OS Fingerprinting

Quickly identifies operating systems from network traffic.

### Easy File Extraction

Automatically extracts transferred files.

### Credential Discovery

Locates transmitted credentials.

### Cleartext Keyword Parsing

Extracts human-readable content.

### Rapid Overview

Provides a fast summary of network activity and hosts.

---

## Disadvantages

### Limited Live Sniffing

Not ideal as a primary packet capture solution.

### Poor Performance with Large PCAP Files

Large captures can become difficult to process efficiently.

### Limited Filtering Capabilities

Filtering options are significantly less powerful than Wireshark.

### Not Designed for Deep Packet Investigation

Limited packet-level analysis compared to specialised packet analyzers.

---

# NetworkMiner vs Wireshark

Although both tools analyse network traffic, their intended purposes differ significantly.

## Recommended Workflow

1. Capture traffic.
2. Load the PCAP into NetworkMiner.
3. Obtain a quick overview.
4. Identify hosts, files, credentials, and anomalies.
5. Perform detailed investigation using Wireshark.

---

# Feature Comparison

| Feature | NetworkMiner | Wireshark |
|----------|-------------|-----------|
| Primary Purpose | Quick overview, traffic mapping, and data extraction | Deep packet analysis |
| GUI | ✅ | ✅ |
| Sniffing | ✅ | ✅ |
| PCAP Processing | ✅ | ✅ |
| OS Fingerprinting | ✅ | ❌ |
| Parameter / Keyword Discovery | ✅ | Manual |
| Credential Discovery | ✅ | ✅ |
| File Extraction | ✅ | ✅ |
| Filtering Options | Limited | ✅ |
| Packet Decoding | Limited | ✅ |
| Protocol Analysis | ❌ | ✅ |
| Payload Analysis | ❌ | ✅ |
| Statistical Analysis | ❌ | ✅ |
| Cross-Platform Support | ✅ | ✅ |
| Host Categorisation | ✅ | ❌ |
| Ease of Management | ✅ | ✅ |

---

# When to Use NetworkMiner

NetworkMiner is particularly useful when you need to:

- Quickly identify hosts within a capture
- Discover usernames and credentials
- Extract files from traffic
- Determine operating systems
- Locate suspicious communications
- Build an initial understanding of a PCAP file

---

# When to Use Wireshark Instead

Wireshark is the better choice when you need to:

- Analyse protocols in depth
- Inspect packet payloads
- Create complex filters
- Perform statistical analysis
- Investigate attack techniques in detail
- Reconstruct communication sessions

---



