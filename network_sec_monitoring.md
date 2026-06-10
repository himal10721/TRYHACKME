# Network Components – Building Blocks of a Network

A computer network is not simply a collection of connected devices; it is a structured environment where network assets communicate, share resources, and provide access to internal and external services.

Understanding the purpose and security significance of each network component helps security analysts quickly identify abnormal behaviour, detect attacks, and investigate incidents effectively.

---

# Small Enterprise Network

A typical enterprise network consists of several key components that work together to support business operations.

From a security perspective, each component presents unique risks and monitoring requirements.

---

# User Workstations (Endpoints)

User workstations include:

- Desktop computers
- Laptops
- Thin clients

These devices are used by employees to perform daily business activities.

## Common Attack Scenarios

Endpoints are often the first point of compromise through:

- Phishing emails
- Malicious downloads
- USB-based malware
- Browser exploits

### Example

A finance employee receives a phishing email containing malware.

The malware executes and establishes a Command and Control (C2) connection to an attacker-controlled server.

---

## Security Importance

Endpoints are commonly:

- Less monitored than servers
- Widely distributed across the organization
- Operated by non-technical users

Once compromised, attackers can:

- Steal credentials
- Escalate privileges
- Move laterally across the network

### What Analysts Should Monitor

- Endpoint logs
- Antivirus alerts
- EDR (Endpoint Detection & Response) events
- Unusual outbound network connections
- C2 traffic indicators

> Network logs often reveal malicious communications before endpoint logs identify the malware itself.

---

# File & Database Servers

These servers store the organization's most valuable asset: **data**.

## File Servers

Provide centralized storage for:

- Documents
- Shared folders
- Project files

## Database Servers

Store structured information such as:

- Customer records
- Financial information
- Employee data
- Inventory records

---

## Security Importance

Attackers frequently target these systems because they contain high-value information.

### Common Threats

- Ransomware attacks
- Data theft
- Insider threats
- Unauthorized access

### Example

A ransomware group encrypts a file server containing all company documents.

The business becomes unable to access critical information until recovery occurs.

---

## What Analysts Should Monitor

- Large file transfers
- Unusual database queries
- Privilege escalation events
- Data exfiltration activity
- Unauthorized access attempts

---

# Application Servers

Application servers provide services required by employees and customers.

Examples include:

- Web Servers
- Email Servers
- VPN Gateways
- Internal Applications

---

## Web Servers

Host:

- Company websites
- Web applications
- Customer portals

### Common Threats

- SQL Injection
- Cross-Site Scripting (XSS)
- Remote Code Execution (RCE)

---

## Email Servers

Handle:

- Internal communications
- External communications
- Email delivery and storage

### Common Threats

- Phishing campaigns
- Malware distribution
- Account compromise

---

## VPN Gateways

Provide secure remote access to internal resources.

### Common Threats

- Credential stuffing
- Brute-force attacks
- VPN vulnerabilities

---

## Security Importance

Because these systems are Internet-facing, they are constantly targeted by attackers.

A compromise often provides:

- Initial network access
- Credential exposure
- Access to internal resources

---

## What Analysts Should Monitor

### Application Logs

Look for:

- Exploit attempts
- Unexpected errors
- Suspicious user activity

### Firewall Logs

Monitor:

- Blocked connections
- Unauthorized access attempts

### IDS/IPS Alerts

Detect:

- Known attack signatures
- Malicious traffic patterns

---

# Active Directory (AD) / Authentication Servers

Active Directory serves as the identity management system for most enterprise environments.

It manages:

- Users
- Groups
- Computers
- Access permissions

Employees use AD credentials to access:

- Workstations
- Email
- File servers
- Business applications

---

## Security Importance

Active Directory is often considered the most critical component of the enterprise network.

### Why?

It controls:

- Authentication
- Authorization
- User privileges

Compromising Active Directory can provide attackers with control over the entire environment.

---

## Common Attack Objectives

- Privilege escalation
- Credential theft
- Persistence
- Lateral movement

### Example

An attacker compromises a Domain Administrator account.

This can effectively grant unrestricted access to the enterprise.

---

## What Analysts Should Monitor

### Failed Logins

Indicators of:

- Password spraying
- Brute-force attacks

### Unusual Login Activity

