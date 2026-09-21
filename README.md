# aws-soc-home-lab
AWS-based SOC home lab for security monitoring, Windows endpoint detection, log analysis, and incident investigation using Wazuh, Sysmon, Kali Linux, Nmap, and MITRE ATT&amp;CK.


# AWS SOC Home Lab – Security Monitoring & Log Analysis

## Overview

Built an AWS-based Security Operations Center (SOC) home lab to practice security monitoring, endpoint detection, log analysis, alert investigation, network reconnaissance detection, and incident response.

The lab simulates a small enterprise environment where a Windows endpoint generates security telemetry that is collected and analyzed using Wazuh.

Controlled security testing is performed from Kali Linux against the Windows endpoint.

---

## Objectives

* Build a cloud-based SOC monitoring environment using AWS
* Deploy and configure Wazuh SIEM
* Deploy a Windows endpoint with Sysmon
* Collect Windows Security and Sysmon telemetry
* Configure the Wazuh Agent
* Detect failed authentication attempts
* Analyze Windows Event IDs 4624 and 4625
* Perform controlled Nmap reconnaissance
* Investigate security alerts
* Analyze network activity
* Map detected activity to MITRE ATT&CK
* Document investigation and response procedures

---

## Lab Architecture

```text
                         Internet
                            |
                            |
                     +------+------+
                     |   Kali      |
                     |   Linux     |
                     |  Attacker   |
                     +------+------+
                            |
                 Controlled Testing
                            |
                            v
              +-------------------------+
              |      AWS VPC             |
              |     10.10.0.0/16         |
              |                           |
              |  +-------------------+   |
              |  | Windows EC2       |   |
              |  |                   |   |
              |  | Windows Server    |   |
              |  | Sysmon            |   |
              |  | Wazuh Agent       |   |
              |  +---------+---------+   |
              |            |              |
              |            | Logs         |
              |            v              |
              |  +-------------------+   |
              |  | Ubuntu EC2        |   |
              |  |                   |   |
              |  | Wazuh Manager     |   |
              |  | Wazuh Indexer     |   |
              |  | Wazuh Dashboard   |   |
              |  +-------------------+   |
              |                           |
              +---------------------------+
```

---

## Technologies Used

| Technology           | Purpose                      |
| -------------------- | ---------------------------- |
| AWS EC2              | Cloud infrastructure         |
| AWS VPC              | Network isolation            |
| Ubuntu               | Wazuh server                 |
| Windows              | Monitored endpoint           |
| Wazuh                | SIEM and security monitoring |
| Wazuh Agent          | Endpoint log collection      |
| Sysmon               | Windows endpoint telemetry   |
| Kali Linux           | Security testing             |
| Nmap                 | Network reconnaissance       |
| Wireshark            | Network traffic analysis     |
| Windows Event Viewer | Event investigation          |
| MITRE ATT&CK         | Threat behavior mapping      |

---

## AWS Infrastructure

### VPC

```text
VPC Name: SOC-Lab-VPC
CIDR: 10.10.0.0/16
```

### Subnet

```text
Subnet: SOC-Public-Subnet
CIDR: 10.10.1.0/24
```

### Internet Gateway

```text
SOC-Lab-IGW
```

### Route Table

```text
SOC-Public-RT

0.0.0.0/0 → Internet Gateway
```

### EC2 Instances

#### Wazuh Server

```text
OS: Ubuntu
Role: Wazuh Manager + Indexer + Dashboard
```

#### Windows Endpoint

```text
OS: Windows
Role: Monitored endpoint
Components:
- Wazuh Agent
- Sysmon
- Windows Event Logs
```

---

## Security Monitoring

The Windows endpoint is configured to generate telemetry including:

* Authentication events
* Process creation
* Network connections
* System activity
* Windows Security Events
* Sysmon events

These events are forwarded to the Wazuh server for analysis.

---

# Detection Scenario 1 – Failed Login

## Objective

Generate controlled failed authentication attempts against the Windows endpoint and investigate the resulting security events.

### Windows Event ID

```text
4625 – An account failed to log on
```

### Investigation

The alert is analyzed using:

