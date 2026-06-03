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
