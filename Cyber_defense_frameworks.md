# Cyber kill chain

The cyber kill chain helps us understand and protext against ransomware attacks, security breaches as well as **Advanced Persistent Threats** (APTs). 

Cyber kill chain can be used to assess network and system security by identifying missing security controls and closing certain security gaps based on a company's infras.

By understanding kill chain as a SOC analyst, security researcher, threat hunter or incident response, it will be easier to recognize the intrusion attempts and understand the intruder's goals and objectives


## 1. Reconnaissance
Reconnaissance is the research and planning phase of an attack against a system or victim. Adversaries use this phase to gather info about their target to inform their next steps. This info can include infra details, employee data , business processes and exposed techs. 

A valuable piece of recon is OSINT (Open-Source Intelligence). With OSINT, adversaries gather insights about targets through publicly available info. 

### Reconnaissance Types
i. Passive Recon: Involves having no direct interaction with target. Includes WHOIS, lookups, social media scraping, or reviewing breach data.

ii. Active Recon: Involves direct contact with the target with activites such as social engineering, port scanning, etc.


**Email harsvesting** is the process of obtaining email addresses from public, paid or free services. An attacker can use email-address harvesting for phishing attack. Some of the tools used in email harvesting are theHarvester, Hunter.io, OSINT Framework.

---
## 2. Weaponization
After a successful recon stage, next would be turning raw info into actionable attack tools through crafting malware, exploits into a payload. 

Malware can be created through automated tools or can be purchased through darkweb. 

---
## 3. Delivery
The method of transmitting the payload or the malware onto the target environment.

i. Phishing email
ii. Publicly dropping USB

**Watering hole attacks** are those attacks that are targeted and designed to aim at a aspecific group of people by compromising the website they are visiting.

---
## 4. Exploitation
This is the moment when the attacker's code executes on the target, taking advantage of a known vulnerability. 

In this phase the attacker can utilise a number of techniques to gain access such as:
* Malicious macro execution: could be delivered through a phishing email
* Zero-day exploits
* Known CVEs

Signs of exploitation to look out for include:
* Unexpected process spwans
* Registry changes or new services created
* Suspicious command-line arguments found in system logs

---
## 5. Installation
The backdoor lets an attacker bypass security measures and hide the access. A backdoor is also known as an access point.

To connect back to the system, if the attacker looses the connection or if the initial access is detected and removed, they create a backdoor and will let the attacker access the system he compromised in the past. 

The persistence can be achieved through:
* Installing a web shell on the web server.
* Installing a backdoor using meterpreter to install a backdoor on the victim's machine.
* Creating or modifying windows services. Technique is known as T1543.003 on MITRE ATT&CK.
* Adding the entry to the "run keys" for the malicious payload in the Registry or the startup folder. 

---
## 6. Command & Control
After getting persistence and executing the malware on the victim's machine, C2 channel will be opened through the malware to remotely control and manipulate the victim. The compromised endpoint will communicate with the attackers on an external server to establish C2 channel.

Some of the common C2 channels used by adversaries are:
* HTTP on Port 80 and HTTPS on port 443, where this type of beaconing blends the malicious traffic with the legitimate traffic
* DNS, where the infected machine makes constant DNS requests to the DNS server that belongs to an attacker, also known as DNS tunneling

---
## 7. Actions on Objections (Exfiltration)
After the 6 phases, the attacker can achieve the following:
* Collect creds from users
* Privilege escalation
* Internal reconn
* Lateral movement through the company's environment
* Deleting the backups
* Overwrite or corrupt data

**Shadow Copy** is the technology included in microsoft windows that can cerat backup copies or snapshots of files or volumes on the computer


--------

# Unified Kill chain
The Unified Kill Chain is a framework which establishes the phases of an attack, and a means of identifying and mitigating risk to IT assets.

"Kill chain" is a term used to expkain the various stages of an attacker.
**Kill chain** is used to describe the methodology/path attackers such as hackers or APTs use to approach and intrude a target.



## What is threat modelling ?

A series of steps to ultimately improve the security of a system. Threat modelling is about identifying risks. 

i. Identifying what systems an apps need to be secured and what function they serve in the environment.
ii. Assessing what vulnerabilities and weaknesses these systems and applications may have and how they could potentially be exploited
iii. Crafting a plan of action to secure these systems and applications from the vulnerabilities highlighted.
iv. Putting in policies to prevent these vulnerabilities from occuring again where possible

## What is Unified Kill chain ?

The UKC amis to complement other cyber kill chain frameworks.
The UKC states that there are 18 phases to an attack: everything from reconn to data exfiltration and understanding an attacker's motive.

## Goal: In (Initial Foothold)
The main focus for an attacker is to gain access to a system or networked environment. An attacker can employ numerous tactics to investigate the system for potential vulnerabilities that can be exploited to gain a foothold in the system.Example, a common tactic is the use of reconnaissance against a system to discover potential attacker vectors. The different phases of UKC are:

### Reconnaissance (MITRE Tactic TA0043)
Gathering info relating to the target. Eithe through active recon or passive recon. 

### Weaponization (MITRE Tactic TA0001)
Describes the adversary setting up the necessary infrastructure to perform the attack. Could be setting up a command and control server, or a system capable of catching reverse shells and delivery payloads to the system.

### Social Engineering (MITRE Tactic TA0001)
UKC dedscribes this as techniques that an attacker can employ to manipulate employees to perform actions that can aid in the adversaries attack. 

### Exploitation (MITRE Tactic TA0002)
This phases in UKC describes how an attacker takes advantage of vulnerabilities present in a system. The UKC defines "exploitation" as abuse of vulnerabilities to perform code execution

### Persistence (Mitre Tactic TA0003)
This phase of UKC is short and simple. UKC describes the techniques an adversary uses to maintain access to a system they have gained an initial foothold on. 

