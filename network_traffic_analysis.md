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

### Configuration Example: Cisco SPAN (Port Mirroring)
```text
Switch(config)# monitor session 1 source interface fastEthernet0/1
Switch(config)# monitor session 1 destination interface fastEthernet0/2