Examples:

- Logins outside business hours
- Logins from unexpected locations
- Concurrent logins from multiple locations

### Privilege Changes

Examples:

- New administrator accounts
- Unauthorized group membership changes

---

# Routers & Switches (Network Infrastructure)

Network infrastructure devices provide communication between systems.

---

## Routers

Routers connect:

- Internal networks
- External networks
- The Internet

They determine how traffic moves between networks.

---

## Switches

Switches connect devices within the same network.

Examples include:

- Workstations
- Printers
- Servers

---

## Security Importance

Although not usually targeted first, compromised infrastructure devices provide significant advantages to attackers.

### Potential Risks

#### Traffic Interception

Attackers may capture sensitive data.

#### Man-in-the-Middle (MITM) Attacks

Traffic can be modified or redirected.

#### Hidden Backdoors

Traffic may be secretly routed to attacker-controlled systems.

#### Persistence

Infrastructure devices often receive less monitoring than servers and endpoints.

---

## What Analysts Should Monitor

- Configuration changes
- Routing modifications
- New management connections
- Unauthorized administrative logins

---

# Firewalls / Perimeter Devices

A firewall acts as the primary security boundary between:

- Trusted internal networks
- Untrusted external networks

It examines network traffic and applies security policies to determine whether connections should be allowed or blocked.

---

## Modern Firewall Capabilities

In addition to packet filtering, modern firewalls often provide:

- Application awareness
- Intrusion Prevention Systems (IPS)
- Malware detection
- URL filtering
- SSL/TLS inspection

---

## Security Importance

Firewalls help:

- Prevent unauthorized access
- Protect internal services
- Restrict malicious traffic
- Enforce security policies

---

## Common Threats Detected by Firewalls

### Port Scanning

Attackers searching for open services.

### Brute-Force Attacks

Repeated login attempts against exposed services.

### Exploitation Attempts

Attempts to exploit vulnerabilities in exposed applications.

### Malware Communications

Outbound connections to malicious infrastructure.

---

## What Analysts Should Monitor

### Firewall Logs

Review:

- Allowed connections
- Blocked connections
- Policy violations

### Suspicious IP Addresses

Look for:

- Repeated connection attempts
- Threat intelligence matches
- Known malicious sources

### Traffic Anomalies

Examples:

- Sudden traffic spikes
- Unexpected outbound connections
- New external destinations

---

# Summary

A small enterprise network is built from several critical components:

| Component | Primary Function | Security Importance |
|------------|----------------|--------------------|
| User Workstations | Employee productivity | Common initial compromise point |
| File Servers | Store shared data | Frequent ransomware target |
| Database Servers | Store structured data | High-value data repository |
| Application Servers | Deliver business services | Common external attack surface |
| Active Directory | Identity management | Controls enterprise authentication |
| Routers & Switches | Network connectivity | Enable traffic control and visibility |
| Firewalls | Perimeter protection | First line of defense against external threats |

Understanding how these components function and interact enables security analysts to identify suspicious activity, investigate incidents more effectively, and protect critical enterprise assets.

# Network Visibility in Cyber Security

**Network visibility** is the ability to monitor, observe, and understand what is happening across a network. It is one of the most important principles in cyber security because:

> **You cannot defend what you cannot see.**

Effective network visibility enables organizations to:

- Detect threats and suspicious activity
- Investigate security incidents
- Monitor network health and performance
- Maintain a strong security posture
- Build accurate attack timelines

Network visibility does not necessarily mean inspecting every packet individually. Instead, it involves collecting and analysing relevant information from multiple sources to create a comprehensive picture of network activity.

Without adequate visibility, organizations are effectively blind to potential threats operating within their environment.

---

# Why is Network Visibility Important?

Security teams rely on visibility to answer critical questions such as:

- Who accessed a system?
- What actions were performed?
- When did the activity occur?
- Where did the connection originate?
- How did the attack progress?

By answering these questions, analysts can:

- Detect attacks earlier
- Identify compromised systems
- Investigate incidents efficiently
- Improve security controls
- Reduce attacker dwell time

---

# Sources of Network Visibility

Most network visibility comes from **logs**, which are records of events generated by systems and network devices.

There are two primary categories of logs:

