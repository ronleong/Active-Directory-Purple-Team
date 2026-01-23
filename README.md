# Active Directory Purple Team – End-to-End Attack & Defense Simulation

## Overview

This project demonstrates a **full Active Directory attack lifecycle**, followed by **detection, response, and hardening**, using a realistic on-premises Active Directory lab.

The objective is not only exploitation, but to show **how attacks are detected and stopped**, combining red team techniques with blue team engineering — a **Purple Team approach**.

The lab simulates how a real enterprise Active Directory environment is:
- Misconfigured
- Attacked step by step
- Monitored using a SIEM
- Defended through automated response

---

## Project Scope

This project covers:

- Initial network access via name-resolution poisoning  
- Credential harvesting and offline password cracking  
- Active Directory abuse (Kerberos, GPP, service accounts)  
- Privilege escalation to `NT AUTHORITY\SYSTEM` and Domain Admin  
- Domain persistence via Kerberos Golden Ticket  
- Centralized logging, detection, and alerting  
- Automated defensive controls using Wazuh  

All attack actions are paired with **visibility and detection considerations**.

---

## Lab Environment

### Infrastructure
- Windows Server 2019 (Domain Controller)  
- Windows Client (Domain-joined)  
- Kali Linux (Attacker)  
- Oracle VirtualBox (Isolated lab network)  

### Monitoring & Defense
- Sysmon (Endpoint telemetry)  
- Wazuh (SIEM and detection rules)  
- Group Policy Objects (Hardening and control)  

This environment ensures attacks generate **real Windows and Active Directory telemetry**, not simulated output.

---

## Attack & Defense Flow (High-Level)

### Phase 0 — Lab Setup & Visibility
- Network connectivity validation  
- Domain join confirmation  
- Sysmon deployment  
- Wazuh agent and manager setup  

**Purpose:** Ensure all attack activity is observable before exploitation begins.

---

### Phase 1 — Initial Recon & Credential Access
- LLMNR/NBT-NS poisoning  
- NTLMv2 hash capture  
- Offline password cracking  
- Valid domain credentials obtained  

**Result:** Authenticated domain access without exploiting software vulnerabilities.

---

### Phase 2 — Active Directory Abuse
- Service Principal Name (SPN) discovery  
- Kerberoasting attack  
- Service account compromise  
- Group Policy Preferences (GPP) credential recovery  

**Result:** Privilege escalation through Active Directory misconfiguration.

---

### Phase 3 — Lateral Movement & Privilege Escalation
- Internal service discovery (database access)  
- Firewall-blocked attempt and evasion via allowed service ports  
- Privilege escalation to `NT AUTHORITY\SYSTEM` using token abuse  

**Result:** Full control over compromised hosts.

---

### Phase 4 — Domain Dominance & Persistence
- Domain Admin hash extraction  
- DPAPI master key recovery  
- KRBTGT key compromise  
- Kerberos Golden Ticket creation  

**Result:** Persistent, unrestricted access to the Active Directory domain.

---

### Phase 5 — Detection, Response & Hardening (Purple Team)
- Centralized logging and alert correlation  
- Custom Wazuh detection rules  
- SOC-style alerting on malicious behavior  
- Group Policy enforcement to reduce attack surface  

**Result:** Attacks are detected, logged, and actively prevented.

---

## Why This Project Matters

Many Active Directory labs stop at exploitation.

This project goes further by:
- Explaining **why each attack works**
- Demonstrating **what telemetry is generated**
- Showing **how defenders detect and respond**
- Including failed attempts and troubleshooting for realism  

This reflects **real enterprise security operations**, not CTF-style shortcuts.

📄 A detailed technical PDF expands each phase with screenshots, commands, detection logic, and analysis.

