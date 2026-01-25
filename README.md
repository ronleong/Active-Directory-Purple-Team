# Active Directory Purple Team Project

## Overview
This project demonstrates a complete Active Directory attack lifecycle paired with real time detection, response, and hardening in a realistic on-premises AD environment.

The lab simulates how enterprise Active Directory environments are:
- Misconfigured and exploited
- Monitored using centralized logging (SIEM)
- Defended through automated response mechanisms

---

## Tools Used

| Category              | Tools                                           |
|-----------------------|-------------------------------------------------|
| Reconnaissance        | Nmap, Responder                                 |
| Credential Access     | John the Ripper                                 |
| AD Exploitation       | Impacket Suite                                  |
| Privilege Escalation  | GodPotato                                       |
| Monitoring            | Sysmon, Wazuh, Windows Event Logs               |
| Infrastructure        | Oracle VirtualBox, Windows Server 2019, Window 10 pro          |

---

## Skills Demonstrated

### Offensive Security
- Active Directory enumeration and exploitation
- Kerberos protocol abuse (Kerberoasting, Golden Tickets)
- Credential access and lateral movement
- Windows privilege escalation techniques

### Defensive Security
- SIEM deployment and rule engineering
- Threat hunting and log analysis
- Incident response automation (SOAR)

### Purple Team Integration
- Attack–defend feedback loops
- Detection gap analysis
- Security control validation

---

## Project Scope

### Offensive Operations
- Initial network access via LLMNR poisoning
- Credential harvesting and offline password cracking
- Active Directory abuse (Kerberoasting, SPN enumeration)
- Privilege escalation to `NT AUTHORITY\SYSTEM` and Domain Admin
- Domain persistence via Kerberos Golden Ticket

### Defensive Operations
- Centralized logging and threat detection
- Custom SIEM alerting with Wazuh
- Automated defensive controls and incident response

---

## Lab Environment

### Infrastructure

| Component            | Purpose                          |
|----------------------|----------------------------------|
| Windows Server 2019  | Domain Controller                |
| Kali Linux           | Attacker machine                 |
| Oracle VirtualBox    | Isolated virtual network         |

### Monitoring & Defense Stack
- **Sysmon** – Advanced endpoint telemetry collection
- **Wazuh** – Open-source SIEM with custom detection rules

This setup generates authentic Windows and Active Directory telemetry, not simulated output.

---

## Attack & Defense Flow

### Phase 0: Lab Setup & Visibility
**Objective:** Ensure all attack activity is observable before exploitation begins

- Network connectivity validation
- Sysmon deployment on the Domain Controller
- Wazuh agent and manager configuration

**Result:** Full visibility pipeline established ✓

---

### Phase 1: Initial Reconnaissance & Credential Access
**Objective:** Gain authenticated domain access without exploiting software vulnerabilities

**Attack Steps:**
- LLMNR poisoning with Responder
- NTLMv2 hash capture from network traffic
- Offline password cracking with John the Ripper

**Result:** Valid domain credentials obtained
**Detection:** Network authentication anomalies, unusual DNS/LLMNR traffic patterns

---

### Phase 2: Active Directory Exploitation
**Objective:** Escalate privileges through AD misconfigurations

**Attack Steps:**
- Service Principal Name (SPN) enumeration
- Kerberoasting attack against service accounts
- Service account compromise

**Result:** Elevated privileges through Kerberos ticket exploitation
**Detection:**
- **Event ID 4769:** Kerberos Service Ticket Operations (Indicators of TGS-REQ)
- Weak encryption types (RC4) requested

---

### Phase 3: Lateral Movement & Privilege Escalation
**Objective:** Gain SYSTEM-level access on compromised hosts

**Attack Steps:**
- Internal service discovery (MSSQL on TCP/1433)
- Remote Command Execution via `xp_cmdshell`
- GodPotato exploit to abuse `SeImpersonatePrivilege`
- Token impersonation abuse for privilege escalation to `NT AUTHORITY\SYSTEM`

**Result:** Full control over domain-joined systems
**Detection:**
- **Event ID 4624:** Successful Network Logon (Logon Type 3)
- **Event ID 4688:** Suspicious Process Creation (`sqlservr.exe` spawning `cmd.exe`)
- **Event ID 4672:** Special Privileges Assigned (Administrator Login)

---

### Phase 4: Domain Dominance & Persistence
**Objective:** Achieve unrestricted, persistent access to the entire AD domain

**Attack Steps:**
- Domain Admin hash extraction via `secretsdump` (DCSync)
- KRBTGT hash compromise
- Kerberos Golden Ticket creation

**Result:** Persistent domain-wide access independent of password changes
**Detection:**
- **Event ID 4662:** Operation on an Object (Directory Service Access - DCSync)
- **Event ID 4624:** Account Logons with unusual GUIDs
- Abnormal Kerberos ticket requests (TGTs with 10-year validity)

---

### Phase 5: Detection & Automated Response
**Objective:** Demonstrate how attacks are detected, alerted, and actively blocked

**Defense Implementation:**
- Centralized log aggregation and correlation
- Custom Wazuh detection rules for AD-specific attacks
- **Automated Active Response:** Configured Wazuh to instantly block the attacker's IP upon detecting Credential Dumping

**Result:** Real-time threat detection with automated blocking
**Metrics:** MTTR (Mean Time To Respond) <1 second

---

## Why This Project Matters
Most Active Directory labs focus solely on exploitation. This project goes further by:

- Explaining the *why* behind each attack vector
- Demonstrating defensive telemetry generated by each action
- Showing realistic detection and response workflows

This reflects real enterprise security operations, not CTF-style shortcuts.
