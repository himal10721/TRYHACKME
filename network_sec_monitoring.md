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