1. Host-Centric Logs
2. Network-Centric Logs

Understanding the differences between these sources is essential for effective incident investigation.

---

# Host-Centric Logs

Host-centric logs are generated by individual devices connected to the network.

Examples include:

- Servers
- Workstations
- Laptops
- Virtual machines

These logs provide a detailed view of activity occurring directly on a specific device.

---

## What Host-Centric Logs Show

Host logs can reveal:

- User logins and logouts
- Process execution
- Service activity
- Application behaviour
- File access events
- System configuration changes
- Security tool detections

They help analysts understand the direct impact of an attack on a system.

---

# Key Host-Centric Log Sources

## Operating System Logs

Generated by the operating system itself.

### Examples

#### Windows

- Event Viewer Logs
- Security Logs
- System Logs
- Application Logs

#### Linux

- Syslog
- Auth Logs
- Kernel Logs

#### macOS

- Unified Logs
- System Logs

### Common Events

- Successful logins
- Failed logins
- Process creation
- Service startup and shutdown
- User account changes

---

## Application Logs

Generated by software running on the host.

### Examples

#### Web Servers

- Apache Logs
- Nginx Logs

#### Databases

- MySQL Logs
- MSSQL Logs
- PostgreSQL Logs

#### Business Applications

- ERP systems
- CRM platforms
- Internal services

### Common Events

- User activity
- Errors
- Access attempts
- Transactions

---

## Security Tool Logs

Generated by endpoint security products.

### Examples

- Antivirus solutions
- Endpoint Detection & Response (EDR)
- Host Intrusion Detection Systems (HIDS)

### Common Events

- Malware detection
- Suspicious processes
- Blocked activities
- Threat alerts

---

# Importance of Host-Centric Logs

Host logs help answer questions such as:

- What happened on the system?
- Which process executed?
- Which user performed the action?
- What files were modified?
- Was malware executed?

### Example

A user opens a malicious attachment.

Host logs may reveal:

- The file execution
- Registry modifications
- Process creation
- Malware persistence mechanisms

This level of detail is often unavailable from network logs alone.

---

# Network-Centric Logs

Network-centric logs provide visibility into communications occurring between systems.

Unlike host logs, which focus on a single device, network logs focus on traffic flowing across the network.

---

## What Network-Centric Logs Show

Network logs commonly record:

- Source IP addresses
- Destination IP addresses
- Source ports
- Destination ports
- Protocols used
- Connection status
- Actions taken (allowed or blocked)

These logs help analysts understand:

- Who communicated with whom
- When communication occurred
- How much data was transferred
- Whether traffic was allowed or blocked

---

# Why Network-Centric Logs Are Important

Network logs provide crucial context during investigations.

They help identify:

- Reconnaissance activity
- Initial compromise attempts
- Lateral movement
- Command and Control (C2) communications
- Data exfiltration

---

## Analogy

Think of an organization as a building:

### Host-Centric Logs

Tell you:

> What happened inside a room.

### Network-Centric Logs

Tell you:

> Who entered and exited the building.

Both perspectives are necessary for a complete investigation.

---

# Key Network-Centric Log Sources

## Firewalls

Firewalls monitor and control traffic entering and leaving the network.

### What Firewall Logs Show

- Allowed connections
- Blocked connections
- Policy violations
- Source and destination addresses

### Why They Matter

Firewall logs are often the first place analysts identify:

- Port scans
- Unauthorized access attempts
- Brute-force attacks
- External reconnaissance

---

## Intrusion Detection & Prevention Systems (IDS/IPS)

IDS and IPS solutions inspect network traffic for malicious activity.

### Detection Methods

#### Signature-Based Detection

Matches traffic against known attack patterns.

#### Anomaly-Based Detection

Identifies unusual behaviour.

### Why They Matter

IDS/IPS logs can reveal:

- Exploitation attempts
- Malware traffic
- Command and Control communications
- Active attacks in progress

---

## Routers & Switches

Network infrastructure devices can generate flow records.

### Examples

- NetFlow
- IPFIX
- sFlow

### What Flow Data Shows

- Source and destination devices
- Duration of communication
- Volume of transferred data
- Protocol usage

### Why It Matters

Provides a high-level overview of network activity without storing full packet captures.

---

## Web Proxies