### Defence Evasion (MITRE Tactic TA0005)
This phase in the UKC is one of the more valuable phases of the UKC. This phase is used to understand the techniques an adversary uses to evade defensive measures put in place in the system or network, could be:
* WAF
* Network Firewalls
* anti-virus systems
* IDS

### Command& Control (MITRE Tactic TA0011)
C2 phase of the UKC combines the efforts an adversary made during "weaponization" stage of the UKC to establish communications between the adversary and the target system. The adversary can:
* Execute commands
* Steal data, credentials and other info
* Use the controlled server to pivot to other systems on the network

### Pivoting (MITRE Tactic TA0008)
Pivoting is the technique an adversary uses to reach other systems within a network that are not otherwise accessible. There are aftern many systems in a network that are not directly reachbale and often contain valuable data or have weaker security. 

---
## Goal: Through (Network Propagation)
The "Through" goal follows a successful foothold being established on the target network. If defensive controls prevent the attacker from acheiving their objective, they will seek to gain additional access and privileges to systems. The attacker would set up a base on one of the systems to act as their pivot point and use it to gather information about the internal network. 

### Pivoting (MITRE Tactic TA0008)
Once the attacker has access to the system, they would use it as their staging site and a tunnel between their command operations and the victims network.

### Discovery (MITRE Tactive TA0007)
Uncover info about the system and the network it is connected to. In this stage, the knowledge base would be built from the active user accounts, the permission granted, apps and software in use, web browser activity, files, directories and network shares, and system configuration.

### Privilege Escalation (MITRE Tactic TA004)
The adversary would try to gain more prominent permissions within the pivot system. They would leverage the information on the acocunts present with the vulnerabilities and misconfigurations found to elevate their access to one of the foloowing superior levels:
* ROOT privileges
* Admins
* An account wit admin-like access

### Execution (MITRE Tactic TA0002)
This phase is when the malware is deployed. Remote trojans, C2 scripts, malicious links and scheduled tasks are deployed and created to facilitate a recurring presence on the system and uphold their persistence.

### Credential Access (Mitre Tactic TA0006)
Working hand in hand with the Priv Escalation stage, the attacker can attempt to steal account names and passwords through various methods, including keylogging credential dumping. This makes them harder to detect during their attack as they would be using legitimate cerdentials

### Lateral Movement (MITRE Tactic TA0008)
With the credentials and elevated privs, the attacker would seek to move thru the network and jump onto other targeted systems to achieve their objectives.

---
## Goal: Action on Objective
These are the final phases of the kill chain. These goals are usually geared towards compromising the CIA triad. The following are the phases:

### Collection (MITRE Tactic TA0009)
After all the hunting for access and assets, the attacker will be seeking to gather all the valuable data of interest. This compromises the confidentiality of the data and would lead to the next attack stage, i.e exfiltration.

### Exfiltation (MITRE Tactic TA0010)
To elevate their compromise, the attacker would seek to steal data, which would be packaged using encryption measures and compression to avoid any detection. The C2 channel and tunnel deployed in the earlier phases will come in handy during this process.

### Impact (MITRE Tactic TA0040)
The goal would be to disrupt business and operational processes and may involve removing account access, disk wipes, and data encryption such as ransomware, defacement and DOS.

### Objectives
The attacker can now seek to achieve their strategic goal for the attack. If the attack was financially motivated, they might seek to encypt files and systems with ransomware and ask for payment to release the data. 

---
# MTRE  
 MITRE is researching a known exploit or exploring various tactics used by attackers. 

## ATT&CK Framework
The attack framework is a "globally-accessible" knowledge base of adversary tactics and techniques based on real-world observations. 

### ATT&CK Evolution
The ATT&CK framework has evolved significantly over the years. Focused initially on the Windoes platform, it has expanded to cover a range of environments, including macOS, Linux, cloud platforms, and more, under the Enterprise matrix. Additionally, specialized frameworks exist for mobile and Industrial Control Systems (ICS). 

### ATT&CK Matrix
It is a visual representation of all the tactics and techniques that exist within the framework. 


---
## ATT&CK in Operation
ATT&CK is and the kind of intelligence it contains. 
### Why ATT&CK Matters
Provides cyber security professionals and organizations with a standard and consistent language for describing adversary behavior. 

**Threat Intelligence and Defense**
ATT&CK also helps bridge the gap between threat Intel and defensive operations. A threat report might describe what an atatcker did, but not how to turn that info into a usable detection measures. 

---
## Cyber Analytics Repository (CAR)

MITRE defines the Cyber Analytics Repository (CAR) as "A knowledge base of analytics developed by MITRE based on the MITRE ATT&CK adversary model. CAR Ddefines a data model that is leveraged in its pseudocode representaions, but also includes implementation directly targeted at specific tools.

CAR is a collection of ready-made detection analytics built around ATT&CK. Each analytics decribes how to detect an adversary's behavior. This is key because it allows you to identify the patterns you should look for as a defender. CAR also provides example queries for common industry tools such as Splunk, a defender can translate TTPs into real detections.

---
## MITRE D3FEND Framework
With MITRE ATT&CK,  attacks are carried out, but wit hMITRE D3FEND, we can discover how to stop them. 

D3FEND (Detection, Denial, and Disruption Framework Empowering Network Defense) is a strucutred framework that maps out defensive techniques and establishes a common language for describing how security controls work. D3FEND comes with its own matrix, which is broken doen into seven tactics, each with its associated techniques and IDs. 

---
## Other MITRE Projects
Offers several other projects designed to help cyber security professionals strengthen their skills, test their defenses and outsmart attackers. In this task, we briefly explore these tools and how they can support an individuals growth in the field.

### Emulation Plans
### Caldera
