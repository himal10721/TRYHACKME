# Learning Objections

- Familiarise with the concept of SOC alert.
- Explore alert fields, statuses and classification
- Learn how to perform alert triage as an L1 analyst
- Prepare for SOC sim and SAL1 certification
  
## From Events to Alerts

First and event must occur like login, process launchm or file download. Then OS, firewall or cloud provider must log the event and after that all system logs must be shipped to a security solution like SIEM or EDR. 

SOC can receive thousands of logs per day where most of them are expected but some require attention.

**Alert Management Platforms**

- SIEM (Security Information and Event Management) Systems: Splunk ES, Elastic have solid alert management capabilities and are a perfect choice for most SOC teams

- EDR (Endpoint Detection and Response) or NDR: MS defender or CrowdStrike have their own alert dashboards, it is preferred to use SIEM or SOAR.

- SOAR (Security Orchestration, AUtomation and Response) System: Splunk SOAR or Cortex Soar can be used to aggregate and centralise alerts from multiple solutions

- ITSM (IT Service Management): Jira, TheHive may have a custom ticket management setup using a solution


## L1 Role in Alert Triage

SOC L1 analysts are the first line of defense. They review the alerts, distinguish bad from good and notify L2 analyst in case of a real threat. **SOC L2 Analyst** receive the alerts escalated by L1 and perform a deeper analysis and remediation. **SOC engineers** ensures that the alert contains enough info for efficient alert triage and lastly **SOC managers** track the speed and quality of alert triage to ensure that real attacks wont be missed

## Alert properties

1. **Alert Time** : shows alert creation time. Alert usually tiggers a few minutes after the actual event. Example: Alert time: March 21, 15:35

2. **Alert Name**: Provides summary of what happened based on the detection rules name. *Example: Unusual login location, email marked as phishing, etc*

3. **Alert Severity**: Defines urgency of the alert. *Example: Low, high, medium*

4. **Alert Status**:Informs if somebody is working on the alert or if the triage is done. *Example: New assigned, progress, pending*

5. **Alert Verdict**: Also known as alert classification, explains if the alert is a real threat or noise. *Example: True Positive / Real Threat* 

6. **Alert Assignee**: Shows the analyst that was assigned or assigned themselves to review the alert. 

7. **Alert Description**: Explains what the alert is about, usually in three sections on the right. 

8. **Alert Fields**: Provides SOC analyst comments and values on which alert was triggered. *Example: Affected Hostname, Entered Commandline and manymore*

## Alert Prioritisation

**Picking the right alert**

1. **Filtering the alerts**: Making sure not to take alert that other analysts have already reviewed or that is already being investigated by teammates. Only take unseen, unresolved alerts.
2. **Sort by Severity**: In descending order. First critical, then high, medium. 
3. **Sort by time**: Start with oldest alerts and end with newest ones. The idea is that if both alerts are about two breaches, there is high possibility that the hacker is dumping data.

## Alert Triage 

The alert review by SOC analysts can also be called alert triage, alert handling, alert processing, alert investigation or alert analysis.

### Initial Actions
The initial steps are designed so that the analyst can take ownership of the assigned alert and avoid interfering with alerts being handled by other analysts, and confirm that you are fully prepared to proceed with the detailed investigation. Can be achieved by first assigning the alert to self, moving it to **In progress**, and then familiarising oneself with the alert details like its name, desc and key indicators.

### Investigation
For L1 analysts, workbooks are developed and instruction on how to investigate the specific category of alerts are written there. If workbooks are not available, then the following can be done

i. Understand who is under threat like the affected user, hostname, cloud, network or website
ii. Note the action described in the alert, like whether it was a suspicious login, malware or phishing
iii. Review surrounding events, looking for suspicious actions shortly after or before the alert
iv. Use threat intelligence platform or other available resources to verify thoughts

### Final Actions
Decisions here determine whether a potential cyberattack is found or missed. 

---