Organizations often route web traffic through proxy servers.

### What Proxy Logs Show

- Websites visited
- URLs accessed
- Downloaded files
- User identities

### Why They Matter

Proxy logs help detect:

- Malicious websites
- Phishing attempts
- Policy violations
- Data exfiltration

---

## VPN Logs

VPN systems provide secure remote access to enterprise resources.

### What VPN Logs Show

- User identity
- Connection times
- Source IP addresses
- Session duration

### Why They Matter

VPN logs help identify:

- Unauthorized access
- Stolen credentials
- Suspicious remote logins
- Impossible travel scenarios

---

# Importance of Network-Centric Logs

Network logs help answer questions such as:

- Who initiated the connection?
- Which systems communicated?
- Was the connection allowed or blocked?
- Was data transferred externally?
- Did suspicious traffic occur before a compromise?

### Example

An attacker performs a port scan against an external web server.

Network logs may reveal:

- Source IP address
- Target ports
- Scan timing
- Connection frequency

Even if the attack fails, evidence often remains in network logs.

---

# Correlating Host and Network Logs

The most effective investigations combine both log sources.

## Host Logs Answer

- What happened on the system?
- Which process executed?
- Which files changed?

## Network Logs Answer

- Who connected?
- When did communication occur?
- Where did traffic go?
- How much data was transferred?

---

# Example Incident Investigation

### Network Logs Reveal

```text
Workstation → Suspicious External IP
```

### Host Logs Reveal

```text
User opened malicious attachment
↓
PowerShell launched
↓
Malware executed
↓
C2 connection established
```

Combining both perspectives creates a complete incident timeline.

---

# Summary

| Log Type | Focus | Examples |
|-----------|--------|-----------|
| Host-Centric | Activity occurring on individual systems | OS logs, application logs, EDR logs |
| Network-Centric | Communication between systems | Firewalls, IDS/IPS, VPN, proxy, NetFlow |

---

# Key Takeaways

- Network visibility is a foundational concept in cyber security.
- Security teams rely on logs to understand activity across the environment.
- Host-centric logs provide detailed information about individual systems.
- Network-centric logs provide visibility into communications between systems.
- Neither source alone provides a complete picture.
- Effective threat detection and incident response require correlating both host and network logs.
- Combining multiple visibility sources enables analysts to reconstruct accurate attack timelines and identify malicious activity more effectively.

# Data Exfiltration

**Data exfiltration** is the unauthorized transfer of data from an organization to an external destination controlled by an adversary.

The theft may be:

- **Deliberate** (malicious insider activity)
- **Unintentional** (compromised accounts)
- **Malware-driven** (data theft performed by malicious software)

Data exfiltration is one of the most significant threats organizations face because it can result in financial loss, reputational damage, regulatory penalties, and exposure of sensitive information.

---

# Why Adversaries Perform Data Exfiltration

Attackers steal data for various reasons depending on their objectives.

## Financial Gain

Stolen information can be sold or monetized.

### Examples

- Credit card information
- Banking credentials
- Personally Identifiable Information (PII)
- Customer databases

### Impact

- Fraud
- Identity theft
- Financial losses

---

## Espionage

Nation-state and advanced threat actors often target sensitive information.

### Examples

- Intellectual property
- Trade secrets
- Research data
- Government information

### Impact

- Strategic advantage
- Competitive intelligence
- National security concerns

---

## Ransomware & Extortion

Modern ransomware groups frequently steal data before encrypting systems.

### Common Approach

1. Steal sensitive data
2. Encrypt victim systems
3. Demand ransom
4. Threaten public disclosure if payment is refused

### Impact

- Operational disruption
- Data leaks
- Double extortion

---

## Disruption & Sabotage

Some attackers aim to damage organizations rather than profit directly.

### Examples

- Leaking confidential information
- Publicly exposing internal communications
- Releasing sensitive customer records

---

## Persistence & Reconnaissance

Attackers may exfiltrate information to better understand the environment.

### Examples

- Network diagrams
- Password databases
- User lists
- Configuration files

This information can facilitate future attacks.

---

# Real-World Threat Actors and Exfiltration Methods