* Source IP
* Username
* Logon type
* Timestamp
* Number of attempts
* Target endpoint

### SOC Workflow

```text
Failed Login
     |
     v
Wazuh Alert
     |
     v
Analyze Event ID 4625
     |
     v
Identify Source
     |
     v
Determine Frequency
     |
     v
Investigate
     |
     v
Document Findings
```

---

# Detection Scenario 2 – Successful Login

Windows Event ID:

```text
4624 – An account was successfully logged on
```

Successful authentication events are reviewed and correlated with failed authentication attempts.

Example investigation:

```text
4625
4625
4625
4624
```

A sequence like this may indicate repeated authentication failures followed by a successful login and should be investigated.

---

# Detection Scenario 3 – Network Reconnaissance

Kali Linux is used to perform controlled network scanning against the lab Windows endpoint.

Example:

```bash
nmap -sS <WINDOWS-IP>
```

The resulting network activity is monitored through Wazuh and analyzed alongside Windows/Sysmon telemetry.

---

# Sysmon Monitoring

Sysmon is installed on the Windows endpoint to provide detailed endpoint telemetry.

Important Sysmon events include:

| Event ID | Activity           |
| -------- | ------------------ |
| 1        | Process Creation   |
| 3        | Network Connection |
| 7        | Image/DLL Loading  |
| 11       | File Creation      |
| 22       | DNS Query          |

These events provide additional visibility beyond standard Windows Security logs.

---

# Wazuh

Wazuh is used as the central security monitoring platform.

Responsibilities include:

* Log collection
* Event analysis
* Alert generation
* Event correlation
* Security monitoring
* Endpoint visibility
* Investigation

The Wazuh Dashboard is used to visualize and investigate security events.

---

# Investigation Process

For each alert, the following investigation methodology is used:

### 1. Identify

Determine:

* Source
* Destination
* User
* Host
* Timestamp
* Event ID

### 2. Analyze

Review:

* Event details
* Process information
* Authentication information
* Network activity
* Related events

### 3. Correlate

Correlate multiple events to determine whether activity represents:

* Normal behavior
* Suspicious activity
* Potential attack activity

### 4. Map

Map relevant activity to MITRE ATT&CK techniques.

### 5. Respond

Document:

* Finding
* Impact
* Evidence
* Recommended action

---

# MITRE ATT&CK Mapping

Example mappings used during the lab include:

| Activity                          | MITRE ATT&CK |
| --------------------------------- | ------------ |
| Network Service Scanning          | T1046        |
| Valid Accounts                    | T1078        |
| Brute Force                       | T1110        |
| Command and Scripting Interpreter | T1059        |

Mappings are based on the observed behavior and investigation evidence.

---

# Security Controls

The AWS environment uses security groups to restrict access.

Examples:

```text
SSH 22
HTTPS 443
RDP 3389
Wazuh Agent 1514
Wazuh Enrollment 1515
```

Administrative access is restricted to the lab administrator's public IP where appropriate.

---

# Skills Demonstrated

### SOC Operations

* Security Monitoring
* Alert Triage
* Log Analysis
* Event Correlation
* Incident Investigation
* IOC Analysis
* Incident Documentation
* MITRE ATT&CK Mapping

### Endpoint Security

* Windows Security Events
* Sysmon
* Wazuh Agent
* Process Monitoring
* Network Connection Monitoring
* Authentication Monitoring

### Networking

* TCP/IP
* Network Scanning
* Nmap
* Wireshark
* AWS VPC
* Security Groups

### Cloud

* AWS EC2
* AWS VPC
* Internet Gateway
* Route Tables
* Security Groups

---

# Lessons Learned

Through this project I gained practical experience in:

* Building a SOC environment in AWS
* Deploying a SIEM
* Configuring endpoint telemetry
* Investigating authentication events
* Detecting network reconnaissance
* Understanding Windows security logs
* Correlating endpoint and network activity
* Applying MITRE ATT&CK to security investigations
* Documenting SOC investigation workflows

---

# Disclaimer

This project is a controlled security lab created for educational and defensive security purposes.

All testing was performed only against systems owned or controlled by the lab environment.