# SOC L1 Alert Reporting

Learning Objectives:
- Understanding the need for SOC alert reporting and escalation
- Learning how to write alert coment or case reports properly
- Explore escalation methods and communication best practices
- Applying the knowledge to triage alerts 

---
## Alert Funnel
L1 analysts receive the alerts in a SIEM, EDR or a ticket management platfroms. 

### Alert Reporting 
Before escalating it to seniors, reporting is necessary. Depending on the team standards and alert severity, instead of a short alert comment, documenting the investigation in detail is required, specially for True Positives.

### Alert Escalation
If true positive alert requires additional actions or deeper investigations, escalating it to L2 is necessary. Thats when the alert reports become handy.

### Communication
Might need to communicate with other departments during or after the analysis.

---
## Repot Guide
Report should be written for the following purposes:
- To provide context for escalation
- Save fidning for records
- Improve investigation skill

### Report Format
While writing a report, the format of 5 Ws must be followed, i.e:
**Who**: which user logs in, runs the command, or downloads the file
**What**: What exact actions or event sequence was performed
**When**: WHen that happened
**Where**: which device, IP, or website was involved in the alert
**Why**: The most import W, the reason

---
## Escalation Guide
Escalating the alerts if:
1. The alert is an indicator of a major cyberattack requiring deeper investigation or DFIR
2. Malware removal, host isolation or password resets are required
3. Communicate with customers, partners, management, or law enforcement is required.
4. In case of not understanding the alert at all!

### Escalation Steps
To escalate the alert, need to reassign the alert to the L2 on shift and ping them in corporate chat or in person.

### Requesting L2 Support
Simply pinging the L2 analyst

---
## SOC Communication
Cases:
- When a critical alert needs to be escalated, but L2 is unavailable and does not respond for 30 minutes, need to ensure where to find the emergency contacts. First calling L2, then L3 and finally the manager.
- Alert about slack/teams account comrpomise requires validation of the login with the affected user. DONOT contact the user through the breached chat
- When an overwhelming number of alerts are generated during short period of time, some of which are critical, alerts need to be priortized and infromed to L2 on shift about the situation
- Donot skip the alert, investigating what can be ivestigated and reporting the issue to L2 on shift or SOC engineer

---
# SOC Workbooks and Lookups

## Assets & Identities
**Identity Inventory**: is a catalogue of corporate employees (user accounts), services (machine accounts), and their details like privileges, contacts and roles within the company. 

### Sources of Identities

Active Directory: On-prem AD, Entra ID. AD is an identity database
SSO Provides: Okta, google workspace
HR Systems: BambooHR, SAP, HiBob
Custom Solution: CSV or Excel sheets

### Asset Inventory
Asset inventory is a lost of all computing resources within an organisation's IT environment. Can also refer to software, hardware or employees.

---
## Network Diagrams
Network point of view is also necesssary to investigate a chain of alerts based on firewall log. 

---
## Workbooks Theory
**SOC Workbooks**: also called playbook, runbook or workflow is a structured document that defines the steps required to investigate and remediate specific threats efficiently and consistently. 

The following step in correct order would garauntee high-quality alert triage and eliminate cases where the verdict is made without enough evidence:
i. Enrichment: Using threat intelligence and identity inventory to get information about the affected user
ii. Investigation: Using the gathered data and SIEM logs 
iii. Escalation: Escalating the alert to L2 or communicate the login with the user if necessary


# SOC Metrics and Objectives

## Core metrics
Main goal of SOC is to protect CIA of the organisation's digital assets. 

Four metrics are:
Alert Count(AC): Overall load of SOC analysts
False Positive Rate (FPR): Level of noise in the alerts
Alert Escalation Rate (AER): Experience of L1 analysts
Threat Detection Rate (TDR): Reliability of the SOC team

## Triage Metrics
The requirements to ensure a quick detection and remediation of the threat are commonly grouped into a service level agreement.

## Improving Metrics