| Threat Actor / Campaign | Exfiltration Technique | Description |
|-------------------------|-----------------------|-------------|
| APT29 (Cozy Bear) | HTTPS over legitimate domains | Used encrypted HTTPS channels to exfiltrate data from government networks. |
| FIN7 | HTTP POST Requests | Embedded stolen data in HTTP POST traffic to Command & Control servers. |
| Lunar Spider (Zloader) | Encrypted C2 Channels | Maintained long-term access and staged data theft through encrypted communications. |
| DarkSide Ransomware | Dual Extortion | Stole data before encryption and threatened public release. |
| APT10 (Cloud Hopper) | Cloud-to-Cloud Transfers | Used cloud APIs to exfiltrate data from managed service providers. |

---

# Common Phases of Data Exfiltration

Data exfiltration typically follows a structured attack lifecycle.

---

## 1. Discovery / Collection

The attacker identifies valuable information.

### Targets

- Documents
- Databases
- Credentials
- Intellectual property
- Customer records

### Indicators

- Large numbers of file access events
- Extensive directory enumeration
- Database queries

---

## 2. Staging / Compression

Before transfer, data is often consolidated.

### Common Techniques

- ZIP archives
- RAR archives
- 7z compression
- TAR archives
- Base64 encoding
- Encryption
- Steganography

### Purpose

- Reduce transfer size
- Avoid detection
- Simplify exfiltration

---

## 3. Exfiltration Transport

The attacker transfers data outside the organization.

### Common Channels

- HTTPS
- FTP
- SFTP
- SCP
- Cloud storage services
- DNS tunnelling
- USB devices

---

## 4. Command & Control (C2) Coordination

Attackers often use a C2 server to:

- Coordinate transfers
- Verify successful uploads
- Issue additional commands

---

# Data Exfiltration Techniques & Indicators

Detection requires visibility across:

- Host logs
- Network logs
- Cloud logs

No single log source provides the full picture.

---

# Network-Based Exfiltration

Data is transferred across network protocols.

## Common Techniques

- HTTP/HTTPS uploads
- FTP
- SFTP
- SCP
- DNS tunnelling
- ICMP tunnelling
- Custom TCP/UDP protocols

---

## Indicators of Compromise

### Proxy Logs

Look for:

- Large POST requests
- Uploads to cloud storage providers
- Unusual outbound transfers

### Firewall Logs

Look for:

- High outbound traffic volume
- Large transfers to unknown destinations

### NetFlow Data

Look for:

- Traffic spikes
- Long-duration connections
- Large outbound flows

### DNS Logs

Look for:

- Long domain names
- High-entropy subdomains
- DNS TXT queries
- Repetitive DNS requests

---

# Host-Based Exfiltration

Attackers often use legitimate tools already present on systems.

## Common Tools

### PowerShell

```powershell
Invoke-WebRequest
```

### Command-Line Utilities

```bash
curl
wget
```

### Cloud Transfer Tools

```bash
awscli
rclone
```

### Archive Utilities

```text
zip
rar
7z
```

### Removable Media

- USB devices
- External drives

---

## Indicators of Compromise

### Sysmon / EDR Logs

Look for:

- Process creation
- Network connections
- File creation activity

### Important Sysmon Events

| Event ID | Description |
|-----------|-------------|
| 1 | Process Creation |
| 3 | Network Connection |
| 11 | File Creation |

### Windows Security Logs

Monitor:

```text
4656 - Handle Request
4663 - Object Access
```

### Linux

Monitor:

- auditd logs
- shell history
- file access events

---

# Cloud-Based Exfiltration

Attackers increasingly use cloud services as exfiltration destinations.

## Common Services

- Amazon S3
- Azure Blob Storage
- Google Cloud Storage
- SharePoint
- OneDrive
- Google Drive

---

## Indicators of Compromise

### AWS

Monitor:

```text
PutObject
Multipart Upload
```

### Azure

Monitor:

- Blob uploads
- Storage API activity

### Google Cloud

Monitor:

- Object creation
- Storage transfers

---

## Relevant Log Sources

- AWS CloudTrail
- Azure Activity Logs
- Google Cloud Audit Logs

---

# Covert & Encoded Exfiltration

Attackers may disguise data to avoid detection.

## Common Techniques

### DNS Tunnelling

Data encoded inside DNS requests.

### Base64 Encoding

Converts binary data into text-based format.

### Steganography

Hides information inside:

- Images
- Audio files
- Videos

### Low-and-Slow Exfiltration

Files are divided into many small transfers.

---

## Indicators of Compromise

### DNS Logs

Look for:

- Long subdomains
- Encoded strings
- Frequent DNS TXT queries

### Proxy Logs

Look for:

- Numerous small uploads
- Repeated outbound requests

### Correlation

Combine:

- DNS activity
- Upload activity
- Suspicious process execution

---

# Insider Threat & Collaboration Tool Exfiltration

Attackers may use legitimate collaboration platforms.

## Common Services

- Slack
- Microsoft Teams
- Dropbox
- Google Drive
- Box
- SharePoint

---

## Indicators of Compromise

### Audit Logs

Monitor:

- File uploads
- External sharing events
- Mass downloads

### Email Logs

Monitor:

- Large attachments
- External recipients
- Unusual forwarding activity

---

# General Indicators of Attack (IoAs)

The following signs frequently indicate possible exfiltration activity.

## Network Indicators

- Large outbound data transfers
- Traffic spikes
- Unknown destination domains
- Long-duration outbound sessions
- Excessive cloud storage uploads

---

## Host Indicators

- Archive creation before uploads
- Suspicious PowerShell activity
- Unexpected use of transfer tools
- Numerous file reads before network connections

---

## Cloud Indicators

- Unusual API activity
- New storage destinations
- Service account abuse
- Access from unfamiliar locations

---

# SOC Level 1 Exfiltration Triage

When investigating a potential exfiltration event, analysts should focus on:

## Source

- Which host initiated the transfer?
- Which user was involved?

## Destination

- External IP address
- Domain name
- Cloud storage service

## Volume

- How much data was transferred?
- Was the volume abnormal?

## Process

- Which process initiated the connection?
- Was the process legitimate?

## Timeline

- When did file access occur?
- When did the upload begin?

---

# Recommended Log Correlation

A strong investigation combines evidence from:

| Source | Information Provided |
|----------|---------------------|
| Proxy Logs | Web uploads and downloads |
| Firewall Logs | Allowed/blocked connections |
| NetFlow | Traffic volume and communication patterns |
| DNS Logs | Domain lookups and tunnelling indicators |
| Sysmon / EDR | Process and network activity |
| Cloud Logs | Storage and API activity |
| Email Logs | Message and attachment activity |

---

# Key Takeaways

- Data exfiltration is the unauthorized transfer of sensitive information outside an organization.
- Attackers perform exfiltration for financial gain, espionage, extortion, sabotage, and future attack planning.
- Exfiltration commonly involves four phases: **Discovery → Staging → Transfer → C2 Coordination**.
- Adversaries frequently use legitimate tools and trusted protocols such as HTTPS, DNS, cloud storage services, and collaboration platforms.
- Detecting exfiltration requires correlating host, network, and cloud telemetry.
- Effective SOC investigations focus on identifying **who accessed the data, what was transferred, how it was staged, and where it was sent**.
- Successful detection relies on visibility across multiple log sources rather than a single alert or event.

# Data Exfiltration Techniques: DNS, FTP, HTTP, and ICMP

Attackers frequently abuse legitimate protocols to move sensitive information outside an organization. Because these protocols are commonly allowed through firewalls and security controls, malicious traffic can blend into normal network activity.

This guide covers four common exfiltration methods:

1. DNS Exfiltration
2. FTP Exfiltration
3. HTTP Exfiltration
4. ICMP Exfiltration

---

# DNS Exfiltration

## Overview

**DNS Exfiltration** abuses the **Domain Name System (DNS)** to transfer data outside the network.

Instead of using DNS solely for hostname resolution, attackers encode sensitive data into DNS queries or responses and send them to attacker-controlled DNS servers.

Since DNS traffic is almost always allowed through firewalls and proxies, it provides an effective covert communication channel.

---

## DNS Tunneling

DNS tunneling is the process of embedding data inside DNS requests and responses.

### Example

Normal DNS Query:

```text
www.example.com
```

Malicious DNS Query:

```text
U2Vuc2l0aXZlRGF0YQ.attackerdomain.com
```

The subdomain contains encoded data that the attacker's DNS server extracts and reconstructs.

---

## Why Attackers Use DNS

### Always Available

DNS traffic is essential for network operations and is typically permitted.

### High Cover

DNS queries appear legitimate unless inspected closely.

### Flexible Payloads

Data can be stored inside:

- Subdomains
- TXT records
- DNS responses

---

## DNS Characteristics

| Feature | Description |
|----------|-------------|
| Protocol | DNS |
| Port | 53 |
| Transport | UDP (primarily), TCP (occasionally) |
| Common Records | A, AAAA, MX, TXT, CNAME |

---

## Indicators of DNS Exfiltration

### High Query Volume

Many DNS requests sent to a single external domain.

### Long Domain Names

Query names exceeding normal lengths.

Examples:

```text
dns.qry.name.len > 60
```

or

```text
dns.qry.name.len > 100
```

---

### High Entropy Strings

Encoded data often appears random.

Examples:

```text
a8k3mx92zq91vxn2d93b.domain.com
```

Look for:

- Base32 strings
- Base64 strings
- Random character patterns

---

### Excessive TXT Queries

TXT records are frequently abused for command and control and exfiltration.

---

### Frequent NXDOMAIN Responses

Attackers may generate fake domains that never resolve.

Indicators:

```text
Repeated NXDOMAIN responses
```

---

### Beaconing Behaviour

Regularly timed DNS requests.

Examples:

```text
Every 60 seconds
Every 5 minutes
```

---

# FTP Exfiltration

## Overview

**File Transfer Protocol (FTP)** is one of the oldest file transfer protocols.

It provides direct file upload and download capabilities between clients and servers.

Because FTP is designed for transferring files, attackers often use it to move large quantities of stolen data.

---

## Why Attackers Use FTP

### Efficient Transfers

Large files can be uploaded quickly.

### Existing Infrastructure

Many organizations still maintain FTP servers.

### Credential Abuse

Compromised credentials can provide access to legitimate FTP services.

---

## Common Attack Methods

### Legitimate FTP Servers

Uploading stolen data to public FTP services.

### Compromised Accounts

Using stolen user credentials.

### Non-Standard Ports

Moving FTP traffic away from common detection signatures.

### Tunneling

Encapsulating FTP inside other protocols.

---

## FTP Indicators of Attack

### Cleartext Credentials

FTP transmits credentials in plaintext.

Look for:

```text
USER
PASS
```

commands.

---

### File Upload Commands

```text
STOR
```

indicates file upload activity.

---

### File Download Commands

```text
RETR
```

indicates file download activity.

---

### Large Data Transfers

Look for:

- High outbound traffic volume
- Long FTP sessions
- Large file uploads

---

### Unusual External Destinations

Connections to:

- Unknown FTP servers
- Rare destinations
- Foreign hosting providers

---

### After-Hours Activity

Large transfers occurring outside normal business hours.

---

# HTTP Exfiltration

## Overview

HTTP-based exfiltration uses web traffic to transfer data outside the organization.

Because HTTP traffic is extremely common, attackers can often blend malicious traffic into normal user activity.

---

## Why HTTP Exfiltration Matters

### Legitimate Traffic

Most organizations generate large amounts of HTTP/HTTPS traffic.

### Firewall Friendly

Web traffic is usually permitted.

### Difficult to Distinguish

Malicious uploads can resemble legitimate web activity.

---

# Common HTTP Exfiltration Techniques

## HTTP POST Uploads

The most common exfiltration method.

Example:

```http
POST /upload.php HTTP/1.1
```

The request body contains stolen data.

---

## Encoded GET Requests

Small amounts of data can be embedded inside:

- Query parameters
- URL paths

Example:

```http
GET /collect?data=BASE64DATA
```

---

## Custom HTTP Headers

Attackers may hide data in headers.

Example:

```http
X-Data: U2Vuc2l0aXZlRGF0YQ==
```

---

## Chunked Transfers

Large files split into multiple HTTP requests.

Purpose:

- Avoid size-based detection
- Blend into normal traffic

---

## Multipart Uploads

Files divided into multiple segments.

Commonly seen in:

- Cloud uploads
- File sharing applications

---

## HTTPS/TLS Tunneling

Data transferred through encrypted HTTPS sessions.

Challenges:

- Payload not visible
- Requires TLS inspection
- Detection relies on metadata

---

## Cloud Storage Abuse

Attackers upload stolen information to:

- Dropbox
- Google Drive
- GitHub
- OneDrive
- Gists

---

# HTTP Indicators of Attack

### Large POST Requests

Unusually large uploads to external hosts.

---

### Rare Destinations

Connections to domains not typically observed in baseline traffic.

---

### Beaconing

Repeated small requests to a single destination.

---

### Followed by Large Uploads

Pattern:

```text
Small periodic requests
↓
Large POST request
```

Often indicates command-and-control followed by exfiltration.

---

### Multipart Transfers

Numerous related uploads that collectively form a large file.

---

# ICMP Exfiltration

## Overview

**Internet Control Message Protocol (ICMP)** is primarily used for:

- Diagnostics
- Network troubleshooting
- Error reporting

Examples include:

```text
ping
traceroute
```

Because ICMP is often trusted and lightly inspected, attackers may abuse it to create covert channels.

---

## How ICMP Exfiltration Works

Attackers encode data into ICMP packet payloads and send them to attacker-controlled systems.

The remote system extracts and reconstructs the stolen information.

---

## Common ICMP Exfiltration Techniques

### Echo Request Tunneling

Uses:

```text
ICMP Type 8
```

Data embedded inside ping requests.

---

### Echo Reply Tunneling

Uses:

```text
ICMP Type 0
```

Data embedded inside responses.

---

### Custom ICMP Types

Uses uncommon:

- Types
- Codes

to avoid simple detection signatures.

---

### Fragmentation

Large files split across multiple ICMP packets.

---

### Encryption & Encoding

Data hidden using:

- Base64
- Hexadecimal
- Encryption

---

# Indicators of ICMP Exfiltration

### Persistent ICMP Sessions

Frequent communication with an external host.

---

### Large Payload Sizes

Normal ping payloads are relatively small.

Suspicious examples:

```text
data.len > 64
```

---

### High Entropy Payloads

Random-looking data often indicates encoding or encryption.

---

### Regular Timing

Packets sent at fixed intervals.

Examples:

```text
Every 30 seconds
Every 60 seconds
```

---

### Unusual ICMP Types

Examples:

| Type | Description |
|--------|------------|
| 8 | Echo Request |
| 0 | Echo Reply |
| 13 | Timestamp Request |
| 14 | Timestamp Reply |

Frequent timestamp traffic may warrant investigation.

---

### Fragmented ICMP Packets

Large transfers often generate fragmented traffic.

---

# ICMP Indicators in Wireshark

## Display ICMP Traffic

```wireshark
icmp
```

---

## Detect Large Payloads

```wireshark
data.len > 64 and icmp
```

---

## Identify High Volumes

Look for:

- Numerous echo requests
- Repeated external communication
- Consistent packet sizes

---

## Detect Fragmentation

Look for:

```text
IP fragments
Reassembled packets
```

associated with ICMP traffic.

---

# Quick Comparison

| Technique | Common Protocol | Why Attackers Use It | Key Indicators |
|------------|----------------|----------------------|----------------|
| DNS | UDP/TCP 53 | Commonly allowed and trusted | Long domains, high entropy, TXT records |
| FTP | TCP 21 | Efficient file transfer | STOR commands, large uploads |
| HTTP | TCP 80/443 | Blends with web traffic | Large POST requests, multipart uploads |
| ICMP | Network Layer | Often lightly inspected | Large payloads, beaconing, fragmentation |

---

# Key Takeaways

- DNS, FTP, HTTP, and ICMP are commonly abused for data exfiltration.
- DNS tunneling hides data inside DNS queries and responses.
- FTP provides straightforward file transfer and often exposes credentials in plaintext.
- HTTP exfiltration blends into normal web traffic using POST requests, cloud services, and encrypted channels.
- ICMP tunneling abuses diagnostic traffic by embedding data into packet payloads.
- Effective detection relies on correlating host logs, network logs, proxy logs, firewall logs, DNS logs, and packet captures.
- Analysts should focus on abnormal volumes, unusual destinations, encoded data patterns, beaconing behaviour, and suspicious transfer mechanisms when investigating potential exfiltration activity.
